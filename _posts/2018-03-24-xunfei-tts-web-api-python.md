---
layout: post
title: 在Python中使用科大讯飞Web API进行语音合成
categories: [python, TTS]
description: 科大讯飞的TTS web API使用
keywords: 讯飞, TTS, python
furigana: false
---
前几日讯飞开放平台推出了WebAPI接口，恰好最近需要实现一个文字转语音的功能，于是就尝试着用了起来。但不知什么原因，官方文档的[调用示例](http://doc.xfyun.cn/rest_api/%E8%AF%AD%E9%9F%B3%E5%90%88%E6%88%90.html#%E8%B0%83%E7%94%A8%E7%A4%BA%E4%BE%8B)一直报错，最后自己照着示例的思路用python3重写了一遍。所以这次总结一下在Python中使用讯飞Web API进行语音合成的过程。

# 注册讯飞开放平台

首先注册讯飞开放平台：[http://passport.xfyun.cn/register](http://passport.xfyun.cn/register)

注册完成后进入控制台，在控制台[创建一个新应用](http://console.xfyun.cn/app/create?source=WebAPI) ，填写一些基本信息，注意 `应用平台` 选择 `WebAPI` 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fpn63fdqkej30du0am3z2.jpg)

创建完成后，记录下 `APPID` 和 `APIKey` ，将在程序中用到。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fpn67r2lmdj30ct0bg766.jpg)

另外，请在 `IP白名单` 中添加自己的外网IP，可以在http://www.ip138.com/ 查看。（一般来说外网IP会常常发生变化，请注意）

# 在Python3中使用讯飞Web API

先上代码，后面进行必要的说明：

可能提示缺库：pip3 install requests

***使用python3执行**

``` python
import base64
import json
import time
import hashlib
import requests

# API请求地址、API KEY、APP ID等参数，提前填好备用
api_url = "http://api.xfyun.cn/v1/service/v1/tts"
API_KEY = "替换成你的APIKEY"
APP_ID = "替换成你的APPID"
OUTPUT_FILE = "C://output.mp3"    # 输出音频的保存路径，请根据自己的情况替换
TEXT = "苟利国家生死以，岂因祸福避趋之"

# 构造输出音频配置参数
Param = {
    "auf": "audio/L16;rate=16000",    #音频采样率
    "aue": "lame",    #音频编码，raw(生成wav)或lame(生成mp3)
    "voice_name": "xiaoyan",
    "speed": "50",    #语速[0,100]
    "volume": "77",    #音量[0,100]
    "pitch": "50",    #音高[0,100]
    "engine_type": "aisound"    #引擎类型。aisound（普通效果），intp65（中文），intp65_en（英文）
}
# 配置参数编码为base64字符串，过程：字典→明文字符串→utf8编码→base64(bytes)→base64字符串
Param_str = json.dumps(Param)    #得到明文字符串
Param_utf8 = Param_str.encode('utf8')    #得到utf8编码(bytes类型)
Param_b64 = base64.b64encode(Param_utf8)    #得到base64编码(bytes类型)
Param_b64str = Param_b64.decode('utf8')    #得到base64字符串

# 构造HTTP请求的头部
time_now = str(int(time.time()))
checksum = (API_KEY + time_now + Param_b64str).encode('utf8')
checksum_md5 = hashlib.md5(checksum).hexdigest()
header = {
    "X-Appid": APP_ID,
    "X-CurTime": time_now,
    "X-Param": Param_b64str,
    "X-CheckSum": checksum_md5
}

# 发送HTTP POST请求
def getBody(text):
    data = {'text':text}
    return data
response = requests.post(api_url, data=getBody(TEXT), headers=header)

# 读取结果
response_head = response.headers['Content-Type']
if(response_head == "audio/mpeg"):
    out_file = open(OUTPUT_FILE, 'wb')
    data = response.content # a 'bytes' object
    out_file.write(data)
    out_file.close()
    print('输出文件: ' + OUTPUT_FILE)
else:
    print(response.read().decode('utf8'))
```

下面按照代码顺序进行各部分的说明。

## APIKey等参数

在代码开头填好各项参数，方面代码中使用。

API_KEY和APP_ID请替换为上一步创建应用后得到的内容。请**不要删除双引号**。

OUTPUT_FILE是最终输出音频的保存路径，根据自己的情况替换。

TEXT是将要输出为语音的文本。

## 音频配置参数

`Param` 是字典格式的音频配置参数，其中 `"aue"` 可选 `raw` (生成wav)或 `lame` (生成mp3)，如果修改成raw请记得同时修改输出文件的扩展名。

最后需要将配置参数编码为Base64字符串：字典类型→明文字符串→utf8编码→Base64(bytes)→Base64字符串，具体实现可以参考代码。

音频配置参数的详细说明可以参考[请求参数 - 语音合成](http://doc.xfyun.cn/rest_api/%E8%AF%AD%E9%9F%B3%E5%90%88%E6%88%90.html#%E8%AF%B7%E6%B1%82%E5%8F%82%E6%95%B0) 。

## HTTP请求头部

根据 [授权认证 - 科大讯飞RESET_API开发指南](http://doc.xfyun.cn/rest_api/%E6%8E%A5%E5%8F%A3%E6%A6%82%E8%BF%B0.html#%E6%8E%88%E6%9D%83%E8%AE%A4%E8%AF%81) ，在调用所有业务接口时，都需要在HTTP请求头部中配置以下参数用于授权认证：

| 参数       |  格式  | 说明                                                         |
| :--------- | :----: | ------------------------------------------------------------ |
| X-Appid    | string | 讯飞开放平台注册申请应用的应用ID(appid)                      |
| X-CurTime  | string | 当前UTC时间戳，从1970年1月1日0点0 分0 秒开始到现在的秒数     |
| X-Param    | string | 音频配置参数JSON串经Base64编码后的字符串                     |
| X-CheckSum | string | 令牌，计算方法：MD5(apiKey + curTime + param)。三个值拼接的字符串，进行MD5哈希计算（32位小写）。 |

具体实现参考代码中字典 `header` 。

## 发送请求&读取结果

最后使用requests库发送HTTP POST请求，得到结果。根据响应的 `header` 可以判断是否合成成功。

若响应头部包含 `Content-type: audio/mpeg` ，则响应Body为音频数据，可写入文件保存。

若合成出现错误，响应头部包含 `Content-type: text/plain` ，响应Body为记载了错误类型的json字符串。

返回值的具体说明请参考 [返回值 - 语音合成](http://doc.xfyun.cn/rest_api/%E8%AF%AD%E9%9F%B3%E5%90%88%E6%88%90.html#%E8%BF%94%E5%9B%9E%E5%80%BC) 。

## 运行结果

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fpn7johr0kj30dc03dt8p.jpg)

使用几次后，感觉合成语音的断句做得不是很优秀，但响应速度很快，还是比较满意的。

[output.mp3](https://share.weiyun.com/528R8xM)

# 小结

最近使用了几种Web API，对这类API的使用方法也算是有些经验了。最后，现在语音识别、图灵机器人、语音合成都试着做了一遍，下一篇博客将把他们组合起来，实现一个简单的语音助手。

感谢你阅读文章！
