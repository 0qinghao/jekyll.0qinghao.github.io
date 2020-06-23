> 原文：[Translation APIでテキストをほんやくする](https://codelabs.developers.google.com/codelabs/cloud-translation-intro-ja/index.html#0)

# 概要

使用Cloud Translation，可以将任意的字符串翻译为API支持的语言。由于语言检测功能的存在，即使在源语言未知的情况下，也能使用该API。

**将要学习的东西**

* 创建Cloud Translation API请求，使用curl调用API
* 翻译文本的方法
* 高级版（Premium Edition）的使用方法
* 检测语言

ruby测试{测试|てすと}

**必要的准备**

* 创建Google Cloud Platform项目
* 浏览器（[Chrome](https://www.google.com/chrome/browser/desktop/)、[Firefox](https://www.mozilla.org/firefox/) 等）

# 设置和一些说明

**根据自己的情况进行设置**

还未拥有Google账号（Gmail / Google Apps）的情况下，[创建账号](https://accounts.google.com/SignUp)是必须的。登录Google Cloud Platform Console（[console.cloud.google.com](http://console.cloud.google.com/)），创建一个新项目。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1is4jamij307q04rq2v.jpg)

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1isb2igbj30dg05waa3.jpg)

请记住项目名称。任意一个Google Cloud项目都拥有唯一的名称（上述的名称已经被使用了，所以实际上无法使用）。

Google Cloud Platform的新用户将赠与[相当于$ 300的试用金](https://console.developers.google.com/billing/freetrial)。

*** 用于教学的账号**

教师将已有的项目进行设置，生成临时账号。若你得到了教师发放的临时账号，你将不必顾虑项目中产生的费用。但是，一旦该教学项目结束，所有的临时账号将被无效化。

当你从教师那收到了临时账号的用户名/密码，就能够登录Google Cloud Console（<https://console.cloud.google.com/>）。

登录后，你将看到如下界面。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jc2vxn7j30wt0h4taz.jpg)

# 启用Translation API

点击屏幕左上角的菜单图标。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jddf3k3j307n01dglk.jpg)

在下拉菜单中选择 [**API Manager**] 

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1je8vadpj308k09gq36.jpg)

点击 [**启用API**] 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jfksxryj30l503kgls.jpg)

然后，在搜索框中输入「translate」。点击 [**Google Cloud Translation API**]。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jge0vz8j30sj07rwey.jpg)

API已经启用的情况下，只会显示 [停用] 按钮。请不要停用API。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jj62ajkj30dx01xwed.jpg)

API还未启用的情况下，点击[**启用**]，启用 Cloud Translation API 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jkjrp5kj30ku04zglv.jpg)

等待数秒，API成功启用后，将显示如下。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1jlh7mqoj30dx01xwed.jpg)

# 激活Cloud Shell

Google Cloud Shell 是在云端运行的命令行环境。这台基于 Debian 的虚拟机能够加载任何您需要的开发工具（gcloud、bq、git等），并提供永久的5 GB主目录。这次教程将使用 Cloud Shell 创建对 Translation API 的请求。

点击标题栏右侧的 [激活 Google Cloud Shell] 按钮（**>_**），启动Cloud Shell。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1k54ijd6j318g01y3yw.jpg)

Cloud Shell 将在控制台底部的新窗口中打开，并显示命令行提示符。请等待提示符 `user@project:~$` 出现。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1k6p9s1uj318g0a8dg7.jpg)

# 生成API Key

 你将通过使用curl发送一个请求来调用 Translation API 。在发送请求时，你需要在 URL 中插入一个生成的 API 密钥。为了创建 API 密钥，让我们点击侧边栏的 [API Manager] 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1vzcg8gfj308f09edg0.jpg)

然后，在 [**凭据**] 选项卡中点击 [**创建凭据**] 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1w1xzz3vj30t80fdt9v.jpg)

在下拉菜单中选择  [**API 密钥**] 。

![](http://ww1.sinaimg.cn/large/005MY9Xigy1fq1w36by92j30ds08m74w.jpg)

最后，复制生成好的密钥。

将密钥复制到剪贴板后，使用下述命令将其保存到 Cloud Shell 的环境变量中。下述的 **YOUR_API_KEY** 请替换成剪贴板中的内容。

``` 
export API_KEY=YOUR_API_KEY
```

# 翻译文本

在此例中，将「My name is Steve」这个字符串翻译为西班牙语。使用下述的curl命令，将之前保存好的 API 密钥环境变量和将要翻译的文本一起，传递给 Translation API 。

``` 
TEXT="My%20name%20is%20Steve"
curl "https://translation.googleapis.com/language/translate/v2?target=es&key=${API_KEY}&q=${TEXT}"
```

你将得到形式如下的响应。

``` 
{
  "data": {
    "translations": [
      {
        "translatedText": "Mi nombre es Steve",
        "detectedSourceLanguage": "en"
      }
    ]
  }
}
```

响应中，你可以看到翻译出的文本和 API 检测到的源语言。

> **Premium 模式**
>
> Google Cloud Translation 在几乎所有翻译任务中都是用了 Standard Edition 模式。然而 Google 从最近开始，使用了更为强大的 [Neural machine Translation System](https://research.googleblog.com/2016/09/a-neural-network-for-machine.html) 来优化翻译服务。在这里，我们可以使用 Premium 模式。详情请参阅[此处的指南](https://cloud.google.com/translate/docs/premium)。

# 检测语言

除了文本翻译以外，Translation API 还能用来检测文本的语言。此例中，我们将检测两个字符串的语言。下面将使用 curl 命令，把之前保存的 API 密钥环境变量和待检测的文本一起传递给 Translation API 。

``` 
TEXT_ONE="Meu%20nome%20é%20Steven"
TEXT_TWO="日本のグーグルのオフィスは、東京の六本木ヒルズにあります"
curl "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}&q=${TEXT_ONE}&q=${TEXT_TWO}"
```

你将得到形式如下的响应。

``` 
{
  "data": {
    "detections": [
      [
        {
          "confidence": 0.84644311666488647,
          "isReliable": false,
          "language": "pt"
        }
      ],
      [
        {
          "confidence": 1,
          "isReliable": false,
          "language": "ja"
        }
      ]
    ]
  }
}
```

本例中返回的语言是 「pt」和「ja」。它们是 [ISO-639-1](https://en.wikipedia.org/wiki/ISO_639-1) 的标识符，指葡萄牙语和日本语。关于可能的返回值，在[Translation API 支持的语言一览](https://cloud.google.com/translate/docs/languages)中可以查询。

# 恭喜！

在此次向导中，我们学习了如何使用 Cloud Translation API 进行文本的翻译。

**学到的东西**

* 创建Cloud Translation API请求，使用curl调用API
* 翻译文本的方法
* 高级版（Premium Edition）的使用方法
* 检测语言

**下一步**

* 通过常用的编程语言，使用客户端库，学习 [Translation API 的示例应用程序](https://cloud.google.com/translate/docs/samples)。
* 尝试使用 [Vision API](https://cloud.google.com/vision/) 、[Speech API](https://cloud.google.com/speech/) 。
