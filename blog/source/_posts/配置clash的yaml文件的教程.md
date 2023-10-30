---
title: 配置clash的yaml文件的教程
date: 2023-04-18 08:00:52
tags:
---
# clash配置文件的自定义教程

## 为什么要学会配置clash的yaml文件？

学会配置yaml文件可以实现以下内容

- 可以同时订阅多个机场
- 可以先个性化节点分流(而且大部分机场不带分流策略)
- openclash好像必须要用yaml文件

## 各个客户端如何导入配置文件

### 一.clash for windows（windows/mac）

中文版:clash-配置-导入-你要导入的yaml配置文件-打开

![](../src/%E9%85%8D%E7%BD%AEclash%E7%9A%84yaml%E6%96%87%E4%BB%B6%E7%9A%84%E6%95%99%E7%A8%8B/yaml%20for%20clash%20for%20windows.png)

### 二.openclash(openwrt)
![](../src)
### 三.clash for Android
略
![](../src)
### 常见问题：

#### 导入失败的一些原因及解决方法:(不同客户端有差异)

- 因为网络原因，无法访问配置文件中的url
  先打开代理或vpn是电脑可以访问目标url或者将不能访问的url注释掉，再导入
- filter失败，因为你的节点中没用找到任何相关的关键词
  把相关filter注释掉


## 套用他人yaml文件（这里我提供了模板）
先看套用我的yaml的效果图:(其中不同软件或网站的分流是通过rule-providers和rules来实现的)
![](../src/%E9%85%8D%E7%BD%AEclash%E7%9A%84yaml%E6%96%87%E4%BB%B6%E7%9A%84%E6%95%99%E7%A8%8B/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%85%8D%E7%BD%AE%E5%90%8E%E7%9A%84%E6%95%88%E6%9E%9C.png)
1. 下载或复制他人的yaml文件
2. 将他人的配置添加到自己的配置中
   - 第一次配置的话,直接改别人的配置文件即可,这里以我提供的模板示范一下
     模板文件下载地址
   - 已经配置好自己的情况下,将对方的规则添加到自己的配置中
     添加相应的rules，rule-providers,将规则使用于proxies中

### 如何套用我提供的模板（弄不好可以先适用我提供的样例）
1. 先下载我提供的模板
   模板:[clash配置模板](clash%E9%85%8D%E7%BD%AE%E6%A8%A1%E6%9D%BF.yml)
   可直接使用的样例:[clash的yaml样例](clash%E9%85%8D%E7%BD%AE%E5%8F%AF%E7%9B%B4%E6%8E%A5%E7%94%A8%E7%9A%84%E6%A0%B7%E4%BE%8B%EF%BC%88%E8%AF%B7%E4%B9%8B%E5%90%8E%E8%87%AA%E5%B7%B1%E6%9B%B4%E6%8D%A2%E8%AE%A2%E9%98%85%E5%9C%B0%E5%9D%80%EF%BC%89.yml)


