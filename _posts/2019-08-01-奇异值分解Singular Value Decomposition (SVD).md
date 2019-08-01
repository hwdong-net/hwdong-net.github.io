---
layout:       post
title:        "奇异值分解Singular Value Decomposition (SVD)"
subtitle:     "Singular Value Decomposition (SVD)"
date:         2019-07-30 07:48:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - DS
---

**请在chrome浏览器安装mathjax插件浏览博客的公式**

### 初等变换

对矩阵的行初等变换可以分为以下三类：

-1）对调矩阵的两行，记为 $E_{i,j} = r_i \leftrightarrow r_j$

-2）将某一行的元素乘以非零常数，记为 $E_{s} =r_i\times s $

-3）将某一行的元素乘以非零常数后，加到另一行上，记为$E_{i,j(s)}r_i+r_j\times s $


**定理1**： 
 - 对一个矩阵$A_{mn}$进行初等行变换相对于用相应的初等矩阵$T_{mm}$左乘矩阵A。
 
 $$\begin{bmatrix}
     1 &  & & & & & \\
     0&\ddots& & & & & \\
     0 & 0 & &1 & & & \\
     0 & 1 & &0 & & & \\
     0 &  & & & &\ddots & \\
     &  & & & & &1 \\end{bmatrix}$$

 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/3b151927c99a2a93d0357d22dce8ba67c88bb14b)
 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/c8f916ca60d21f8aca2150614e01cde5d7ae72de)
 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/faedc76d62657278fc36551465bf189d4fb5da8a)
 
 - 对一个矩阵$A_{mn}$进行初等列变换相对于用相应的初等矩阵$T_{nn}$右乘矩阵A。

**定理2**： 任何初等矩阵E都是正交矩阵，即 $E^TE = EE^T = I$。

**定理3**： 任何正交矩阵的乘积仍然是正交矩阵。

$A_1、A_2$是正交矩阵,即满足
$$ A_1^TA_1 = A_1A_1^T = I$$
$$ A_2^TA_2 = A_2A_2^T = I$$
那么：
$$ (A_1A_2)^TA_1A_2 = A_2^TA_1^T A_1A_2 = I$$
   $$ A_1A_2(A_1A_2)^T = A_1A_2A_2^TA_1^T  = I$$
  即 $A_1A_2$页是正交矩阵  

根据定理1、定理2，可以得到：

**定理3**：
任何一个矩阵$A_{mn}$可以初等变换变成一个对角矩阵。即：

$$ A_{mn} = U_{mm} D_{mn} V_{nn}^T $$

证明：对A进行一系列的初等行变换和初等列变换，可将A化为对角矩阵（如同高斯消去法）。即
 $$E_hE_{h-1}\cdots E_{2} E_{1} A \hat{E_{1}} \hat{E_{2}} \cdots \hat{E_{k}} = D $$
 
  $$ A  ={E_1}^T \cdots  {E_h}^TD{\hat{E_k}}^T \cdots {\hat{E_1}} $$
  
 令 $U_{mm}  = {E_1}^T \cdots  {E_h}^T$, $V_{mn}^T = {\hat{E_k}}^T \cdots {\hat{E_1}} $。
 
  即有：$ A_{mn} = U_{mm} D_{mn} V_{nn}^T $
  
  当然$U_{mm}$和 $V_{nn}^T$都是正交矩阵，因为它们是初等矩阵的乘积。 因此：
  
  $$ U_{mm}^TU_{mm} = U_{mm}U_{mm}^T = I$$
  $$V_{nn}^TV_{nn} = V_{nn}V_{nn}^T = I$$

因为 $ A_{mn}^T = V_{nn} D_{mn} U_{mm}^T $

左右两边乘以 $U_{mm}$，得到：
 $ A_{mn}^T  U_{mm}  = V_{nn} D_{mn} U_{mm}^T  U_{mm} =V_{nn} D_{mn} $
 
 左右两边再乘以 $A_{mn}$，得到：
 $$ A_{mn}A_{mn}^T  U_{mm}  = A_{mn}V_{nn} D_{mn} = U_{mm} D_{mn} V_{nn}^TV_{nn} D_{mn} =  U_{mm} D_{mn}^2$$
 
 同理，可以得到：
 $$A_{mn}^T  A_{mn} V_{nn} = V_{nn} D_{mn}^2$$
 
 省略下标，即有：
  $$ AA^TU = UD^2  = D^2U$$
  $$ A^TAV = VD^2 = D^2V$$
  
  根据特征值和特征向量的定义：   $Bv= \lambda v$，则v是B的特征向量，而$\lambda$是对应的特征值。
  
  那么矩阵$AA^T$的特征向量是U中的列向量，特征值就是$D^2$中的对角线上相应的值。
  那么矩阵$A^TA$的特征向量是V中的列向量，特征值就是$D^2$中的对角线上相应的值。

参考：


1. [Singular Value Decomposition (SVD) tutorial](http://web.mit.edu/be.400/www/SVD/Singular_Value_Decomposition.htm)

[Singular Value Decomposition (SVD) Tutorial: Applications, Examples, Exercises](https://blog.statsbot.co/singular-value-decomposition-tutorial-52c695315254)
