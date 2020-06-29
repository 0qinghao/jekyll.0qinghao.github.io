如果你想在一个virtualenv中安装PyAudio，请安装APT中的PortAudio开发头文件，然后安装PyAudio：
```nohighlight
sudo apt-get install portaudio19-dev
pip install --allow-unverified=pyaudio pyaudio
```