2. 打开我的模板,更改提供节点的链接为你的订阅链接
  1. 找到proxy-providers
  2. 把机场的名字以及url替换为你的
  例如：
  原模板：
  ```yaml
proxy-providers: #这个是载入获取代理策略的文件或者网址

  合并订阅: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅 #导入合并订阅的链接来查看所有机场的节点情况
    type: http
  
    url: 你合并订阅的地址格式应该是https://api.dler.io/sub?target=clash&url=https* #合并订阅的地址


    path: ./profiles/proxies/合并订阅.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  ikuuu: #自定义的名称 可以分别导入不同机场的url,来从clash观看不同的机场的节点情况
    type: http # 来源类型：有http和file
    url: ikuuu的订阅地址格式为https//*/ #地址来源
    path: ./profiles/proxies/ikuuu.yaml #不知道为什么不生效#这个省了安卓就载入不了配置
    interval: 300 #更新配置的时间间隔
    health-check: #用于检查配置网络情况的网站和间隔
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 600

  三分机场:
    type: http
    url: 三分机场的订阅地址,地址格式为https//*/
    path: ./profiles/proxies/三分机场.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300



  日本: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅 #先通过导入合并订阅链接，再筛选出某个国家的节点，以达到按国家选择节点，再用url-test按照延迟来选择地区延迟最低的节点
    type: http
    url: 你合并订阅的地址格式应该是https://api.dler.io/sub?target=clash&url=https* #合并订阅的地址
    path: ./profiles/proxies/我的机场们.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300
    filter: '日本|japan' # 删选出含有该关键词的节点,而且被筛选关键字必须要有一个节点,不如windows版无法导入配置

  台湾: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅
    type: http
    url: 你合并订阅的地址格式应该是https://api.dler.io/sub?target=clash&url=https* #合并订阅的地址
    path: ./profiles/proxies/台湾.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300
    filter: '台湾|台北' # 删选出含有该关键词的节点
  ```
  替换后
  ```yaml
proxy-providers: #这个是载入获取代理策略的文件或者网址

  合并订阅: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅 #导入合并订阅的链接来查看所有机场的节点情况
    type: http
  
    url: https://api.dler.io/sub?target=clash&url=https%3A%2F%2Fsub1.smallstrawberry.com%2Fapi%2Fv1%2Fclient%2Fsubscribe%3Ftoken%3Dc4e8691523351ffe16ea678f32410548%26flag%3Dclash #这是我免费给你们提供的一个机场,这里就合并了一个订阅链接


    path: ./profiles/proxies/合并订阅.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  一元机场:
    type: http
    url: https://sub1.smallstrawberry.com/api/v1/client/subscribe?token=c4e8691523351ffe16ea678f32410548&flag=clash #都免费给你们用来，这还不投个币？
    path: ./profiles/proxies/一元机场.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300



  日本: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅 #先通过导入合并订阅链接，再筛选出某个国家的节点，以达到按国家选择节点，再用url-test按照延迟来选择地区延迟最低的节点
    type: http
    url: https://api.dler.io/sub?target=clash&url=https%3A%2F%2Fsub1.smallstrawberry.com%2Fapi%2Fv1%2Fclient%2Fsubscribe%3Ftoken%3Dc4e8691523351ffe16ea678f32410548%26flag%3Dclash #这是我免费给你们提供的一个机场,这里就合并了一个订阅链接
    path: ./profiles/proxies/我的机场们.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300
    filter: '日本|japan' # 删选出含有该关键词的节点,而且被筛选关键字必须要有一个节点,不如windows版无法导入配置

  台湾: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅
    type: http
    url: https://api.dler.io/sub?target=clash&url=https%3A%2F%2Fsub1.smallstrawberry.com%2Fapi%2Fv1%2Fclient%2Fsubscribe%3Ftoken%3Dc4e8691523351ffe16ea678f32410548%26flag%3Dclash #这是我免费给你们提供的一个机场,这里就合并了一个订阅链接
    path: ./profiles/proxies/台湾.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300
    filter: '台湾|台北' # 删选出含有该关键词的节点
  ```

3. 保存后直接导入配置,如果报错就把相应的地区节点个注释掉，以及使用地区节点的策略组中把该节点注释掉
- 点击在文本模式下编辑 对导入后的配置文件进行修改了解决报错问题
- 也可以用文本编辑器像记事本vscode打开配置文件进行修改

