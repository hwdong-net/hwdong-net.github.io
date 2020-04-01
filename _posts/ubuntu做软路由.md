
## 用Debian系统做软路由

### 1. 安装debian
   1.1 制作安装u盘
      [debian 下载] (https://www.debian.org/distrib/netinst)
  [How to create a bootable Debian USB drive using Windows](https://unix.stackexchange.com/questions/263615/how-to-create-a-bootable-debian-usb-drive-using-windows)
  1.2 用debian安装u盘在工控机上安装debian系统
###
  

  

### 1. 安装ubuntu系统
  1.1 制作ubuntu安装u盘
  1.2 用ubuntu安装u盘在工控机上安装ubuntu系统
  
  

### 2. 使ubuntu成为软路由
  
更新软件包列表

```
sudo apt-get update
sudo apt-get upgrade -y
```

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

Ubuntu 18.04使用netplan配置网络

先查询网络设备有哪些？
```
ip a
```
再修改配置文件
```
sudo nano /etc/netplan/01-netcfg.yaml
```
修改为：
```
network:
    renderer: networkd
    ethernets:
        enp1s0:
          dhcp4: no
        enp2s0:
          dhcp4: no
        enp3s0:
          dhcp4: no
        enp4s0:
          dhcp4: no
        enp5s0:
          dhcp4: no
        enp6s0:
          dhcp4: no
    bridges:
      br-lan:
        addresses: [10.0.0.1/24,'ipv6全球路由前缀/64']
        dhcp4: no
        dhcp6: no
        accept-ra: no
        interfaces:
          - enp6s0
          - enp5s0
          - enp4s0
          - enp3s0
          - enp2s0
    version: 2
```

使用10.0.0.1/24作为内网ip，enp1s0作为外部网络（WAN）接口，其余接口作为局域网（LAN）接口



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


[Linux 将debian配置为软路由](https://www.hotbak.net/key/debian%20%E8%BD%AF%E8%B7%AF%E7%94%B1.html)

[使用Ubuntu 18.04打造超级家庭网关（边缘路由器）](https://www.johnrosen1.com/ubuntu-router/)

[如何在 Linux 上使用网络配置工具 Netplan](https://zhuanlan.zhihu.com/p/46544606)

[配置ubuntu搭建一个路由器](https://blog.csdn.net/u012174021/article/details/45369457?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

[Ubuntu Server（18.04）开启路由转发搭建软路由](https://blog.csdn.net/Splend520/article/details/86505569)

[https://www.right.com.cn/forum/thread-220005-1-1.html](https://www.right.com.cn/forum/thread-220005-1-1.html)

[Linux 软路由单线多拨](https://www.zfl9.com/multi-wan-router.html)


[基于Ubuntu的软路由教程（单臂）](基于Ubuntu的软路由教程（单臂）)
