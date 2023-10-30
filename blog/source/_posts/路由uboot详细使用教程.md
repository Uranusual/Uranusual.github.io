---
title: 路由uboot详细使用教程
date: 2023-04-17 11:05:09
tags:
---
"My New Post"

# uboot使用教程

## 教程声明

1. 教程只提供uboot的使用教程，该教程只适用于你的路由器设备已经有了uboot的功能。
2. 教程分为5节内容，分别为：1.下载用于reboot的固件、2.设置用于操作reboot的设备的ip设定、3.打开路由器的reboot功能、4,登录reboot界面并实现刷固件操作、5.恢复设备的ip设定来正常访问网络。2.3顺序不分先后

## 图文教程

1. 下载好reboot用的系统固件（已经下好的可以跳过）
   这里以glinet的sft1200为例:

   1. 打开GL.iNET的官方固件下载中心[GL.iNet download center (gl-inet.com)](https://dl.gl-inet.com/)
   2. 搜索栏输入sft1200,选择到你的固件sft1200
   3. 选择用于reboot固件的下载地址下载即可
      ![下载固件](../src/%E8%B7%AF%E7%94%B1uboot%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/%E4%B8%8B%E8%BD%BDsft1200%E5%AE%98%E6%96%B9reboot%E5%9B%BA%E4%BB%B6.png)
2. 配置进行reboot操作的设备的ip地址：设置为192.168.1.2 (以winodws设备演示，没用手机reboot过不清楚能否用wifi来reboot)

   1. 进入网络连接管理界面
      下面为进入该界面的2中方式

   - 打开控制面板\网络和 Internet\网络连接
   - 设置-网络和 Internet-高级网络设置-更多适配器选项

   2. 进入适配器ip地址设置界面:右键点击以太网，再点击设置，点击internet协议版本4（TCP/IPv4）
   3. 点击适用下面的IP地址,设置ip地址为用于uboot的ip地址:192.168.1.2(最后一个数字2-255好像都行)，点上默认设置的子网掩码255.255.255.0,最后再点上确定使设置生效
      ![img](../src/%E8%B7%AF%E7%94%B1uboot%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/%E8%AE%BE%E7%BD%AEip%E5%9C%B0%E5%9D%80.png)
      操作的设备开了代理、vpn的请关闭代理和vpn相关的设置和软件,如果windows电脑开了防火墙，也要把它关了，如果能进操作页面这些就不用管，进不了就看看是不是这里出问题了
3. 打开路由器的reboot功能
   关闭路由器,一直按住路由器reboot按钮再同时开启路由器，一直按住reboot等路由器指示灯表示出reboot模式开启时再松开(sft1200和MT3000是常亮灰白色的灯)，这个时候路由器就开启了reboot功能
4. 登录uboot界面并实现刷固件操作
   在操作yboot的设备打开网址192.168.1.1，出现reboot的界面
   将reboot文件上传并安装即可
   ![img](../src/%E8%B7%AF%E7%94%B1uboot%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/%E4%B8%8A%E4%BC%A0reboot%E5%9B%BA%E4%BB%B6.png)
   出现以下界面表示路由器正在执行reboot,这个时候这个界面就可以关闭了,等待出现wifi出现默认的路由器名称或者用于reboot的设备可以进入路由器后台就表示uboot已经执行完毕了
   ![img](../src/%E8%B7%AF%E7%94%B1uboot%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/%E5%9B%BA%E4%BB%B6%E4%B8%8A%E4%BC%A0%E5%AE%8C%E6%AF%95.png)
5. 恢复ip设置为自动获取以正常上网
   将之前设置ip地址的选项改为自动获取ip地址
   ![img](../src/%E8%B7%AF%E7%94%B1uboot%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/%E6%81%A2%E5%A4%8Dip%E4%B8%BA%E8%87%AA%E5%8A%A8%E8%8E%B7%E5%8F%96.png)