4. 最后,如果格式正确的话,哪里报错就把相关的内容注释[常见报错](#常见问题)

## 合并订阅机场的方法
打开 https://sub.dler.io网址 点击基础模式 在订阅链接那信息栏中每行填入你要合并的订阅链接 再点击生成订阅链接
![](../src/%E9%85%8D%E7%BD%AEclash%E7%9A%84yaml%E6%96%87%E4%BB%B6%E7%9A%84%E6%95%99%E7%A8%8B/%E5%90%88%E5%B9%B6%E8%AE%A2%E9%98%85%E8%8A%82%E7%82%B9.png)
## yaml文件中各个内容详细介绍

### 一.各个板块

1. 基础内容 (这些内容刚上手直接复制就行了)

```yml
   port: 7890 #端口

socks-port: 7891 #socks5端口

allow-lan: true #允许局域网连接

mode: Rule #模式

log-level: info #日志级别

external-controller: 127.0.0.1:9090 #控制面板地址

dns: #dns设置

  enable: true

  nameserver:

    - 223.5.5.5

    - 119.29.29.29

  fallback:

    - https://doh.buzz:8000/dns-query

    - https://doh.beauty:8000/dns-query

    - https://cloudflare-dns.com/dns-query

    - tls://1.1.1.1:853

    - tls://1.0.0.1:853

    - https://1.1.1.1/dns-query

    - https://1.0.0.1/dns-query

    - tls://8.8.8.8:853

    - tls://8.8.4.4:853

    - https://dns.google/dns-query

    - https://dns.twnic.tw/dns-query

  fallback-filter:

    geoip: true

    geoip-code: CN

    ipcidr:

    - 240.0.0.0/4

    domain:

    - +.google.com

    - +.facebook.com

    - +.youtube.com

  nameserver-policy: ~

cfw-bypass: #要直接通过的一些域名的设置，这些一些都是自己区域可以通过的域名和ip

- qq.com
- music.163.com
- "*.music.126.net"
- localhost
- 127.*
- 10.*
- 172.16.*
- 172.17.*
- 172.18.*
- 172.19.*
- 172.20.*
- 172.21.*
- 172.22.*
- 172.23.*
- 172.24.*
- 172.25.*
- 172.26.*
- 172.27.*
- 172.28.*
- 172.29.*
- 172.30.*
- 172.31.*
- 192.168.*
- <local>

cfw-latency-timeout: 5000  #延迟超时时间
```

2. proxy-providers
可以用这个配置项来选择或显示机场的提供的所有节点、通过合并订阅来显示和选择所有合并了的机场的所有节点、通过筛选将所有的节点按照地区进行分类

```yml
proxy-providers: #这个是载入获取代理策略的文件或者网址

  合并订阅: #目前好像只有proxy-providers会筛选节点 所以就写到这里，而且好像只能用一个地址，需要合并订阅 #导入合并订阅的链接来查看所有机场的节点情况
    type: http
  
    url: 你合并订阅的地址格式应该是https://api.dler.io/sub?target=clash&url=https* #合并订阅的地址

    path: ./profiles/proxies/合并订阅.yaml
    interval: 600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300
  ikuuu: #自定义的名称 可以分别导入不同机场的url,来从clash观看不同的机场的节点情况
    type: http # 来源类型：有http和file
    url: ikuuu的订阅地址格式为https//*/ #地址来源
    path: ./profiles/proxies/ikuuu.yaml #不知道为什么不生效#这个省了安卓就载入不了配置
    interval: 300 #更新配置的时间间隔
    health-check: #用于检查配置网络情况的网站和间隔
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 600

```
以下是我实现的效果图:(其中不同软件或网站的分流是通过rule-providers和rules来实现的)
![](../src/%E9%85%8D%E7%BD%AEclash%E7%9A%84yaml%E6%96%87%E4%BB%B6%E7%9A%84%E6%95%99%E7%A8%8B/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%85%8D%E7%BD%AE%E5%90%8E%E7%9A%84%E6%95%88%E6%9E%9C.png)
3. rule-providers
如果有机场订阅不了的，可以在链接结尾加上flag=clash试试,一元机场就是这样的,合并订阅的链接也要加上，不如加上了但是连不上
```yml
rule-providers: #使用在线规则集来对不同的地址使用不同的节点
  Spotify: #这里包含spotify应用、网站的所有ip地址和域名
    behavior: classical #规则集的行为
    interval: 86400 #更新间隔
    path: ./Rules/Spotify #规则集的本地保存路径
    type: http #规则集的类型
    url: 'https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Spotify/Spotify.yaml' #确保自己的网络可以访问该地址,不然导入配置时因为访问不了导致导入配置导入失败,所以尽量选用国内的地址
  lancidr:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/lancidr.txt"
    path: ./profiles/rules/lancidr.yaml
    interval: 86400
```
1. proxies
这个我没用上,所以就略过了
1. proxy-groups
  这些都不用抄，自己借鉴一下
```yml
proxy-groups: #这些内容时显示在控制面板进行节点选择的内容
  - name: 可选择的所有节点 #策略组名称
    type: select #该策略组选择节点的方式，这里为自主选择
    # type: url-test #select自主选择url-test自动选择
    use: #使用的来源
      - 合并订阅
      # - 三分机场
      # - ikuuu
    # url: http://www.gstatic.com/generate_204 #测试节点的地址
    # interval: 300 #测试节点的间隔

  - name: ikuuu节点
    type: select
    use: #使用的来源
      - ikuuu

  - name: 三分机场节点
    type: select
    use: #使用的来源
      - 三分机场

  # - name: 一元机场节点 #（不能直接导入）
  #   type: select
  #   use: #使用的来源
  #     - 一元机场

  # - name: 解锁节点
  #   type: select
  #   use: 
  #     - 解锁

  - name: 🔰 选择节点
    type: select
    proxies:
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点
      - 🇸🇬新加坡节点
      - DIRECT


  - name: 🎬Netflix
    type: select
    proxies:
      # - 解锁节点
      - 🇺🇸美国节点
      - 🔰 选择节点
      - DIRECT

  - name: 🎮 Steam商店
    type: select
    proxies:
      - 🔰 选择节点
      - DIRECT


  - name: 😼GitHub
    type: select
    proxies:
      - 🔰 选择节点
      - DIRECT
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点

  - name: 🔍必应
    type: select
    proxies:
      - 可选择的所有节点
      - 🔰 选择节点
      - DIRECT
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点

  - name: 🤖OpenAI
    type: select
    proxies:
      - 可选择的所有节点
      - 🔰 选择节点
      - DIRECT
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点


  - name: 🇨🇳 国内网站
    type: select
    proxies:
      - DIRECT
      - 🔰 选择节点

  - name: 局域网
    type: select
    proxies:
      - DIRECT
      - 🔰 选择节点
      - 可选择的所有节点
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点

      
  - name: 🍎苹果服务
    type: select
    proxies:
      - DIRECT
      - 🔰 选择节点
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点



  - name: 🛑 拦截广告
    type: select
    proxies:
      - REJECT
      - DIRECT
      - 🔰 选择节点
  - name: 🐟 漏网之鱼
    type: select
    proxies:
      - DIRECT
      - 🔰 选择节点
      
  - name: 🪟微软
    type: select
    proxies:
      - DIRECT
      - 🔰 选择节点
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点

  - name: 🎵spotify
    type: select
    proxies:
      - 🇹🇼台湾节点 #第一个节点时默认选择的
      - 🔰 选择节点
      - DIRECT
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      
      - 🇭🇰香港节点
  - name: 🇯🇵日本节点
    type: url-test
    use: #使用的来源
      - 日本
    url: http://www.gstatic.com/generate_204 #测试节点的地址
    interval: 120 #测试节点的间隔
  - name: 🇺🇸美国节点
    type: url-test
    use: #使用的来源
      - 美国
    url: http://www.gstatic.com/generate_204 #测试节点的地址
    interval: 120 #测试节点的间隔


#游戏策略
  - name: 🎮气球塔防6
    type: select
    proxies:
      - DIRECT
      - REJECT
      - 🔰 选择节点
      - 🇯🇵日本节点
      - 🇺🇸美国节点
      - 🇹🇼台湾节点
      - 🇭🇰香港节点
```
6. rules
```yaml
rules:
 - DOMAIN-SUFFIX,ninjakiwi.com:443,🎮气球塔防6
 - DOMAIN-SUFFIX,bing.net,🔍必应
 - DOMAIN-SUFFIX,bing.com,🔍必应
 - DOMAIN-SUFFIX,openai.com,🤖OpenAI
 - DOMAIN-SUFFIX,github.com,😼GitHub
 - DOMAIN-SUFFIX,github.io,😼GitHub
 - DOMAIN-SUFFIX,githubapp.com,😼GitHub
 - DOMAIN-SUFFIX,githubassets.com,😼GitHub
 - DOMAIN-SUFFIX,githubusercontent.com,😼GitHub
 - RULE-SET,Microsoft,🪟微软
 - RULE-SET,Spotify,🎵spotify
 - RULE-SET,lancidr,局域网

 - IP-CIDR,103.151.150.0/23,📺 爱奇艺&哔哩哔哩 #类型，地址，生效的策略组
 - DOMAIN,app.biliapi.net,📺 爱奇艺&哔哩哔哩
 - DOMAIN,grpc.biliapi.net,📺 爱奇艺&哔哩哔哩
 - DOMAIN,p-bstarstatic.akamaized.net,📺 爱奇艺&哔哩哔哩
 - DOMAIN,p.bstarstatic.com,📺 爱奇艺&哔哩哔哩
 - DOMAIN,upos-bstar-mirrorakam.akamaized.net,📺 爱奇艺&哔哩哔哩
 - DOMAIN,upos-bstar1-mirrorakam.akamaized.net,📺 爱奇艺&哔哩哔哩
 - DOMAIN,upos-hz-mirrorakam.akamaized.net,📺 爱奇艺&哔哩哔哩
 - DOMAIN-SUFFIX,acgvideo.com,📺 爱奇艺&哔哩哔哩
 - DOMAIN-SUFFIX,bilibili.com,📺 爱奇艺&哔哩哔哩
 - DOMAIN-SUFFIX,bilibili.tv,📺 爱奇艺&哔哩哔哩
 - IP-CIDR,45.43.32.234/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,103.151.150.0/23,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,119.29.29.29/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,128.1.62.200/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,128.1.62.201/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,150.116.92.250/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,164.52.33.178/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,164.52.33.182/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,164.52.76.18/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,203.107.1.33/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,203.107.1.34/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,203.107.1.65/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - IP-CIDR,203.107.1.66/32,📺 爱奇艺&哔哩哔哩,no-resolve
 - DOMAIN,cache.video.iqiyi.com,📺 爱奇艺&哔哩哔哩
 - DOMAIN-SUFFIX,iq.com,📺 爱奇艺&哔哩哔哩
 - DOMAIN,bahamut.akamaized.net,📺 动画疯
 - DOMAIN,gamer-cds.cdn.hinet.net,📺 动画疯
 - DOMAIN,gamer2-cds.cdn.hinet.net,📺 动画疯
 - DOMAIN-SUFFIX,bahamut.com.tw,📺 动画疯
 - DOMAIN-SUFFIX,gamer.com.tw,📺 动画疯
 - DOMAIN-SUFFIX,exhentai.org,🔰 选择节点
 - DOMAIN-SUFFIX,e-hentai.org,🔰 选择节点
 - DOMAIN-SUFFIX,ehgt.org,🔰 选择节点
 - DOMAIN-SUFFIX,forums.e-hentai.org,🔰 选择节点
```

