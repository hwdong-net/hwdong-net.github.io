---
layout:       post
title:        "2026 最新搭建VPS教程：2分钟用S-UI面板搭建VPS节点Realitty "
subtitle:     "2026 Vps S-UI"
date:         2026-04-19 00:13:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - it
---

# VPS服务器搭建过程：

## 用SSH登录VPS服务器，安装S-UI面板

### 第一步：先获取 VPS 登录信息（IP + root 密码）

1. 打开浏览器，登录VPS 官网
2. 登录你的账号 → 进入 **控制台 / 我的产品 / 云服务器**（或直接点“我的云服务器”）。
3. 找到你购买的那台 VPS，点击进入**详情/管理页面**。
4. 这里就能看到：
   - **公网 IP 地址**
   - **SSH 端口**（默认是 **22**）
   - **root 用户名**
   - **root 密码**（初始密码通常在购买成功邮件里；如果没看到或忘记，页面上有 **重置密码 / Change Password** 按钮，点一下立刻生成新密码）

**小贴士**：密码是随机字符串，建议登录成功后立刻改成自己好记的（后面会教）。

---

### 第二步：Windows 用户连接 SSH（官方推荐 + 两种方式）

#### 方式一：官方推荐 —— ATerminal（最简单，图形界面）

1. 打开浏览器，进入 **https://aterminal.net/** 下载最新版 `.exe` 安装包。
2. 双击安装（一路下一步即可，绿色免费）。
3. 打开 ATerminal：
   - **Host Name (or IP address)**：填你的 VPS **IP 地址**
   - **Port**：填 **22**（默认）
   - 左侧导航栏点击 **SSH → Auth**（如果用密码登录就不用选密钥）
   - 返回主窗口，点击 **Open**
4. 第一次连接会弹出安全警告，点击 **Yes**（信任服务器）。
5. 在弹出的窗口输入：
   - 用户名：`root`
   - 密码：从面板复制的 root 密码（输入时不会显示字符，直接回车）
6. 看到 `root@服务器名:~#` 就成功登录了！

#### 方式二：Windows 自带 SSH（不用装任何软件，最轻量）

1. 按 `Win` 键搜索打开 **Windows Terminal**（Win11 自带，Win10 也可以用 PowerShell）。

   搜索里输入："**cmd**，打开**Windows Terminal**（windows终端）

2. 复制下面命令粘贴（把 `你的VPS-IP` 换成实际 IP）：

   ```
   ssh root@你的VPS-IP
   ```

   示例：

   ```
   ssh root@123.45.67.89
   ```

   或

   ```
   ssh root@123.45.67.89 -p 22
   ```

   "-p 22"指定SSH服务的端口号为22（通常SSH服务的端口号默认是22，所以也可以省略 -p 22）

3. 第一次会提示 `Are you sure you want to continue connecting (yes/no)?` → 输入 `yes` 回车。

4. 输入 root 密码（不会显示字符），直接回车。

5. 看到提示符就登录成功！

---

### 第三步：登录成功后立即做的事（强烈建议）

1. **修改 root 密码**（更安全）：

   ```
   passwd
   ```

   输入两次新密码（不会显示）。

2. **更新系统**（后面装 Sing-Box-Plus 需要）：

   ```
   apt update && apt upgrade -y
   ```

3. 想退出连接：输入 `exit` 回车。

---


### 第四步：安装S-UI

