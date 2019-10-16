---
layout:       post
title:        "The implementation of LSTM（LSTM的实现代码）"
subtitle:     "The implementation of LSTM（LSTM的实现代码）"
date:         2019-10-16 18:38:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - DL
---

The Equations for LSTM 

[](https://weibo.com/u/6762417916?refer_flag=1005055010_&is_all=1)

![](http://hwdong-net.github.io/img3/lstm.png)

```python
def lstm_init_params(input_dim,hidden_dim,output_dim,scale=0.01):
    normal = lambda m,n : np.random.randn(m, n)*scale
    two = lambda : (normal(input_dim+hidden_dim, hidden_dim),np.zeros((1,hidden_dim)))
    
    Wi, bi = two() 
    Wf, bf = two()  
    Wo, bo = two()  
    Wc, bc = two()  
    
    Wy = normal(hidden_dim, output_dim)
    by = np.zeros((1,output_dim))
   
    params = [Wi, bi,Wf, bf, Wo,bo,Wc, bc,Wy,by]
    return params
    
def lstm_state_init(batch_size, hidden_size):
    return (np.zeros((batch_size, hidden_size)),
            np.zeros((batch_size, hidden_size)))
            
def lstm_forward(params,Xs, state):
    [Wi, bi,Wf, bf, Wo,bo,Wc,bc,Wy,by] = params   
    
    (H, C) = state         
    Hs = {}
    Cs = {}
    Zs = []
    
    Hs[-1] = np.copy(H)
    Cs[-1] = np.copy(C)
    
    Is = []
    Fs = []
    Os = []
    C_tildas = []
    
    for t in range(len(Xs)): 
        X = Xs[t]
        XH = np.column_stack((X, H))
       
        I = sigmoid(np.dot(XH, Wi)+bi)
        F = sigmoid(np.dot(XH, Wf)+bf)
        O = sigmoid(np.dot(XH, Wo)+bo)
        C_tilda = np.tanh(np.dot(XH, Wc)+bc)
    
        C = F * C + I * C_tilda
        H = O*np.tanh(C)       #O * C.tanh()  #输出状态 
        
        Y = np.dot(H, Wy) + by        # 输出
        
        Zs.append(Y)
        Hs[t] = H
        Cs[t] = C
        
        Is.append(I)
        Fs.append(F)
        Os.append(O)
        C_tildas.append(C_tilda)
    return Zs,Hs,Cs,(Is,Fs,Os,C_tildas)
    
def dtanh(x):
    return 1 - np.tanh(x) * np.tanh(x)
    
def lstm_backward(params,Xs,Hs,Cs,dZs,cache): # Ys,loss_function):
    [Wi, bi,Wf, bf, Wo,bo,Wc, bc,Wy,by] = params
    
    Is,Fs,Os,C_tildas = cache
    
    
    dWi,dWf,dWo,dWc,dWy  = np.zeros_like(Wi), np.zeros_like(Wf), np.zeros_like(Wo), np.zeros_like(Wc), np.zeros_like(Wy)
    dbi,dbf,dbo,dbc,dby = np.zeros_like(bi), np.zeros_like(bf),  np.zeros_like(bo), np.zeros_like(bc), np.zeros_like(by)

    dH_next = np.zeros_like(Hs[0])
    dC_next = np.zeros_like(Cs[0])
    
    input_dim = Xs[0].shape[1]
  
    h = Hs
    x = Xs
    
    T = len(Xs)  
    for t in reversed(range(T)):  
        I = Is[t]
        F = Fs[t]
        O = Os[t]
        C_tilda = C_tildas[t]
        H = Hs[t]
        X = Xs[t]
        C = Cs[t]
        H_pre =  Hs[t-1]
        C_prev = Cs[t-1]
        XH_pre = np.column_stack((X, H_pre))
        XH_ = XH_pre
        
        dZ = dZs[t]  
    
        #输出f的模型参数的idu
        
        dWy += np.dot(H.T,dZ)      
        dby += np.sum(dZ, axis=0, keepdims=True)   
        
        #隐状态h的梯度
        
        dH = np.dot(dZ, Wy.T) + dH_next     
        dC = dH_next*O*dtanh(C) +dC_next    #* H = O*np.tanh(C)
                       
        # do
        
        dO = np.tanh(C) *dH              # H = O * C.tanh()  
        
        dOZ = O * (1-O)*dO               
        dWo += np.dot(XH_.T,dOZ)
        dbo += np.sum(dOZ, axis=0, keepdims=True)              
        
        # dC_bar  
        
        dC_tilda = dC*I                         #C = F * C + I * C_tilda
        
        dC_tilda_Z =(1-np.square(dC_tilda))*dC_tilda    # C_tilda = sigmoid(np.dot(XH, Wc)+bc)    
        
        dWc += np.dot(XH_.T,dC_tilda_Z)       
        dbc += np.sum(dC_tilda_Z, axis=0, keepdims=True)
        
        #di  
        
        di = dC * C_tilda
        diZ = I*(1-I) * di
        dWi += np.dot(XH_.T,diZ)
        dbi += np.sum(diZ, axis=0, keepdims=True)  
        
        #df
        
        df = dC*C_prev 
        dfZ = F*(1-F) * df
        dWf += np.dot(XH_.T,dfZ)
        dbf += np.sum(dfZ, axis=0, keepdims=True)  
        
        
        dXH_ = (np.dot(dfZ, Wf.T)
             + np.dot(diZ, Wi.T)
             + np.dot(dC_tilda_Z, Wc.T)
             + np.dot(dOZ, Wo.T))
    
        dX_prev = dXH_[:, :input_dim]
        dH_prev = dXH_[:, input_dim:]
        dC_prev = F * dC
        
        dC_next = dC_prev
        dH_next = dH_prev              
        
    
    #for dparam in [dWi, dbi,dWf, dbf, dWo,dbo,dWc, dbc,dWy,dby]:
    
    #    np.clip(dparam, -5, 5, out=dparam) # clip to mitigate exploding gradients
    
    return [dWi, dbi,dWf, dbf, dWo,dbo,dWc, dbc,dWy,dby]
```
