---
layout:       post
title:        "test the deep learning library. in my book"
subtitle:     "test the deep learning library. in my book"
date:         2019-10-19 15:38:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - DL
---

test dense layer
```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

from NeuralNetwork import *
import util

np.random.seed(1)

pts = 100

k = 2
b =1
X = np.random.randn(pts,1)*10                 # 随机采样一些x坐标
Y = k*X+b
Y  = Y+ np.random.randn(pts,1)*4              #给 Y随机噪声
plt.plot(X,Y,'o')
plt.xlabel('x')
plt.ylabel('y')
plt.show()


import util 
dense = Dense(1,1,('no',0.01))
losses = []
epochs = 100
reg  = 0.
learning_rate = 1e-2

nn = dense
for epoch in range(epochs):
    for i in range(len(nn.params)):
        nn.grads[i] = 0.
    #print(nn.params[0],nn.params[1])
    F = nn.forward(X)
    loss,grad = util.loss_grad_least(F,Y)
    dx = dense.backward(grad)
    for i in range(len(nn.params)):
        nn.params[i] -=  learning_rate*nn.grads[i]
    print(loss)
    losses.append(loss)
plt.plot(losses)
```

test NeuralNetwork
 ```python
 import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

from NeuralNetwork import *
import util
 pts = 1000

k = 2
b =1
X = np.random.randn(pts,1)*10                 # 随机采样一些x坐标
Y = k*X+b
Y  = Y+ np.random.randn(pts,1)*4              #给 Y随机噪声
plt.plot(X,Y,'o')
plt.xlabel('x')
plt.ylabel('y')
plt.show()


m,n = X.shape[0],X.shape[1]
print("n",n)
nn = NeuralNetwork()
dense = Dense(1,1,('no',0.01))
nn.add_layer(Dense(1,1,('xavier',0.01))) # dense)

learning_rate = 1e-3
momentum = 0 #0.9
optimizer = SGD(nn.parameters(),learning_rate,momentum)

losses = []
epochs = 100
reg  = 0.

for epoch in range(epochs):
    F = nn.forward(X)
    loss,loss_grad = util.loss_grad_least(F,Y)
     
    optimizer.zero_grad() 
    nn.zero_grad()
    nn.backward(loss_grad,reg)               
    loss += nn.reg_loss(reg)         
    
    optimizer.step()  
          
    losses.append(loss) 
    if epoch % 10 == 4: 
        print('epoch {}, loss {}'.format(epoch,loss.item())) #.data[0]))        
        
plt.plot(losses)
 ```