[https://github.com/alireza0/s-ui](https://github.com/alireza0/s-ui)

登录SSH后，输入下面命令（在刚才的cmd窗口里，点击鼠标右键黏贴下面的命令内容。）：

```
bash <(curl -Ls https://raw.githubusercontent.com/alireza0/s-ui/master/install.sh)
```


当出现：

```
s-ui/
s-ui/sui
s-ui/s-ui.service
s-ui/s-ui.sh
Migration...
Database not found
Install/update finished! For security it's recommended to modify panel settings
Do you want to continue with the modification [y/n]? :
```

直接**按下回车键**。

接着复制 “**Global address**(全局地址)”和“管理员用户名和密码（**First admin credentials**）”，将它们保存下来：

```
if you forgot your login info,you can type s-ui for configuration menu
reset admin credentials success
First admin credentials:
        Username:        8dfdsdfk
        Password:        fdnjsdkf
Created symlink /etc/systemd/system/multi-user.target.wants/s-ui.service → /etc/systemd/system/s-ui.service.
s-ui v1.2.2 installation finished, it is up and running now...
You may access the Panel with following URL(s):
Local address:
http://123.45.67.89:2095/app/

Global address:
http://123.45.67.89:2095/app/
```

接着复制下面的内容：

```
for d in www.microsoft.com cdn.bizibly.com cdn77.api.userway.org gray-config-prod.api.arc-cdn.net amd.com tag.demandbase.com www.tesla.com statici.icloud.com prod.log.shortbread.aws.dev electronics.sony.com ; do t1=$(date +%s%3N); timeout 1 openssl s_client -connect $d:443 -servername $d </dev/null &>/dev/null && t2=$(date +%s%3N) && echo "$d: $((t2 - t1)) ms" || echo "$d: timeout"; done
```

在刚才的cmd窗口里，点击鼠标右键黏贴上面的内容，并按回车键执行。

将延迟最低的域名保存下来：

```
www.microsoft.com: 23 ms
cdn.bizibly.com: 24 ms
cdn77.api.userway.org: 24 ms
gray-config-prod.api.arc-cdn.net: 31 ms
amd.com: 20 ms
tag.demandbase.com: 56 ms
www.tesla.com: 16 ms
statici.icloud.com: 18 ms
prod.log.shortbread.aws.dev: 29 ms
electronics.sony.com: 91 ms
root@jp4-20260420164904c8ff15:~#
```

### 第五步：浏览器里打开S-UI面板

在浏览器里输入服务器地址: http://123.45.67.89:2095/app/ 。打开S-UI登录界面

将登录窗口的语言从"English"改为“中文”

在打开页面里输入刚才保存的用户名和密码：

进入后将“信息卡”的所有开关全打开，可以观察VPS服务器信息。

你也可以在左栏的最下面管理员头像那里修改你的S-UI管理员的用户名和密码

## 开始节点的设置过程

### 设置节点

#### 添加tls模板：
点击左栏的tls setting,点击“Add（添加）”，做下面的设置：
 - 名称随便起，比如“tls”
 - 点击右边的钥匙图标，自动生成密钥。
 - 将“允许不安全”打开
 - tls选项，将SNI和ALPN打开
 - SNI里黏贴前面访问快的域名，比如: amd.com
 - 点击保存

#### 添加Hysteria2模板：
继续点击“Add（添加）”，做下面的设置：
 - 名称随便起，比如“hy2_tls”
 - 点击右边的钥匙图标，自动生成密钥。
 - 将“允许不安全”打开
 - tls选项，将SNI打开
 - SNI里黏贴前面访问快的域名，比如: amd.com
 - 点击保存
   
#### 添加Reality模板：
继续点击“Add（添加）”，做下面的设置：
 - 右上角 鼠标点击Reality，切换到 Reality
 - 名称随便起，比如“reality”
 - SNI和握手服务器里黏贴前面访问快的域名，比如:amd.com
 - 端口可以填：443
 - 点击右边的钥匙图标，自动生成密钥。
 - 浏览器指纹可以随便选择某个浏览器
 - 点击保存



### 入站管理(inbounds)：添加节点协议

1. 添加 Reality入站节点

点击左栏的“入站管理”，点击“添加”
 - 类型选择“vless”，名称可以随便设置，如“vless_reality”
 - 端口号改成443。 和前面的reality里设置一样
 - 模板（template）里选择 reality
 - 点击保存

2. 添加Hysteria2入站节点
继续点击“添加”
 - 类型选择“Hysteria2”，名称可以随便设置，如“Hysteria2-123456”
 - 端口号改成443。 
 - 模板（template）里选择 hy2_tls
 - 点击保存

2. 添加tuic入站节点
继续点击“添加”
 - 类型选择“tuic”，名称可以随便设置，如“tuic-1”
 - 端口号可以用随机生成
 - 拥塞控制选择 bbr 
 - 模板（template）里选择tls
 - 点击保存

### 添加用户

点击 “用户管理”，
- “入站标签”里勾选前面所有的入站协议
- 点击“保存”

  点击二维码，就可以看到 节点的扫描二维码或链接
  
## 客户端导入节点（导入上述链接即可）
Windows系统用V2rayN，安卓用V2rayNG。

推荐代理工具

Windows/Mac（v2rayN）：[https://github.com/2dust/v2rayN/releases/tag/7.12.7](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tLzJkdXN0L3YycmF5Ti9yZWxlYXNlcy90YWcvNy4xMi43)
Android（NekoBox）：[https://github.com/MatsuriDayo/NekoBoxForAndroid/releases/tag/1.3.9](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tL01hdHN1cmlEYXlvL05la29Cb3hGb3JBbmRyb2lkL3JlbGVhc2VzL3RhZy8xLjMuOQ)
IOS/Mac（shadowrocket）：[https://apps.apple.com/app/shadowrocket/id932747118](https://bulianglin.com/g/aHR0cHM6Ly9hcHBzLmFwcGxlLmNvbS9hcHAvc2hhZG93cm9ja2V0L2lkOTMyNzQ3MTE4)

## 验证
客户端连接eiVPS服务器后，在浏览器访问网站：

```
https://whatismyipaddress.com
```
如果显示的ip地址和你的VPS服务器一致，说明你的 Reality连接成功了!

### 参考：

[https://www.youtube.com/watch?v=6l01iAgKglY](https://www.youtube.com/watch?v=6l01iAgKglY)

[https://timely-vacherin-fcf11a.netlify.app/](https://timely-vacherin-fcf11a.netlify.app/)


