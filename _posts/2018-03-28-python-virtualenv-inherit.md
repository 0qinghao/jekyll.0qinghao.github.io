---
layout: post
title: python - 创建virtualenv时选择继承系统站点程序包
categories: [python]
description: python创建虚拟环境时继承系统程序包
keywords: python, virtualenv, 继承
furigana: false
---
笔记

``` nohighlight
virtualenv --system-site-packages --python python3 env
```

可以解决smbus模块不存在的问题。
