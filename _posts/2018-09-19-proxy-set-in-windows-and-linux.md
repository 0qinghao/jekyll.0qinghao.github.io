---
layout: post
title: 在Windows命令行、Linux终端使用代理
categories: [proxy, shadowsocks, linux]
description: 在命令行配置代理的方法
keywords: shadowsocks, proxy
furigana: true
---

在之前的博文中分享了如何[使用Google云计算引擎搭建SS服务器](https://segmentfault.com/a/1190000013399064)，如何[使用SS客户端](https://segmentfault.com/a/1190000013539547)，已经满足了科学上网的基本需求。这次将要总结在Windows的 `CMD` 窗口和Linux的 `LX终端` 中，让 `wget`  `curl` 等命令使用代理需要进行的一些配置。

# Windows命令行代理

假设你已经使用了SS客户端，本地socks5代理为127.0.0.1:1080

在CMD窗口输入如下指令设置代理：

``` nohighlight
set http_proxy=socks5://127.0.0.1:1080
set https_proxy=socks5://127.0.0.1:1080
set ftp_proxy=socks5://127.0.0.1:1080
```

测试 `curl https://www.facebook.com` 能得到返回结果。

![](http://ww1.sinaimg.cn/mw690/005MY9Xigy1fp4azce62uj30hd075t9e.jpg)

取消代理命令：

``` 
set http_proxy=
set https_proxy=
set ftp_proxy=
```

*****设置代理后只对当前命令行窗口生效，重新打开CDM需要再次设置。

# Linux LX终端代理

由于Linux下SS客户端仅代理socks5协议的流量（如果不是这个原因恳请指正）。所以想在LX终端使用代理，需要在SS的socks5流量前再接一个代理，允许http、https、ftp协议流量通过。

我们也假定本地socks5代理为127.0.0.1:1080

## 安装polipo

Debian下直接使用apt命令安装：

``` shell
sudo apt update
sudo apt install polipo
```

编辑配置文件：

``` nohighlight
sudo nano /etc/polipo/config
```

配置内容如下

``` nohighlight
# This file only needs to list configuration variables that deviate
# from the default values.  See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.

logSyslog = true
logFile = /var/log/polipo/polipo.log

proxyAddress = "0.0.0.0"

socksParentProxy = "127.0.0.1:1080"
socksProxyType = socks5

chunkHighMark = 50331648
objectHighMark = 16384

dnsQueryIPv6 = no
```

按<kbd>CTRL</kbd>+<kbd>X</kbd>，<kbd>Y</kbd>保存退出。

重启polipo服务：

``` shell
sudo service polipo restart
```

## 启用代理

通过 `service polipo status` 命令，我们可以看到新的监听端口为**8123**。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fp4g22nbr8j30f905ddhd.jpg)

因此，LX终端启用代理的命令为：

``` nohighlight
export http_proxy=http://127.0.0.1:8123
export https_proxy=http://127.0.0.1:8123
export ftp_proxy=http://127.0.0.1:8123
```

同样，直接输入上述命令设置的代理也是临时的。一个比较实用的方法是在~/.bashrc文件中设置环境，之后就不需要再手动设置了。

``` shell
sudo nano ~/.bashrc
```

在文件最后插入上述三条指令，保存。

测试 `wget` 指令：

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fp4gl2vchsj30er05a3z3.jpg)

# 小结

我对CMD/LX终端设置代理的出发点，是为了使用Google的一个API，设置后确实能够成功使用。另外似乎对 `pip` 等指令也有效果，安装python模块时的下载速度有比较明显的提升。不过说到底只是在总结如何使用别人做好的工具，很多原理还是没有明白，如果文中有何纰漏，恳请指正。

感谢你阅读文章！
