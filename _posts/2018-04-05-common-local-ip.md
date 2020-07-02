---
layout: post
title: 常见内网网段
categories: [network]
description: 常见的局域网地址，方便平时查询
keywords: 内网网段, 局域网网段, IP
furigana: true
---

> [内网IP段有哪些](http://blog.kankanan.com/article/51857f51-ip-6bb5670954ea4e9b.html)

常见的内网IP段有：

``` nohighlight
10.0.0.0/8
10.0.0.0 - 10.255.255.255

172.16.0.0/12
172.16.0.0 - 172.31.255.255

192.168.0.0/16
192.168.0.0 - 192.168.255.255
```

以上三个网段分别属于A、B、C三类IP地址，来自 《RFC 1918》。

但是根据 《Reserved IP addresses - Wikipedia, the free encyclopedia》 及《RFC 6890 - Special-Purpose IP Address Registries》的描述， 还有很多其它的内网IP段（包括IPv6），以及其它用途的保留IP地址。

其它IPv4内网段罗列如下：

``` 
0.0.0.0/8
0.0.0.0 - 0.255.255.255
用于当前网络内的广播消息。
```

``` 
100.64.0.0/10
100.64.0.0 - 100.127.255.255
由运营商使用的私网IP段，随着IPv4地址池的耗光，会有更多用户被分配到这个网段。
```

``` 
127.0.0.0/8
127.0.0.0 - 127.255.255.255
本机回环地址。
```

``` 
169.254.0.0/16
169.254.0.0 - 169.254.255.255
获取不到IP地址时使用，通常因为从DHCP服务器获取不到IP。
```

``` 
255.255.255.255/32
255.255.255.255
本网段的广播地址。
```