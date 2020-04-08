## 1. 刷Breed

[新路由3 (Newifi D2) 免拆机免解锁刷 Breed 教程](https://www.right.com.cn/forum/thread-342918-1-1.html)

参考：[一步一步教你解锁newifi3(新路由3)并编译刷入最新官方OpenWrt](https://www.right.com.cn/forum/thread-365936-1-1.html)

## 2. 官网openwrt

[http://openwrt-dist.sourceforge.net/packages/base](http://openwrt-dist.sourceforge.net/packages/base)

[http://openwrt-dist.sourceforge.net/packages/luci](http://openwrt-dist.sourceforge.net/packages/luci)

2.1 重置路由器
```
1. 拔掉电源
2. 按下电源旁按键
3.插上电源
4. 看见路由器灯亮可以放开（大约3秒）
5.登录192.168.1.1
```

2.2 刷入openwrt
```
恢复出厂设置-公版
更新固件-上传openwrt
```
2.3 配置LAN (或WAN)


[N1 OpenWRT 当旁路由设置教程](https://www.hotbak.net/key/n1%E5%81%9A%E6%97%81%E8%B7%AF%E7%94%B1%E7%9A%84%E7%94%A8%E5%A4%84.html)

## 3. 安装酸酸乳-透明代理

2.1 下载：
`
ChinaDNS (openwrt-chinadns)
DNS-forwarder (openwrt-dns-forwarder)
shadowsocks-libev (openwrt-shadowsocks)
luci-app-ChinaDNS
luci-app-dns-forwarder
luci-app-shadowsocks
`

2.2 用winscp上传到路由器某个目录（如ss）下

2.3 安装

```
opkg install /ss/*.ipk
```


[一步一步教你解锁newifi3(新路由3)并编译刷入最新官方OpenWrt](https://www.right.com.cn/forum/thread-365936-1-1.html)

[https://leamtrop.com/2017/05/14/shadowsocks-proxy-on-lede/](https://leamtrop.com/2017/05/14/shadowsocks-proxy-on-lede/)
