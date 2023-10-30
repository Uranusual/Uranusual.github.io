---
title: 从MT3000入门openwrt软路由系统
date: 2023-03-31 12:11:21
tags:
---
# 前言：
我是通过这款路由器入门的openwrt，所以我现在所写的教程更适合和我一样刚入门openwrt的人看，如果出现我没提到的问题，请自信搜索报错代码寻找解决方法

通过我半个月的折腾,我总结了一下这款路由容易装且实用openwrt的应用：

1. openclash(很多人入门都是为了这个翻墙功能吧)
2. adguardhome(有点去广告的效果，但不多,适合愿意多折腾的用户,glinet自带会使用就行)
3. smartDNS(加快进入网页的速度,安装使用简单,效果明显)
4. DDNS/内网穿透功能的应用：zerotier，tailscale(因为本人用校园网没有ipv6没有公网ip就只能体验一下内网穿透了)
5. 远程唤醒(用于在本地或远程唤醒电脑,上手十分简单)
6. Alist(管理挂载的usb移动硬盘/u盘,云端的百度网盘/阿里云盘来实现轻NAS的功能)
7. Aria2(用于离线下载到移动硬盘的下载功能,glinet的openwrt设置有问题需要自己手动配置一些设置才能正常实用,上手稍微麻烦)
8. 不推荐的应用：docker的一切应用。原因：glinet软件包空间有限，性能也有限，而docker占用空间很大，配置也不简单，软件包空间占满不仅不能添加新的应用，有些功能也会受限，本人就因为没剩余空间返回备份点失败于是路由器给重置了

# 基本操作方法
## 登录路由后台已经进入openwrt后台：
首先在浏览器地址输入：192.168.8.1进入glinet管理后台，设置好密码登录后，点击系统-高级设置-192.168.8.1/cgi-bin/luci，再输入你一开始设置的管理员密码即可进入

## 系统刷死后reboot
1. 下载好reboot用的系统固件
2. 配置进行reboot操作的设备的ip地址：设置为192.168.1.2
3. 操作的设备开了代理，vpn的请关闭这些设置和软件
4. 用lan口连接路由器和用于操作reboot的设备,然后关闭路由器
5. 一直按住路由器reboot按钮再同时开启路由器，一直按住reboot等路由器指示灯表示出reboot模式(sft1200是常亮灰白色的灯，MT3000好像进入一直常亮还是常黑的状态)开启时再松开
6. 在操作reboot的设备打开网址192.168.1.1，出现reboot的界面
7. 将reboot文件上传并安装即
   
## 安装应用：
这边不建议用glinet的软件包界面安装软件
这里有4种方式安装软件
### 通过图形化界面安装
在openwrt界面，点击系统-软件包到达软件包安装界面
可

#### 通过软件包列表安装软件
   更新列表，在筛选器搜索你想要安装的软件包，点击安装即可
   <!-- 如果更新列表失败，就配置opkg添加openwrt软件源
   src/gz tsinghua https://mirrors.tuna.tsinghua.edu.cn/openwrt -->
   示例：
   ![安装ttyd](../src/%E4%BB%8EMT3000%E5%85%A5%E9%97%A8openwrt%E8%BD%AF%E8%B7%AF%E7%94%B1%E7%B3%BB%E7%BB%9F/%E5%AE%89%E8%A3%85ttyd%E7%A4%BA%E4%BE%8B.png)
   软件包名字表示的意义,及安装效果：
   ttyd -通常表示为软件的核心 (核心)
   luci-app-ttyd -表示带可视化配置界面的完整的软件（核心+界面）
   luci-i18n-ttyd-zh-cn -表示含中文语言包的带可视化配置界面的完整的软件（核心+界面+语言包）
   这些不会重复安装
#### 上传软件包文件安装软件
   - 18版本及之前的openwrt界面没有入口直接上传软件包：
    windows电脑中下载winscp,用winscp远程scp连接openwrt，将安装包导入到openwrt的/tmp路径下，再执行opkg install tmp/你导入的安装包的名字.ipk
    - 

   
### 使用Windows终端/命令行/ttydSSH进入openwrt终端,输命令进行安装：
#### windows终端及命令行(较麻烦)
   ![配置设置](../src/%E4%BB%8EMT3000%E5%85%A5%E9%97%A8openwrt%E8%BD%AF%E8%B7%AF%E7%94%B1%E7%B3%BB%E7%BB%9F/ssh%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.png)
