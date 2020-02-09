---
layout:       post
title:        谷歌云VPS+V2ray+WebSocket+TLS+Web_CDN
subtitle:     google cloud_VPS+V2ray+WebSocket+TLS+Web_CDN
date:         2020-02-10 10:11:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Life     
---   

### 1. 申请谷歌云（或购买其他VPS）+安装v2ray

### 2. WebSocket+TLS+Web

[https://guide.v2fly.org/en_US/advanced/wss_and_web.html](https://guide.v2fly.org/en_US/advanced/wss_and_web.html)

[v2ray基于caddy的VMESS+WS+TLS+Website+CDN豪华配置](http://csuncle.com/2019/06/05/v2ray%E5%9F%BA%E4%BA%8Ecaddy%E7%9A%84VMESS-WS-TLS-Website-CDN%E8%B1%AA%E5%8D%8E%E9%85%8D%E7%BD%AE/)

[用Caddy配合Docker搭建V2Ray的(vmess+ws)+tls，可选套cdn](https://ssu.tw/index.php/archives/6/)

1. 申请域名+配置解析+验证



2.  安装Caddy

[Install Caddy with PHP & HTTPS using Let’s Encrypt on Ubuntu](https://www.cloudbooklet.com/install-caddy-with-php-https-using-letsencrypt-on-ubuntu/)

[Debian 9 安装配置 Caddy Server](https://cloud.tencent.com/developer/article/1375370)

[Install V2Ray + WebSocket + TLS + Caddy + CDN Using 233boy Script](https://armazopu.github.io/v2ray+websocket+tls+caddy+cdn.html)

2.1 install curl
  For **Debian** or **Ubuntu** users, update your system and install curl like this:
  ```
  apt update
  apt upgrade
  apt install curl
  ```
  For **CentOS** users, update your system and install curl like this:
  ```
  yum update
  yum install curl
```
check:
```
which curl
```
output:
```
/usr/bin/curl
```

2.2  install Caddy.

```
curl https://getcaddy.com | sudo bash -s personal
```
check:
```
which caddy
```
output:
```
/usr/local/bin/caddy
```

2.3 Configure Caddy

Setup directories for Caddy.
```
sudo mkdir /etc/caddy
sudo mkdir /etc/ssl/caddy 
sudo mkdir /var/log/caddy 
```
Configure correct permissions.
```
sudo chown -R root:root /etc/caddy
sudo chown -R root:www-data /etc/ssl/caddy
sudo chown -R root:www-data /var/log/caddy 
sudo chmod 0770 /etc/ssl/caddy
```

