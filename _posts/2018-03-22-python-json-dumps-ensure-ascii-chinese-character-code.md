---
layout: post
title: python json.dumps中的ensure_ascii参数引起的中文编码问题
categories: [python]
description: python中一个坑的记录
keywords: python, ensure_ascii, dumps, 中文, 编码
furigana: false
---
json.dumps序列化时对中文默认使用的ascii编码，想输出真正的中文需要指定ensure_ascii=False。

``` python
result_str = json.dumps(result, ensure_ascii = False)
```
