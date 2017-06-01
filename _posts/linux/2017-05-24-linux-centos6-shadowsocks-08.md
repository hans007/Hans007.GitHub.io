---
layout: post
title: linux - centos6 Shadowsocks
category: linux
tags:
  - linux
  - centos6
---

# Shadowsocks

## setup

```
yum install -y python-setuptools #安装setuptools 
easy_install pip #安装pip
pip install shadowsocks #安装package
```

## config

```
vi /etc/shadowsocks.json

{
 "server":"your server ip",
 "local_address": "127.0.0.1",
 "local_port":1080,
  "port_password": {
     "8080": "password"
 },
 "timeout":300,
 "method":"aes-256-cfb",
 "fast_open": false
}
```

## run

ssserver -c /etc/shadowsocks.json -d start #启动 
ssserver -c /etc/shadowsocks.json -d stop #停止

## boot

```
vi /etc/rc.d/rc.local
ssserver -c /etc/shadowsocks.json -d start
```

## nscd hosts缓存

```
yum install -y nscd
/etc/rc.d/init.d/nscd start
/etc/rc.d/init.d/nscd restart

vi /etc/nscd.conf
enable-cache hosts yes
```

## dns

```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
dns1=8.8.8.8
dns2=8.8.4.4

vi /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4
```

