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

#### 1. 申请域名+配置解析+验证

#### 2.  安装Caddy

[Install Caddy with PHP & HTTPS using Let’s Encrypt on Ubuntu](https://www.cloudbooklet.com/install-caddy-with-php-https-using-letsencrypt-on-ubuntu/)

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
curl https://getcaddy.com | sudo bash -s personal  tls.dns.cloudflare
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
设置权限：

```
sudo apt-get install libcap2-bin
sudo setcap cap_net_bind_service=+ep /usr/local/bin/caddy
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
**2.4 Configure Caddy Systemd service unit**

ow you can create a systemd service file for Caddy which is available in the official repository and reload the demon for the changes to be available.

```
wget https://raw.githubusercontent.com/caddyserver/caddy/master/dist/init/linux-systemd/caddy.service
sudo cp caddy.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/caddy.service
sudo chmod 644 /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
```

**2.5 Configure Domain and Webroot in Caddy**

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
yourdomain.com {
     root /var/www/
     tls     23232@qq.com
     gzip
 protocols tls1.2 tls1.3
  ciphers ECDHE-ECDSA-AES128-GCM-SHA256 ECDHE-RSA-AES128-GCM-SHA256 ECDHE-ECDSA-AES256-GCM-SHA384 ECDHE-RSA-AES256-GCM-SHA384 ECDHE-ECDSA-WITH-CHACHA20-POLY1305 ECDHE-RSA-WITH-CHACHA20-POLY1305 

 proxy /ray localhost:10000 {
         websocket
        header_upstream -Origin
   }
} 
```

Hit **CTRL + X** followed by **Y** and **ENTER** to save and exit the file.

modify v2ray confiture file:

```
{
  "inbounds": [
    {
      "port": 10000,
      "listen":"127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/ray"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}

```

### 参考：
1. https://guide.v2fly.org/advanced/wss_and_web.html#%E9%85%8D%E7%BD%AE
2. https://www.cloudbooklet.com/install-caddy-with-php-https-using-letsencrypt-on-ubuntu/
3. https://melty.land/blog/caddy-and-cloudflare

### CDN 
sudo nano /etc/systemd/system/caddy.service

一个坑（参考[https://github.com/caddyserver/caddy/issues/2775](https://github.com/caddyserver/caddy/issues/2775)）： 要删除"Environment=CADDYPATH=/etc/ssl/caddy"
然后添加：
```
Environment=CLOUDFLARE_EMAIL=hwdong.net@gmail.com
Environment=CLOUDFLARE_API_KEY=699a91dfssdfa1d34fd53499a57e5
sudo systemctl daemon-reload
sudo systemctl restart caddy
sudo systemctl status caddy.service
```
CaddyFile shoube be modified as following :
yourdomain.com {
     root /var/www/
     tls     23232@qq.com
      tls {
        dns cloudflare
     }
     gzip 

 proxy /ray localhost:10000 {
         websocket
        header_upstream -Origin
   }
} 
```

#### 其他参考：

[Debian 9 安装配置 Caddy Server](https://cloud.tencent.com/developer/article/1375370)

[https://github.com/caddyserver/caddy/tree/master/dist/init/linux-systemd](https://github.com/caddyserver/caddy/tree/master/dist/init/linux-systemd)

[v2ray基于caddy的VMESS+WS+TLS+Website+CDN豪华配置](http://csuncle.com/2019/06/05/v2ray%E5%9F%BA%E4%BA%8Ecaddy%E7%9A%84VMESS-WS-TLS-Website-CDN%E8%B1%AA%E5%8D%8E%E9%85%8D%E7%BD%AE/)

[用Caddy配合Docker搭建V2Ray的(vmess+ws)+tls，可选套cdn](https://ssu.tw/index.php/archives/6/)

#### 适合小白的最简单的谷歌云+V2ray教程：

[2020最新5分钟申请免费谷歌云搭建v2ray|(含bbr加速)|免费看油管|客户端可用SSR](https://www.youtube.com/watch?v=hscVmLJxPhc)

2020年2月谷歌云申请+V2ray搭建补充说明|最简单教程官方命令非脚本安全无木马](https://www.youtube.com/watch?v=PKtmGgdVEas)
