## 1. 刷Breed

[新路由3 (Newifi D2) 免拆机免解锁刷 Breed 教程](https://www.right.com.cn/forum/thread-342918-1-1.html)

参考：[一步一步教你解锁newifi3(新路由3)并编译刷入最新官方OpenWrt](https://www.right.com.cn/forum/thread-365936-1-1.html)

## 2. 官网openwrt

下载[https://downloads.openwrt.org/releases/](https://downloads.openwrt.org/releases/)

#### 2.1 重置路由器

```
1. 拔掉电源
2. 按下电源旁按键
3.插上电源
4. 看见路由器灯亮可以放开（大约3秒）
5.用网线直连newfi3和pc，登录192.168.1.1。输入密码：password
```

#### 2.2 刷入openwrt

在breed Web界面中:

```
恢复出厂设置-公版
更新固件-上传openwrt
```
#### 2.3 配置LAN (或WAN)

静态ip地址：

```
ip:  192.168.2.242

网关：192.168.2.1

DNs：192.168.2.1

144.144.144.144
```

![](C:\Users\hwdon\Downloads\mipsel_24kc\LAN_ip.png)

禁用ipv6

#### 2.4 检查能否连到路由器

用网线直连盒子和主路由，并让盒子重新上电

PC端ping 192.168.2.242，正常后，浏览器登陆盒子web界面192.168.2.242






[N1 OpenWRT 当旁路由设置教程](https://www.hotbak.net/key/n1%E5%81%9A%E6%97%81%E8%B7%AF%E7%94%B1%E7%9A%84%E7%94%A8%E5%A4%84.html)

## 3. 安装酸酸乳-透明代理

3.0 检查CPU架构：
```
opkg print-architecture | awk '{print $2}'
```
3.1 下载：

```
ChinaDNS (openwrt-chinadns)
DNS-forwarder (openwrt-dns-forwarder)
shadowsocks-libev (openwrt-shadowsocks)
luci-app-ChinaDNS
luci-app-dns-forwarder
luci-app-shadowsocks
```

[http://openwrt-dist.sourceforge.net/packages/base](http://openwrt-dist.sourceforge.net/packages/base)

[http://openwrt-dist.sourceforge.net/packages/luci](http://openwrt-dist.sourceforge.net/packages/luci)

3.2 用winscp上传到路由器某个目录（如ss）下

3.3 安装

```
opkg install /ss/*.ipk
```


[一步一步教你解锁newifi3(新路由3)并编译刷入最新官方OpenWrt](https://www.right.com.cn/forum/thread-365936-1-1.html)

[https://leamtrop.com/2017/05/14/shadowsocks-proxy-on-lede/](https://leamtrop.com/2017/05/14/shadowsocks-proxy-on-lede/)
