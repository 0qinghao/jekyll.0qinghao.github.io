```nohighlight
wget --no-check-certificate -O shadowsocks-libev-debian.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev-debian.sh

sudo chmod +x shadowsocks-libev-debian.sh

sudo ./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log
```