---
layout: post
title: 修改pip源
categories: [python, linux]
description: win和linux下修改pip软件源
keywords: python, windows, linux, pip
furigana: false
---
linux下，修改 ~/.pip/pip.conf (没有就创建一个)，内容如下：

``` nohighlight
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
 ```

 
windows下，直接在user目录中创建一个pip目录，其中新建文件pip.ini，如：C:/Users/xx/pip/pip.ini，内容如下

``` nohighlight
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```