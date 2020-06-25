---
layout: post
title: 申请和使用Google云计算引擎配置SS Server
categories: [google cloud, shadowsocks]
description: 无法在墙内po出来，SS入门时写的记录
keywords: google cloud, shadowsocks
furigana: true
---

[Google Cloud Platform](https://cloud.google.com/)的新用户可以获得$300赠金的一年使用权，使用这笔不菲的赠金，我们可以构建应用程序、搭建网站、存储数据、体验各种强大的API。这次，我总结了使用Google云计算引擎搭建SS服务器，实现科学上网的过程，也算作为墙内使用谷歌云平台的第一步。

# 科学上网的基本原理

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotxvw10lnj30b908lt9s.jpg)

我们只需要一个能够访问墙外目标地址的代理服务器。本地设备向服务器发送访问目标地址的请求，服务器收到请求后访问目标并将结果回传给本地设备。

我们是使用Shadowsocks（简称SS，中文名影梭）来配置服务器的，所以一般把这个服务器称为SS服务器。谷歌云平台提供的位于国外的云计算引擎可以用来搭建SS服务器。

# 申请试用谷歌云平台

***重要：你需要一张外币信用卡（VISA/MasterCard/JCB）**

首先，翻墙。突然有种鸡生蛋，蛋生鸡的矛盾，不过我相信你能找到一个免费试用的VPN。

登录[谷歌云平台](https://cloud.google.com)，点击右上角的申请试用后进入申请界面。地区可以选择中国，不影响后续的申请。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotyf40bi9j30b00aymy2.jpg)

账号类型选择“个人”，填写名称地址电话。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotyl3w3anj30dk09ujsp.jpg)

付款方式填写你的外币信用卡（单币银联卡无效）。提交后信用卡会扣除1美金进行验证，验证完成即退回。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotynitahxj30dt07ft9n.jpg)

# 创建计算引擎

进入控制台，首先要求创建一个项目，尽量使用简单易记的项目名。

项目创建完成后，点击控制台左上角的 `☰` 打开导航栏，找到 `Compute Engine` → `VM实例` ，点击 `创建` 开始创建一个计算引擎。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotz3od46zj30j309eabi.jpg)

`区域` 有3个比较好的选择：

``` nohighlight
asia-east1：位于台湾

asia-southeast1：位于新加坡

asia-northeast1：位于东京
```

从国内ping延迟都在100ms左右，它们的流量费用和硬件费用有细微的差别，在意的朋友可以在[这里](https://cloud.google.com/compute/pricing)查询。

`机器类型` 可以选择最小的微型（1个共享vCPU，0.6GB内存）以节省硬件费用，单作为SS服务器该配置已经足够。

其他设置可以保持默认。点击 `创建` 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotzuwl2gej30ep0acwgb.jpg)

# 配置SS服务器

创建完成后可以看到分配给实例的 **`外部IP`** ，请牢记。

点击云引擎后面的 `SSH` ，远程连接该主机，进行配置。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fou05u93clj30cw04yweq.jpg)

这里使用[秋水逸冰](https://teddysun.com/358.html)大大的SS服务器配置脚本。

依次输入下面三条指令：

``` nohighlight
wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/wjk199511140034/ss-onekeyinstall/master/shadowsocks.sh

sudo chmod +x shadowsocks.sh

sudo ./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

第三条指令运行后即进入配置过程，需要根据提示输入几项信息。

Please input password for shadowsocks-libev：输入 **`密码`** ，请牢记

Please enter a port for shadowsocks-libev：输入SS **`服务器端口号`** ，请牢记

Which cipher you'd select：选择一种 **`加密方式`** ，请牢记

按任意键开始执行脚本，等待脚本运行完毕。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fou5nkwqofj30gs07d768.jpg)

# 创建防火墙规则

点击控制台左上角的 `☰` 打开导航栏，找到 `VPC网络` → `防火墙规则` ，点击 `创建防火墙规则` 创建如下2个规则。

1. 入站规则

`流量方向` ：入站

`目标` ：网络中的所有实例

`来源 IP 地址范围` ：0.0.0.0/0

`协议和端口` ：全部允许

   其他部分可以保持默认，这条规则表示允许所有ip/端口的所有协议入站。

2. 出站规则

`流量方向` ：出站

`目标` ：网络中的所有实例

`来源 IP 地址范围` ：0.0.0.0/0

`协议和端口` ：全部允许

   其他部分可以保持默认，这条规则表示允许所有协议出站到所有ip/端口。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fou5nhwhkuj30hx07eq4a.jpg)

至此，SS服务器部署完毕。你可以关闭你不稳定的试用版VPN，准备开始正确地科学上网了。

# 使用SS客户端

这里仅以Windows客户端为例，Android端很相似。Debian平台使用SS客户端则需要进行一些配置，将另外做一次总结。

可以在GitHub下载到Windows平台的[SS客户端](https://github.com/shadowsocks/shadowsocks-windows/releases)。

如果你无法打开GitHub，可以点击[这里](https://share.weiyun.com/edaa2c5f08aa5169c2be5c6a9f59662d)，前往微云下载，但不保证是最新版本。

请将可执行程序放置在合适的文件夹内，运行后会在程序同一目录下产生配置文件，如果随便放置容易显得杂乱。

第一次打开SS客户端会主动要求编辑服务器。填入你的 **`外部IP`** **`密码`** **`服务器端口号`** **`加密方式`** ，其他设置可以保持默认。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fou6bi1tebj309z09l3zf.jpg)

最后，右击任务栏的小图标，勾选 **`启用系统代理`** 。系统代理模式选择 **`PAC模式`** ，这样SS会自动使用代理访问墙外站点，不需要另外安装浏览器的代理插件。

![](http://ww1.sinaimg.cn/mw690/005MY9Xigy1fou6nucjagj3059065t8t.jpg)

# 参考资料

[Debian下shadowsocks-libev一键安装脚本](https://teddysun.com/358.html)

[Shadowsocks Troubleshooting](https://teddysun.com/399.html)

[Shadowsocks原理和搭建](http://blog.021xt.cc/archives/98)

[Google Cloud服务免费申请试用以及使用教程](https://51.ruyo.net/2144.html)

感谢你阅读文章！
