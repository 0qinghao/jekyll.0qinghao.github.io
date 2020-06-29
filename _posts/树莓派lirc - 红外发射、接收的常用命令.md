测试IR输入
```nohighlight
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