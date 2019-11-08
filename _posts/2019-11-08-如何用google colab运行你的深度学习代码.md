---
layout:       post
title:        "如何用google colab运行你的深度学习代码"
subtitle:     "how to use google colab for your deep learning"
date:         2019-11-08 19:53:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - DL
---

google Colaboratory（简称 google colab）是谷歌提供的免费的云端的Jupyter notebook环境，google colab已经自带了各种常用的python程序包包括著名的深度学习库tensorflow和pytorch，特别是google colab提供了免费的GPU，使得不需要做任何安装和配置就能在浏览器编写和执行python代码（包括机器学习、深度学习代码），使用者可以保存和共享文档，访问强大的计算资源。

使用colab非常简单，首先进入gmail账户（没有的可以免费注册一个），然后进入google drive（硬盘）:

<div align="center"><img  style="zoom:70%;" src="../imgs/colab_1.png"/></div>

然后在google drive页面左上角的"My Drive（我的硬盘）"处点鼠标右键，出现弹出菜单，选择其中的“New Folder(新建文件夹)”：

<div align="center"><img  style="zoom:70%;" src="imgs/colab_2.png"/></div>

在出现的创建文件夹对话框中输入要创建的文件夹的名字，如“App”：

<div align="center"><img  style="zoom:70%;" src="imgs/colab_3.png"/></div>

在左侧导航选择这个App文件夹，然后在其右侧窗口里点击鼠标右键：

<div align="center"><img  style="zoom:70%;" src="imgs/colab_4.png"/></div>

在右键菜单里选择“More” -> “Colaboratory”，colab将自动创建一个名字为“Untitled0.ipynb”的jupyter notebook文档。
<div align="center"><img  style="zoom:70%;" src="imgs/colab_5.png"/></div>

可以将这个文档命令为你感兴趣的名字。

<div align="center"><img  style="zoom:70%;" src="imgs/colab_6.png"/></div>

要使用GPU，请在"runtime"的子菜单里选择“change runtime type”：
<div align="center"><img  style="zoom:70%;" src="imgs/colab_7.png"/></div>

在弹出菜单的第二项“Hardware accelerator”的下拉菜单里选择“GPU”。

现在你可以利用谷歌提供的GPU运行你的深度学习代码了。

更详细的使用说明，请参考本书作者的网址：[https://hwdong-net.github.io](https://hwdong-net.github.io) 。
