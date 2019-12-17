---
layout:       post
title:        双硬盘安装windows 10和 ubuntu 19.04双系统的2个坑
subtitle:     双硬盘安装windows 10和 ubuntu 19.04双系统的2个坑
date:         2019-06-07 15:06:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - IT
---

先在一个硬盘上安装好windows(windows10或windows7)系统，然后按照下面的步骤安装ubuntu双系统：

1. 下载ubuntu  iso镜像文件
2. 用rufus制作安装USB盘
3. Windows系统安装盘上压缩出比如256M用于安装ubuntu的boot分区。用磁盘管理器将另一块硬盘或其他的剩余空间留给ubuntu系统。
   当然如果只有一块硬盘，只要压缩出足够未使用空间给ubuntu系统就行。
4. 选择从USB盘启动，一般是启动时，不断按“ del”键，直到出现BIOS，然后选择从 U盘启动就行（不要选择UEFI方法）。
5. 安装与其他系统并存的 ubuntu，
     安装过程中，不要选择擦除原来的系统，而是选择其他，然后在下一部弹出窗口中，一定要选择“**装第三方软件、graphcis和wifi**"等功能
   
    安装过程中，通过'+'创建分区：
```
    1) /分区：  10G
  
    2) swap交换分区： 2048G
    
    3）/boot分区： 256M。  注意： 这个分区必须和windows系统在同一个物理硬盘上。
    
    4) /home分区：剩余空间
```   


有2个坑:

  + 第一个坑： ubuntu的/boot分区（/分区也如此）必须和window系统盘是同一个物理硬盘;
  
  + 第二个坑，安装过程要联网，且开始选项要勾选“安装第三方软件、graphcis和wifi等功能”。不然启动可能一直停留在logo画面。
  
  #### 觉得文章对您有用，帮忙点击博客上面的广告哦！谢谢！

