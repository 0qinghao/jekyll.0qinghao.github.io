https://github.com/houtianze/bypy

**安装**
```
pip install bypy 

# 执行任意命令进行授权
bypy info
```

```
#上传文件夹内的内容到app/bypy/
bypy upload 文件夹
```

```
#定时执行上传
sudo nano /etc/crontab

*/1 * * * * Rin bypy upload 文件夹

sudo /etc/init.d/cron restart
```