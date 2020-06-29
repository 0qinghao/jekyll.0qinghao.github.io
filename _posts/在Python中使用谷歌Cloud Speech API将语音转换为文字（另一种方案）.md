在之前发布的[使用谷歌Cloud Speech API将语音转换为文字](https://segmentfault.com/a/1190000013591768)一文中，我们实现了在控制台使用curl发送post请求，得到语音转文字的结果；而[在Python中使用谷歌Cloud Speech API将语音转换为文字](https://segmentfault.com/a/1190000013600141)一文中，我们实现了安装Cloud Speech API客户端库，通过调用库函数得到语音转文字的结果。

如果你尝试过这两种方法，就会发现其实后者得到结果需要的时间要长一些（笔者使用这两种方法得到结果的耗时分别大约是5秒、7秒）。那么，有没有办法在python中像第一种方法那样，使用curl命令发送post请求呢。当然是可行的，所以今天我们将介绍在Python中使用Cloud Speech API将语音转换为文字的另一种方案，另外这次我们将把音频文件编码为base64嵌入到json请求文件中，省去了上传声音文件到Cloud Storage的步骤。

------

相关说明之类的在上面两篇文章里已经写了很多，这边就直接贴代码。
***使用python3**

```python
import json
import urllib.request
import base64

# ①
api_url = "https://speech.googleapis.com/v1beta1/speech:syncrecognize?key=你的API密钥"
audio_file = open('/home/pi/chat/test-speech/output.wav', 'rb')
audio_b64 = base64.b64encode(audio_file.read())
audio_b64str = audio_b64.decode()	# ②
# print(type(audio_b64))
# print(type(audio_b64str))
audio_file.close()

# ③
voice = {
  "config":
  {
    #"encoding": "WAV",
    "languageCode": "cmn-Hans-CN"
  },

  "audio":
  {
    "content": audio_b64str
  }
}
# 将字典格式的voice编码为utf8
voice = json.dumps(voice).encode('utf8')

req = urllib.request.Request(api_url, data=voice, headers={'content-type': 'application/json'})
response = urllib.request.urlopen(req)
response_str = response.read().decode('utf8')
# ④
# print(response_str)
response_dic = json.loads(response_str)
transcript = response_dic['results'][0]['alternatives'][0]['transcript']
confidence = response_dic['results'][0]['alternatives'][0]['confidence']
print(transcript)
print(confidence)
```

几点说明：

注释`①`：请求API的链接，请替换`你的API密钥` 。如果你有疑问，或许可以参考 [创建API密钥 | 使用谷歌Cloud Speech API将语音转换为文字](https://segmentfault.com/a/1190000013591768#articleHeader2) 。
  `audio_file` 路径替换为你的本地声音文件路径。


注释`②`：这次上传音频的方式是，将声音文件编码为base64，把对应的整个字符串放进json请求中。如果你执行 `print(type(audio_b64))` 就会发现编码后的audio_b64是 `bytes` 类型，所以还需要做一次decode()，转成字符串。


注释`③`：先以字典格式保存json请求内容，代表声音文件的字符串就在这里放入。


注释`④`：API返回的结果保存在 `response_str` ，如果你直接运行 `print(response_str)` 就会发现这个字符串可以看做一个有很多“层”的字典，要提取出识别结果，需要搞清楚这个字典到底是怎么组成的：

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fpijie8o5dj30fj09it8v.jpg)

第`1`层：花括号{}说明字符串 `response_str` 在执行 `json.loads` 后变成一个"字典"。得到"字典" `response_dic` 。

第`2`层：字典中只有一组键-值， `response_dic['results']` 取出唯一的键"results"对应的值。观察这个值，发现中括号[]，说明这个值的类型是”列表“。

第`3`层：观察列表 `response_dic['results']` ，发现列表中只有一项数据，但这项数据又是"字典"类型。将其取出，得到"字典" `response_dic['results'][0]` 。

第`4`层：字典中又是只有一组键-值， `response_dic['results'][0]['alternatives']` 取出唯一的键"alternatives"对应的值。观察这个值，[]说明我们得到的结果又是一个"列表"。

第`5`层：观察列表 `response_dic['results'][0]['alternatives']` ，列表中只有一项数据， `response_dic['results'][0]['alternatives'][0]` 再将这唯一一项数据取出，发现得到的是一个"字典"，而这次得到的字典中有两组键-值，分别取出就是我们要的结果和置信度了。
```python
transcript = response_dic['results'][0]['alternatives'][0]['transcript']
confidence = response_dic['results'][0]['alternatives'][0]['confidence']
```

------

**小结：**

今天介绍的这种方案，获取结果需要的时间比用API客户端库要快一些，另外应用了把本地语音编码后放入json请求的方式，也能方便后期和录音程序结合在一起使用。但稍有一点缺点是API密钥直接暴露在代码中，对实际应用可能会有一些影响。

下一步的目标是和录音功能结合起来，实现自动识别当前录制的语音。

感谢你阅读文章！