
## 用Debian系统做软路由

### 1. 安装debian
   1.1 制作安装u盘
      [debian 下载] (https://www.debian.org/distrib/netinst)
  [How to create a bootable Debian USB drive using Windows](https://unix.stackexchange.com/questions/263615/how-to-create-a-bootable-debian-usb-drive-using-windows)
  
  1.2 用debian安装u盘在工控机上安装debian系统
  
  1.3 安装需要的软件：
  ```
  sudo apt-get install dnsmasq hostapd udhcpd pppoeconf
  ```
  
  
  1.3 网络配置，设有2块网卡：eth0 和  eth1。 eth0作为LAN(内网的网卡)， eth1(外网的网卡)。需要给它们配置ip地址。
   
    LAN口需要拥有一个固定的内网IP地址，可设置为 192.168.3.1/24。外网可以是拨号或直接连到上级路由器获取ip地址。
  
   外网如果需要拨号：  
  ```
   sudo apt install pppoeconf
   sudo pppoeconf
  ```
  
  设置LAN的ip地址，编辑 
  ```
  vim/etc/network/interfaces
  ```
  添加LAN口配置：
  
```
source /etc/network/interfaces.d/*
auto lo
iface lo inet loopback

#WAN 
allow-hotplug eth1
iface eth1 inet dhcp

#LAn
auto eth0
allow-hotplug eth0
iface eth0 inet static
address 192.168.2.1
netmask 255.255.255.0
 ```
  
  
 重启 networking 服务使之生效：
 ```
 sudo systemctl restart networking
 ```
  
###   2 安装DNS和DHCP服务

[使用 Debian 服务器作为家庭网关](https://ichon.me/post/1033.html)


 2.1  安装bind9 DNS服务器：
 ```
  sudo apt-get install bind9 bind9utils bind9-doc dnsutils
 ```
修改/etc/resolv.conf，
```
vim /etc/resolv.conf
```

加上：
```
nameserver 127.0.0.1
```  
2.2 [安装和配置DHCP服务器](https://www.tecmint.com/install-dhcp-server-in-ubuntu-debian/)

2.2.1 安装DHCP服务器
```
apt-get install isc-dhcp-server
```
编辑/etc/default/isc-dhcp-server，定义DHCPD用于服务DHCP请求的接口。

如果您希望DHCPD守护程序监听eth0，则将其设置为：
```
INTERFACES =“ eth0”
```

2.2.2 在Ubuntu中配置DHCP服务器

DHCP的主要配置文件是/etc/dhcp/dhcpd.conf，您必须在此处添加所有要发送到客户端的网络信息。并且，DHCP配置文件中定义了两种类型的语句，它们是：

+ 参数 –指定如何执行任务，是否执行任务或要发送到DHCP客户端的网络配置选项。
+ 声明 –定义网络拓扑，声明客户端，为客户端提供地址或将一组参数应用于一组声明。

```
sudo vi /etc/dhcp/dhcpd.conf
```
在文件顶部设置以下全局参数，它们将应用于下面的所有声明（请指定适用于您的方案的值）,如找到 option domain-name-servers，将其配置为本机地址：
```
option domain-name "tecmint.lan";
option domain-name-servers 192.168.10.1, 8.8.8.8;
default-lease-time 3600; 
max-lease-time 7200;
authoritative;
```

定义子网络,如为192.168.10.0/24 LAN网络设置DHCP
```
subnet 192.168.10.0 netmask 255.255.255.0 {
        option routers                  192.168.10.1;
        option subnet-mask              255.255.255.0;
        option domain-search            "tecmint.lan";
        option domain-name-servers      192.168.10.1;
        range   192.168.10.10   192.168.10.100;
        range   192.168.10.110   192.168.10.200;
}
```

要将固定（静态）IP地址分配给特定的客户端计算机，请在下面的部分中添加您需要显式指定其MAC地址和要静态分配的IP的部分：
```
host centos-node {
	 hardware ethernet 00:f0:m4:6y:89:0g;
	 fixed-address 192.168.10.105;
 }

host fedora-node {
	 hardware ethernet 00:4g:8h:13:8h:3a;
	 fixed-address 192.168.10.106;
 }
```

接下来，暂时启动DHCP服务，并使其从下一次系统启动时自动启动，如下所示：
```
----------- SystemD ------------ 
$ sudo systemctl start isc-dhcp-server.service
$ sudo systemctl enable isc-dhcp-server.service


------------ SysVinit ------------ 
$ sudo service isc-dhcp-server.service start
$ sudo service isc-dhcp-server.service enable
```
接下来，不要忘记允许防火墙上的DHCP服务（DHCPD守护程序侦听端口67 / UDP），如下所示：
```
$ sudo ufw allow  67/udp
$ sudo ufw reload
$ sudo ufw show
```

#### 步骤4：配置DHCP客户端计算机
此时，您可以将网络上的客户端计算机配置为自动从DHCP服务器接收IP地址。

登录到客户端计算机并按如下所示编辑以太网接口配置文件（注意接口名称/编号）：
```
$ sudo vi / etc / network / interfaces
```
并定义以下选项：
```
auto  eth0
iface eth0 inet dhcp
```
保存文件并退出。然后重新启动网络服务（或重新启动系统）：
```
------------ SystemD ------------ 
$ sudo systemctl restart networking

------------ SysVinit ------------ 
$ sudo service networking restart
```

或者，使用台式机上的GUI进行设置，将方法设置为自动（DHCP），如下面的屏幕截图所示（Fedora 25台式机）。

![](https://www.tecmint.com/wp-content/uploads/2017/03/Set-DHCP-Network-in-Fedora.png)
此时，如果正确配置了所有设置，则您的客户端计算机应该正在从DHCP服务器自动接收IP地址。


### 3. PPPoE拨号

安装pppoeconf：
```
apt-get install pppoeconf
```
安装结束后执行
```
pppoeconf
```
该工具用来配置 `/etc/ppp/peers/dsl-provider, /etc/ppp/*ap-secrets 以及 /etc/network/interfaces`，这个时候一定要确保eth0是连到猫上的，否则执行的时候会提示无法找到可以PPPoE的设备的。

接下来编辑/etc/network/interfaces，配置eth0的IP地址为 192.168.0.1：
```
auto eth1 iface eth1 inet static address 192.168.0.1 netmask 255.255.255.0 broadcast 192.168.0.255
```

重启


### 4. 搭建路由与防火墙

修改 /etc/sysctl.conf，添加以下几行让这台电脑支持包转发变交换机：
```
net.ipv4.ip_forward=1
```
然后安装arno-iptables-firewall，这个会帮你配置好iptables，让你的iptables直接变成路由器+防火墙。
```
安装界面后第一个要问你你的对外的网口是什么，前面因为配置好了PPPoE，所以这里填写ppp0，然后第二个问题问你的IP是否是ISP动态分配的，因为每次拨号都会有不一样的IP，所以这里选择Yes。
第三个问题问你要开放哪些TCP端口，我这里开放了80和443,
第四个问题问你开放哪些UDP端口，我这里留的空
第五个问题问你是否允许该机器的公网IP可以被外网ping，我选的No
第六个问题问你内网接口是什么我填的eth1
第七个问题问你内网子网的网段，我填的192.168.0.0/24
第八个问题问你是否开启NAT允许内网机器访问外网，这里当然要了，选Yes
第九个问题问你允许哪些子网或者主机访问外网，留空表示允许所有内网设备访问外网。
第十个问题问你是否重启防火墙，选Yes
```



[Want Faster, Easier-to-Manage DNS? Use Dnsmasq](https://medium.com/linode-cube/want-faster-easier-to-manage-dns-use-dnsmasq-a02517234d5f)

[shadowsocks透明代理](https://lala.im/6431.html)
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

[使用 Debian 10 搭建旁路代理网关/透明代理](https://pengjiayou.com/debian-10-transparent-gateway)

[Linux 将debian配置为软路由](https://www.hotbak.net/key/debian%20%E8%BD%AF%E8%B7%AF%E7%94%B1.html)

[DNSCrypt简明教程](https://jiajunhuang.com/articles/2019_10_31-dnscrypt.md.html)

[使用Debian9自己打造一个旁路由](https://lala.im/5727.html)

[使用Ubuntu 18.04打造超级家庭网关（边缘路由器）](https://www.johnrosen1.com/ubuntu-router/)

[如何在 Linux 上使用网络配置工具 Netplan](https://zhuanlan.zhihu.com/p/46544606)

[配置ubuntu搭建一个路由器](https://blog.csdn.net/u012174021/article/details/45369457?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

[Ubuntu Server（18.04）开启路由转发搭建软路由](https://blog.csdn.net/Splend520/article/details/86505569)

[https://www.right.com.cn/forum/thread-220005-1-1.html](https://www.right.com.cn/forum/thread-220005-1-1.html)

[Linux 软路由单线多拨](https://www.zfl9.com/multi-wan-router.html)

[使用 iptables 透明代理 TCP 与 UDP](https://blog.lilydjwg.me/2018/7/16/transparent-proxy-for-tcp-and-udp-with-iptables.213139.html)
[基于Ubuntu的软路由教程（单臂）](基于Ubuntu的软路由教程（单臂）)

[ss-redir+dnsmasq+iptables 透明代理](https://ivo-wang.github.io/2019/04/18/ss-redir+dnsmasq+iptables-%E9%80%8F%E6%98%8E%E4%BB%A3%E7%90%86/)

[使用 bind9 配合 dnscrypt-proxy 搭建自己的无污染的DNS服务器](https://oing9179.github.io/blog/2016/201609-Clean-DNS-Server-Setup-Using-bind9-and-dnscrypt-proxy/)

[关于软路由的一些配置](https://darkof.me/2019/07/07/hulu-configuration/)

[x86 软路由透明代理构建方案](https://0x01.io/2017/04/01/x86-%E8%BD%AF%E8%B7%AF%E7%94%B1%E9%80%8F%E6%98%8E%E4%BB%A3%E7%90%86%E6%9E%84%E5%BB%BA%E6%96%B9%E6%A1%88/#more)


