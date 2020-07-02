---
layout: post
title: 在virtualenv中安装PyAudio
categories: [python]
description: python录音
keywords: python, virtualenv, pyaudio, 录音
furigana: false
---
如果你想在一个virtualenv中安装PyAudio，请安装APT中的PortAudio开发头文件，然后安装PyAudio：

``` nohighlight
sudo apt-get install portaudio19-dev
pip install --allow-unverified=pyaudio pyaudio
```
