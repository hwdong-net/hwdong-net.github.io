---
layout:       post
title:        "2024最强翻墙方法，用CloudFlare搭建永久免费Vless节点，无需VPS和域名，无流量限制，小白几分钟轻松搭建机场，优选域名，轻松解锁ChatGPT、netflix，看8K视频无压力."
subtitle:     "最简单CloudFlare部署Vless的教程"
date:         2024-03-23 10:05:00
author:       "xhwdong"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - AI
--- 

#### [2024最强翻墙方法，用CloudFlare搭建永久免费Vless节点，无需VPS和域名，无流量限制，小白几分钟轻松搭建机场，优选域名，轻松解锁ChatGPT、netflix，看8K视频无压力.](https://youtu.be/tWhbZWtn7kQ)

![](https://hwdong-net.github.io/yt_imgs/CloudFlare_Vless.jpg)




#### 准备工作：

1. V2ray客户端:
https://github.com/2dust/v2rayN/releases
https://github.com/yanue/V2rayU/releases/tag/v3.8.0

2. ip&域名优选打包

#### 部署过程

申请临时email: [linshiyouxiang.net](linshiyouxiang.net)

1.  用email在[Cloudflare.com](Cloudflare.com) 注册一个账号

2.  Cloudflare创建workers

3.  修改works的代码为:
[https://github.com/3Kmfi6HP/EDtunnel/blob/main/_worker.js](https://github.com/3Kmfi6HP/EDtunnel/blob/main/_worker.js)

4.  用V2ray软件生成的UUID修改 User_id

5.  在V2ray软件中用vless订阅网址创建订阅分组

6.  用ip优选工具获取优选的ip，或者用域名优选工具获取优选域名，
    填入vless订阅的地址里




