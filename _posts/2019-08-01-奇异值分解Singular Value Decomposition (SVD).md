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

-1）对调矩阵的两行，记为 $r_i \leftrightarrow r_j$

-2）将某一行的元素乘以非零常数，记为 $r_i\times s $

-3）将某一行的元素乘以非零常数后，加到另一行上，记为$r_i+r_j\times s $


**定理**： 
 - 对一个矩阵$A_{mn}$进行初等行变换相对于用相应的初等矩阵$T_{mm}$左乘矩阵A。
 
 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/3b151927c99a2a93d0357d22dce8ba67c88bb14b)
 
 - 对一个矩阵$A_{mn}$进行初等列变换相对于用相应的初等矩阵$T_{nn}$右乘矩阵A。


任何一个矩阵$A_{mn}$可以初等变换变成一个对角矩阵。即：

$$ A_{mn} = U_{mm} D_{mn} V_{mn}^T $$


参考：


1. [Singular Value Decomposition (SVD) tutorial](http://web.mit.edu/be.400/www/SVD/Singular_Value_Decomposition.htm)

[Singular Value Decomposition (SVD) Tutorial: Applications, Examples, Exercises](https://blog.statsbot.co/singular-value-decomposition-tutorial-52c695315254)
