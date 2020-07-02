---
layout: post
title: 树莓派lirc - 红外发射、接收的常用命令
categories: [raspberrypi, linux]
description: 树莓派红外常用命令
keywords: 树莓派, 红外, lirc, 命令
furigana: false
---
笔记

测试IR输入

``` nohighlight
sudo service lircd stop
mode2 -m -d /dev/lirc0
```

录制

``` 
irrecord -d /dev/lirc0
# 空调等复杂设备使用-f
irrecord  -f -d /dev/lirc0
# 录制完成之后
sudo cp xxxxxxx.conf /etc/lirc/lircd.conf.d/
```

红外发射

``` 
sudo service lircd restart

irsend SEND_ONCE 设备名 按键名
irsend --count=10 SEND_ONCE 设备名 按键名
```
