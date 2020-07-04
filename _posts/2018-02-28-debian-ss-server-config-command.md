---
layout: post
title: SS Debian服务器端配置指令
categories: [shadowsocks, linux]
description: Debian下配置SS服务器的命令笔记
keywords: shadowsocks, debian, linux, ss服务器
furigana: false
---

笔记

``` nohighlight
wget --no-check-certificate -O shadowsocks-libev-debian.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev-debian.sh

sudo chmod +x shadowsocks-libev-debian.sh

sudo ./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log
```
