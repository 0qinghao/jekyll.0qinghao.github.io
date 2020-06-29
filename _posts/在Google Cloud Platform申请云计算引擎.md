[Google Cloud Platform](https://cloud.google.com/)的新用户可以获得$300赠金的一年使用权，使用这笔不菲的赠金，我们可以构建应用程序、搭建网站、存储数据、体验各种强大的API。这次，我总结了使用Google云平台申请计算引擎的过程。

# 申请试用谷歌云平台

***重要：你需要一张外币信用卡（VISA/MasterCard/JCB）**

首先，有种鸡生蛋，蛋生鸡的矛盾，不过我相信你能找到*一架梯子*。

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

`地区` 有3个比较好的选择：

```nohighlight
asia-east1：位于台湾

asia-southeast1：位于新加坡

asia-northeast1：位于东京
```

从国内ping延迟都在100ms左右，它们的流量费用和硬件费用有细微的差别，在意的朋友可以在[这里](https://cloud.google.com/compute/pricing)查询。

`机器类型` 可以选择最小的微型（1个共享vCPU，0.6GB内存）以节省硬件费用，单作为SS Server该配置已经足够。

其他设置可以保持默认。点击 `创建` 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fotzuwl2gej30ep0acwgb.jpg)

# 配置计算引擎

创建完成后可以看到分配给实例的 **`外部IP`** ，请牢记。

点击云引擎后面的 `SSH` ，远程连接该主机，进行需要的配置。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fou05u93clj30cw04yweq.jpg)

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

至此，计算引擎配置完毕。

# 参考资料

[Google Cloud服务免费申请试用以及使用教程](https://51.ruyo.net/2144.html)

感谢你阅读文章！


