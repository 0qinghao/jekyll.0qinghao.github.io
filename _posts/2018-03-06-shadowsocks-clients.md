---
layout: post
title: Shadowsocks客户端在不同系统下的使用方法
categories: [shadowsocks, linux]
description: 记录ss客户端使用的水文，后期换了更强大的clash
keywords: shadowsocks
furigana: true
---

当我们配置好Shadowsocks服务器端，或是购买了SS账号后，就可以使用客户端开始科学上网了。下面分别介绍在Windows、安卓、Linux(Debian)系统下SS客户端的使用方法。

# Windows系统下SS客户端的使用方法

Windows系统下的SS客户端使用起来最为方便。客户端自带了系统全局代理的功能，甚至可以省去配置浏览器插件的操作。

## 下载客户端

* [点击这里，跳转到GitHub下载](https://github.com/shadowsocks/shadowsocks-windows/releases)
* [如果你无法打开GitHub，可以点击这里，前往微云下载，但不保证是最新版本](https://share.weiyun.com/edaa2c5f08aa5169c2be5c6a9f59662d)

将压缩包内的可执行程序解压，放置在合适的文件夹内，运行后会在程序同一目录下产生配置文件，如果随便放置容易显得杂乱。

## 配置客户端

第一次打开SS客户端会主动要求编辑服务器。填入你的 **`服务器地址`** **`密码`** **`服务器端口号`** **`加密方式`** ，其他设置可以保持默认。

![图 3](/assets/images/6801a5a7d3a234f4155dc191472d80cc6edcaeab6d3d360a7833610cea1397e5.jpg)

  

最后，右击任务栏的小图标，勾选 **`启用系统代理`** 。系统代理模式选择 **`PAC模式`** ，这样SS会自动使用代理访问墙外站点，不需要另外安装浏览器的代理插件。

![](http://ww1.sinaimg.cn/mw690/005MY9Xigy1fou6nucjagj3059065t8t.jpg)

# 安卓系统下SS客户端的使用方法

安卓系统下的SS客户端也很完善，配置方便，甚至还可以指定仅部分APP使用代理。

## 下载客户端

* 如果你能使用Google Play商店，直接搜索安装[Shadowsocks](https://play.google.com/store/apps/details?id=com.github.shadowsocks)
* [你也可以点击这里，前往微云下载，但不保证是最新版本](https://share.weiyun.com/f9250253dab9ec9ba9b12e124733adcd)

## 配置客户端

点击右上角的 `+` 选择 `手动设置` ，填入你的 **`服务器地址`** **`密码`** **`服务器端口号`** **`加密方式`** ，其他设置可以保持默认。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fp2brcto8aj307y0e3wf1.jpg)

还可以在配置中开启 `分应用VPN` 功能，来指定仅部分APP的流量进行代理；或者再打开 `绕行模式` 来指定部分APP的流量绕过代理。

# Debian下SS客户端的使用方法

Linux下使用SS客户端要麻烦一些，一方面Linux下SS不带全局代理，需要搭配浏览器插件使用；另一方面笔者在使用中有遇到bug，不知在你阅读这篇文章时是否已经修复，总之还是会记录在下文中以供参考。

另外，这部分介绍的是配合Chrome插件实现浏览器翻墙的方法。关于如何在LX终端让 `wget`  `curl` 等命令使用代理，将在另一篇文章中再做总结。

## 安装客户端

``` shell
sudo apt update
sudo apt install shadowsocks
```

## 运行sslocal

不带任何参数运行 `sslocal` 可以查看帮助。

运行SS客户端一般有两种方法。你可以参考帮助，将必要的参数填入，用一条较长的指令来运行：

``` shell
sudo sslocal -s 服务器地址 -p 服务器端口 -k 密码 -m 加密方式 -d start
```

显然上面这种方式效率太低。另一种方式就是将各项参数保存为json文件，运行时指定配置文件即可。

假设我们的配置文件是 `/etc/ss.json` ，其内容为：

``` nohighlight
{
  "server":"服务器地址",
  "server_port":服务器端口,
  "local_address":"127.0.0.1",
  "local_port":1080,
  "password":"密码",
  "timeout":600,
  "method":"加密方式",
  "fast_open":false
}
```

将你的 **`服务器地址`** **`密码`** **`服务器端口号`** **`加密方式`** 替换到上述文件。（有双引号的请保留双引号，不要删除）

接下来，每次需要运行SS客户端时，我们只需要输入一条简短的指令：

``` shell
sudo sslocal -c /etc/ss.json -d start
```

### *运行sslocal时遇到的bug

> 解决方案来自[Kali2.0 update到最新版本后安装shadowsocks服务报错问题](http://blog.csdn.net/blackfrog_unique/article/details/60320737)

笔者在运行sslocal命令时遇到了形如 *INFO loading libcrypto from libcrypto.so.1.1* 的报错。后在上述文章中找到解决方案。

打开文件openssl.py，请参照错误提示确定是否与下述文件路径相同：

``` nohighlight
sudo nano /usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py
```

使用快捷键<kbd>CTRL</kbd>+<kbd>_</kbd>选择跳转到第52行

将 *libcrypto. EVP_CIPHER_CTX_ `cleanup` .argtypes = (c_void_p, )* 修改为 *libcrypto. EVP_CIPHER_CTX_ `reset` .argtypes = (c_void_p, )* 

同样地，跳转到第111行

将 *libcrypto. EVP_CIPHER_CTX_ `cleanup` (self.ctx)* 修改为 *libcrypto. EVP_CIPHER_CTX_ `reset` (self._ctx)*

按<kbd>CTRL</kbd>+<kbd>X</kbd>，<kbd>Y</kbd>保存退出。重新执行sslocal指令运行正常。

## 安装Chrome插件

由于Linux下的SS客户端不带全局代理功能，需要配合浏览器插件使用。这里只介绍Chrome插件的安装方法，火狐大体上类似。

如果你能够使用Chrome应用商店，搜索[SwitchyOmega](https://chrome.google.com/webstore/search/switchyomega?hl=zh-CN)安装即可。你也可以点击[这里](https://share.weiyun.com/28d769f6e52b68b894736b54b29cf9e4)通过微云下载crx文件，将其拖动到[Chrome扩展程序页面](chrome://extensions/)完成安装。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fp2f11yv4kj30d207y0t4.jpg)

点击选项，如下图配置SwitchyOmega。 `代理协议` 选择 `SOCKS5` ；如果你在ss.json配置文件中修改过 `local_port` 参数，则这里 `代理端口` 必须与其一致，否则保持默认值1080即可。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fp2f9hgejzj30gl06ct9h.jpg)

最后，保存配置，点击SwitchyOmega图标切换到刚才配置好的情景模式。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fp2f89rsqgj30hw09u404.jpg)

# 结语

由于手上设备有限，没办法总结所有系统下的SS客户端使用方法。例如iOS系统下似乎是使用[Big Boss源](http://apt.thebigboss.org/onepackage.php?bundleid=com.linusyang.shadowsocks)搜索ShadowSocks应用，但没法亲自尝试。有兴趣的朋友建议前往官网https://shadowsocks.org/（.com那个是出售SS服务的）进一步了解。

感谢你阅读文章！
