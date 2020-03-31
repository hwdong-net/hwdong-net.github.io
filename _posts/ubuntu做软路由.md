

### 1. 安装ubuntu系统
  1.1 制作ubuntu安装u盘
  1.2 用ubuntu安装u盘在工控机上安装ubuntu系统

### 2. 使ubuntu成为软路由

  2.1 ppoe拨号: 
  
   安装软件包pppoeconf
```
   sudo  install pppoeconf -y 
```
   配置拨号：输入账号名和密码
```
   pppoeconf
```

2.2 配置网卡:

创建 bridge, br0 （eth1 + wlan0）

```
apt-get install bridge-utils
sudo vim /etc/network/interfaces
```

```
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet static
address 192.168.1.155
netmask 255.255.255.0
gateway 192.168.1.1

auto eth1
iface eth1 inet static
address 192.168.6.1
netmask 255.255.255.0
gateway 192.168.6.1
```



[配置ubuntu搭建一个路由器](https://blog.csdn.net/u012174021/article/details/45369457?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

[Ubuntu Server（18.04）开启路由转发搭建软路由](https://blog.csdn.net/Splend520/article/details/86505569)

[https://www.right.com.cn/forum/thread-220005-1-1.html](https://www.right.com.cn/forum/thread-220005-1-1.html)

[Linux 软路由单线多拨](https://www.zfl9.com/multi-wan-router.html)

[使用Ubuntu 18.04打造超级家庭网关（边缘路由器）](https://www.johnrosen1.com/ubuntu-router/)

[基于Ubuntu的软路由教程（单臂）](基于Ubuntu的软路由教程（单臂）)
