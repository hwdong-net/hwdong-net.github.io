---
layout:       post
title:        "A PyTorch implementation of deep Q-learning Network (DQN) for Atari games"
subtitle:     "A PyTorch implementation of deep Q-learning Network (DQN)  for Atari games"
date:         2020-01-21 13:28:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - C
---

Deep Q-learning Network (DQN) can be used to train an agent to play Atari games:


![](https://miro.medium.com/max/200/1*KZm_N60yUP_Kw3L9tfDfGQ.gif)

We often use continuous frames to represent an state of the enviroment. DQN use replay mempry to store ecxperiences which are used to train DQN algorithm.

Here is the class to represent replay mempry:

````python
from collections import deque
import numpy as np
import torch
import random

class ReplayMemory(object):
    def __init__(self,n_history,h,w,capacity=1000000):        
        self.n_history = n_history
        self.n_history_plus = self.n_history+1  
        self.history = np.zeros([n_history+1, h,w], dtype=np.uint8)  
        self.capacity= capacity
        self.memory = deque(maxlen=capacity)  
    
    def push(self, frame, action, reward, done):
        if len(self.memory)==0:
            for i in range(self.n_history):
                self.history[i, :, :] = frame
        else:  
            self.history[self.n_history, :, :] = frame
            self.history[:self.n_history, :, :] = self.history[1:, :, :]
        self.memory.append((frame, action, reward, done))
        
        state = np.stack(self.history[:self.n_history]) #返回state
        return torch.Tensor(state[np.newaxis, :] ).float().to(device)                    
       # return None

    def sample_mini_batch(self,batch_size):
        mini_batch = []
        indices = self._get_valid_indices(batch_size)        
        for i in indices:
            sample = []
            for j in range(self.n_history+1):
                sample.append(self.memory[i + j])

            sample = np.array(sample)
            mini_batch.append((np.stack(sample[:, 0], axis=0), sample[3, 1], sample[3, 2], sample[3, 3]))

        mini_batch = np.array(mini_batch).transpose()  # (batch_size,4) to (4,batch_size)
        

        history = np.stack(mini_batch[0], axis=0)      # stack all states: [[frame1,...,frame5],[frame1,...,frame5],...  ]
                                                       # history.shape:(32, 5, 84, 84)
        states = np.float32(history[:, :self.n_history, :, :]) / 255.
        actions = list(mini_batch[1])
        rewards = list(mini_batch[2])
        next_states = np.float32(history[:, 1:, :, :]) / 255.
        dones = mini_batch[3] # checks if the game is over
        
        #return mini_batch
        return (states,actions,rewards,next_states,dones)
    
    def _get_valid_indices(self,batch_size):  
        indices = np.empty(batch_size, dtype=np.int32)
        n_history_plus =  self.n_history+1
        end_idx = len(self.memory) -n_history_plus
        
        for i in range(batch_size):            
            while True:
                index = random.randint(0, end_idx)                
                valid = True
                for j in range(self.n_history):
                    if self.memory[i + j][3]:
                        valid = False               
                        break
                if valid:
                    break
            indices[i] = index
        return indices

    def __len__(self):
        return len(self.memory)
```

Following code to test this class:
```python
mem = ReplayMemory(4,84,84)

img = np.random.randn(84,84)
action, r, done = 1,0.3,False
for i in range(100):
    state = mem.push(img, action, r, done)
    if i <5:
        print(state.shape)
  
    
(states,actions,rewards,next_states,dones) = mem.sample_mini_batch(32)
print(states.shape,len(actions))
```
output is :
···
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
(32, 4, 84, 84) 32
···

I modified the code after I read other people's code:
```python
from collections import deque
import numpy as np
import torch
import random

device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

class ReplayMemory_2(object):
    def __init__(self,n_history,capacity=1000000):        
        self.n_history = n_history        
        self.capacity= capacity
        self.memory = deque(maxlen=capacity)  
    
    def push(self, frame, action, reward, done):        
        self.memory.append((frame, action, reward, done))
        return self.get_state(len(self.memory))
        #state = np.stack(self.history[:self.n_history]) #返回state
        #return torch.Tensor(state[np.newaxis, :] ).float().to(device)   
    
    def get_state(self,idx):
        n = len(self.memory)
        assert n > 0        
        #if len(self.memory[idx][0].shape) == 2:
        #    return self.memory[idx]
        
        if idx>=self.n_history:
            frames = [self.memory[i][0] for i in range(idx-self.n_history,idx)]
        else:          
            frames = [self.memory[i][0] for i in range(idx)]         
            missing_n = self.n_history - idx         
            frames_2 = [np.zeros_like(self.memory[0][0])  for _ in range(missing_n)]         
            frames = frames_2+frames
           
            
        state =  np.stack(frames)
        #return state
        return torch.Tensor(state[np.newaxis, :] ).float().to(device)         
        

    def sample_mini_batch(self,batch_size):
        mini_batch = []
        indices = self._get_valid_indices(batch_size)        
        for i in indices:
            sample = []
            for j in range(self.n_history+1):
                sample.append(self.memory[i + j])

            sample = np.array(sample)
            mini_batch.append((np.stack(sample[:, 0], axis=0), sample[3, 1], sample[3, 2], sample[3, 3]))

        
        mini_batch = np.array(mini_batch).transpose()  # (batch_size,4) to (4,batch_size)
        print(mini_batch.shape)

        history = np.stack(mini_batch[0], axis=0)      # stack all states: [[frame1,...,frame5],[frame1,...,frame5],...  ]
                                                       # history.shape:(32, 5, 84, 84)
              
        states = np.float32(history[:, :self.n_history, :, :]) / 255.
        actions = list(mini_batch[1])
        rewards = list(mini_batch[2])
        next_states = np.float32(history[:, 1:, :, :]) / 255.
        dones = mini_batch[3] # checks if the game is over
        
        #return mini_batch
        return (states,actions,rewards,next_states,dones)
    
    def _get_valid_indices(self,batch_size):  
        indices = np.empty(batch_size, dtype=np.int32)
        n_history_plus =  self.n_history+1
        end_idx = len(self.memory) -n_history_plus
        
        for i in range(batch_size):            
            while True:
                index = random.randint(0, end_idx)                
                valid = True
                for j in range(self.n_history):                   
                    if self.memory[i + j][3]:
                        valid = False               
                        break
                if valid:
                    break
            indices[i] = index
        return indices

    def __len__(self):
        return len(self.memory)
```

The class is tested again:
```python
mem = ReplayMemory_2(4)

img = np.random.randn(84,84)
action, r, done = 1,0.3,False
for i in range(100):
    state = mem.push(img, action, r, done)
    if i <5:
        print(state.shape)
  
    
(states,actions,rewards,next_states,dones) = mem.sample_mini_batch(32)
print(states.shape,len(actions))
```

The output is same as before:
···
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
torch.Size([1, 4, 84, 84])
(4, 32)
(32, 5, 84, 84)
(32, 4, 84, 84) 32
···

Now define the Q network as following:
```python
class DQN(nn.Module):
    def __init__(self, in_channels,n_actions):
        super(DQN, self).__init__()
        self.conv1 = nn.Conv2d(in_channels = in_channels, out_channels=32, kernel_size=8, stride=4)
        #self.bn1 = nn.BatchNorm2d(32)
        self.conv2 = nn.Conv2d(32, 64, kernel_size=4, stride=2)
        #self.bn2 = nn.BatchNorm2d(64)
        self.conv3 = nn.Conv2d(64, 64, kernel_size=3, stride=1)
        #self.bn3 = nn.BatchNorm2d(64)
        self.fc = nn.Linear(3136, 512) #64 * 8 * 8, 512)
        self.head = nn.Linear(512, n_actions)

    def forward(self, x):
        x = F.relu(self.conv1(x))
        x = F.relu(self.conv2(x))
        x = F.relu(self.conv3(x))
        x = F.relu(self.fc(x.view(x.size(0), -1)))
        return self.head(x)
```
And the Agent is :
```python
class DQNAgent():
    def __init__(self, n_action,cofig):
        self.n_action = n_action  
        self.n_history = cofig["n_history"]
        
        
        self.batch_size =  cofig["batch_size"]
        self.gamma =  cofig["gamma"]       
        self.discount_factor = cofig["gamma"]
        self.epsilon_start =  cofig["epsilon_start"]
        self.epsilon_end =  cofig["epsilon_end"]
        self.epsilon_decay =cofig["epsilon_decay"]
        self.epsilon_mode =cofig["epsilon_mode"]
        
        if  self.epsilon_mode: #False           
            self.eps_schedule = LinearSchedule(self.epsilon_decay,self.epsilon_end,self.epsilon_start)
        else:
            self.eps_schedule = ExpSchedule(self.epsilon_decay,self.epsilon_end,self.epsilon_start)
        
        
       # self.epsilon_decay = (self.epsilon - self.epsilon_min) / self.explore_step  
    
        self.train_start = cofig["train_start"];
        self.update_target = cofig["update_target"];
        self.save_every = cofig["save_every"];
      
         # 回放内存
        self.memory = ReplayMemory(cofig["n_history"],cofig["h"],cofig["w"],cofig["memory_capacity"])
        
        # Create the policy net and the target net
        self.policy_net = DQN(self.n_history,n_actions= self.n_action)
        self.policy_net.to(device)
        self.target_net = DQN(self.n_history,n_actions= self.n_action)
        self.target_net.to(device)
        
        self.optimizer = optim.Adam(params=self.policy_net.parameters(), lr=cofig["learning_rate"]) 
        self.using_DDQN = False
        
        self.model_file =  cofig["model_file"]  
      
        if os.path.isfile(self.model_file):            
            checkpoint = torch.load(self.model_file)
            self.policy_net.load_state_dict(checkpoint['state_dict'])
            print("previous trained model loaded!")
            
        self.update_target_net()
        
        self.steps_done = 0   
        self.train_started= False

    # after some time interval update the target net to be same with policy net
    def update_target_net(self):
        self.target_net.load_state_dict(self.policy_net.state_dict())
        
    """Get action using policy net using epsilon-greedy policy"""    
    def choose_action(self, state):
        self.decrease_epsilon()
        if state is not None and np.random.random() > self.epsilon:
            return self.opti_action(state)
            #with torch.no_grad():
             #   return self.policy_net(state).max(1)[1].view(1, 1).item()
        else:
            return self.random_action()
            #return torch.tensor([[random.randrange(self.n_action)]], device=device, dtype=torch.long).item()
    
    def opti_action(self, state):
         with torch.no_grad():
                return self.policy_net(state).max(1)[1].view(1, 1).item()
    def random_action(self):
        return torch.tensor([[random.randrange(self.n_action)]], device=device, dtype=torch.long).item()
        
    
    
    def decrease_epsilon(self):
        self.steps_done+=1
        self.epsilon = self.eps_schedule( self.steps_done)
        if False:        
            if self.epsilon_mode:
                if self.epsilon > self.epsilon_min:
                    self.epsilon -= self.epsilon_decay  
            else:
                epsilon_start,epsilon_end,epsilon_decay = self.epsilon_start,self.epsilon_end,self.epsilon_decay
                self.epsilon= epsilon_end + (epsilon_start - epsilon_end) * \
                                math.exp(-1. * self.steps_done / epsilon_decay)

    # pick samples randomly from replay memory (with batch_size)
    def train_policy_net(self):
        if len(self.memory)<self.batch_size:
            return
        if self.steps_done< self.train_start:
            return
        
        if self.train_started==False:
            self.train_started = True
            print("training started...")
      
        gamma = self.gamma #0.999       

        #mini_batch = self.memory.sample_mini_batch(self.batch_size)
        
        (states,actions,rewards,next_states,dones) =    self.memory.sample_mini_batch(self.batch_size)
        
        
        state_batch = torch.Tensor(states).float().to(device)
        action_batch = torch.Tensor(actions).type(torch.LongTensor).to(device).view(-1,1)
        reward_batch = torch.Tensor(rewards).to(device)
        
        
        non_final_mask = torch.tensor(tuple(map(lambda s: s is not True,
                                          dones)), device=device, dtype=torch.bool)
        
        non_final_next_states = torch.Tensor([next_states[i] for i in range(dones.shape[0])
                                                if dones[i] is not True]).float().to(device) 
        
        policy_net,target_net,optimizer = self.policy_net,self.target_net,self.optimizer
    
        # 计算预测值： Q(s_t, a) - Q of the current state      
        state_action_values = policy_net(state_batch).gather(1, action_batch) # Q(s_i,a_i)
                
        # 计算下一个状态的最大动作价值：Q(s',a')
        # Find maximum Q-value of action at next state from target net       
        next_state_values = torch.zeros(self.batch_size, device=device)
        if self.using_DDQN:
            next_state_action = policy_net(non_final_next_states.to(device)).detach().max(1)[1].view(-1,1)        
            next_state_values[non_final_mask] = target_net(non_final_next_states.to(device)).detach().gather(1, next_state_action).view(-1) 
        else:
            next_state_values[non_final_mask] = target_net(non_final_next_states.to(device)).max(1)[0].detach() # max Q(s'_i)
             
        # 计算目标值
        expected_state_action_values = (next_state_values * gamma) + reward_batch
            
        # Compute the Huber Loss
        ### CODE ####       
        loss = F.smooth_l1_loss(state_action_values, expected_state_action_values.unsqueeze(1))
        
        # Optimize the model 
        ### CODE ####
        optimizer = self.optimizer
        optimizer.zero_grad()
        loss.backward()
        for param in policy_net.parameters():
            param.grad.data.clamp_(-1, 1)
        optimizer.step()
        
        if self.steps_done% self.update_target == 0:
            self.update_target_net()            
```

Please visit my youtube channel "hwdong" to find more code 
...

