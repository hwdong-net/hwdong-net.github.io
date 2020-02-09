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

#### 1. 申请域名+配置解析+验证



#### 2.  安装Caddy

[Install Caddy with PHP & HTTPS using Let’s Encrypt on Ubuntu](https://www.cloudbooklet.com/install-caddy-with-php-https-using-letsencrypt-on-ubuntu/)

[Debian 9 安装配置 Caddy Server](https://cloud.tencent.com/developer/article/1375370)

[Install V2Ray + WebSocket + TLS + Caddy + CDN Using 233boy Script](https://armazopu.github.io/v2ray+websocket+tls+caddy+cdn.html)

**2.1 install curl**

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
**check(检查安装是否正确)**:
```
which curl
```

output:
```
/usr/bin/curl
```



**2.2  install Caddy**

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
check version(检查安装的caddy版本)
```
caddy -version
```
output(输出)：
```
Caddy v1.0.4 (h1:wwuGSkUHo6RZ3oMpeTt7J09WBB87X5o+IZN4dKeh
cQE=)
```

**2.3 Configure Caddy**

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
** 2.4  Configure Caddy Systemd service unit **
ow you can create a systemd service file for Caddy which is available in the official repository and reload the demon for the changes to be available.
```
wget https://raw.githubusercontent.com/caddyserver/caddy/master/dist/init/linux-systemd/caddy.service
sudo cp caddy.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/caddy.service
sudo chmod 644 /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
```
** 2.5 Configure Domain and Webroot in Caddy**

```
sudo mkdir /var/www
sudo chown www-data:www-data /var/www
sudo nano /var/www/index.html
```

Create a Caddy file named Caddyfile inside /etc/caddy/ and configure your domain name with HTTPS.
```
sudo nano /etc/caddy/Caddyfile 
```

Copy the below configuration and paste it inside this file.
```
https://domain.com {
     root /var/www/

     log /var/log/caddy/domain.log

     tls on
     gzip

    proxy /ray localhost:10000 {
      websocket
      header_upstream -Origin
    }
}
```

Hit **CTRL + X** followed by **Y** and **ENTER** to save and exit the file.

 

### 参考：
[The Beginner’s Guide to Nano, the Linux Command-Line Text Editor](https://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/)

[Nano简单使用](https://www.jianshu.com/p/e9384af07d66) 