配置用户/用户名/.ssh文件,设置192.168.8.1登录的账号为root，在打开windows终端 输入ssh 192.168.8.1，回车再输入密码登录即可
#### 安装ttyd,使用远程网页终端操作openwrt的终端（上手简单）

### 实用istore安装

## 更新应用：
应及时更新的应用：luci-app-opkg luci-i18n-base-zh-cn luci-theme-bootstrap
更新这些可以解决某些文字未汉化的问题

# 实用软件安装及初始化教程

### openclash
项目地址：https://github.com/vernesong/OpenClash
#### 下载openclash中文版
写这个时openclash的最新版
https://github.com/vernesong/OpenClash/releases/download/v0.45.103-beta/luci-app-openclash_0.45.103-beta_all.ipk
#### 把openclash安装到MT3000中
详细内容请进我另外一篇文章看
1. 先安装好依赖(不这样做可能导致装不上或者系统后台死掉)
   ```Bash
   opkg update
   opkg install coreutils-nohup bash iptables dnsmasq-full curl ca-certificates ipset ip-full iptables-mod-tproxy iptables-mod-extra libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip luci-compat luci luci-base
   opkg install coreutils-nohup bash dnsmasq-full curl ca-certificates ipset ip-full libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip kmod-nft-tproxy luci-compat luci luci-base
   ```
2. 上传并安装软件包
   
3. 初始化并运行openclash
   1. 添加配置文件：这里用的是自己自定义的yaml配置文件,还不知道怎么直接用机场给的url
   2. 启动openclash
### smartDNS
#### 下载并安装smartDNS
#### 设置并启动smartDNS



### Aria2
因为glinet设置的问题
http://192.168.8.1/ariang/index.html可以打开，当时默认的http://192.168.8.1/ariang是打不开的 所以需要进行设置
#### 配置Aria2的nginx配置文件
ssh进入openwrt终端，执行命令：
```Bash
vim /etc/nginx/conf.d/aria2.location
```
进入文件后,按i进入编辑模式，输入内容
```Bash
location /ariang/ {
   root   /www/;
   index  index.html index.htm;
}
```
按 Esc 切换到普通模式，键入 :wq 并按 Enter，保存编辑内容

进入gl.conf配置文件
```Bash
vim /etc/nginx/conf.d/gl.conf
```
在最后追加内容
```Bash
include /etc/nginx/conf.d/*.location;
```
保存编辑内容后，
执行命令重新启动nginx
```Bash
nginx -s reload
```

<!-- ```Bash

``` -->
### Adguardhome
因为glinet已经安装好了adguardhome就不需要自己再手动安装了
#### 设置Adguardhome
##### DNS设置
##### 过滤规则设置
##### 客户端设置

### 微力同步

# 软件的搭配实用
以下是用mt3000设置的，不清楚自动的adguardhome有没有开启重定向，可能设置有出入
## DNS设置：（openclash+adguardhome+smartDNS）
方案：客户端(你上网的手机电脑)  传递域名 adguadhome 阻拦过滤域名,再把剩下的域名传递到→ openclash 
openclash 进行网络分流→ 翻墙服务器 访问服务器ip地址→ 返回响应的信息→翻墙服务器 传递响应信息→客户端
 解释：如果是由openclash再到adguardhome,有一些地址请求不会传达到adguardhome
### 各项设置
1. openclash:关闭本地DNS劫持
   openclash-Plugin Settings-DNS设置-本地DNS劫持-选择停用
   ![openclash设置](../src/%E4%BB%8EMT3000%E5%85%A5%E9%97%A8openwrt%E8%BD%AF%E8%B7%AF%E7%94%B1%E7%B3%BB%E7%BB%9F/ClashDNS%E8%AE%BE%E7%BD%AE.png)
2. smartDNS启用并记下端口默认为6053
3. Adguardhome启用,设置DNS上游为只剩一个的192.168.8.1:6053（192.168.8.1为你的路由器地址，6053为你的smartDNS端口）并记下adguardhome的监听端口默认为3053
   ![DNS转发设置](../src/%E4%BB%8EMT3000%E5%85%A5%E9%97%A8openwrt%E8%BD%AF%E8%B7%AF%E7%94%B1%E7%B3%BB%E7%BB%9F/DNS%E8%BD%AC%E5%8F%91%E8%AE%BE%E7%BD%AE.png)
4. DNS转发添加192.168.8.1#3053或者127.0.0.1#3053 (已经自动设置好就不用管了) 

