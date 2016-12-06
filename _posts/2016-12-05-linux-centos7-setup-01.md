---
layout: post
title: linux - centos7安装&配置
category: linux
tags:
  - linux
  - centos7
---

# 安装

- 下载

> https://www.centos.org/download/

推荐下载 mini 版本, 我们都是在命令行下操作，不需要图像界面。

# 启用网卡

```
$ cd /etc/sysconfig/network-scripts/
$ cp ifcfg-eth0 ifcfg-eth0-backup
$ vi ifcfg-eth0

BOOTPROTO=dhcp # 修改成 dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=eth0
UUID=2e207ca0-b72e-4032-a903-45cfb5d5dbb3
DEVICE=eth0
ONBOOT=yes # 改成 yes
PEERDNS=yes
PEERROUTES=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
```

> 修改配置前 请备份
> 把 dhcp 和 onroot 开启

# 修改DNS解析

- 显示当前网络连接

```
$ nmcli connection show
```

- 修改当前网络连接对应的DNS服务器，这里的网络连接可以用名称或者UUID来标识

```
$ nmcli con mod eth0 ipv4.dns "114.215.126.16 42.236.82.22"
```

- 将dns配置生效
```
$ nmcli con up eth0
```

> 国内干净的DNS可以用 http://www.onedns.net/index.php?nfssp=setup-mac

# 关闭 SELINUX

- 修改配置

```
$ vi /etc/selinux/config
```

```
#SELINUX=enforcing #注释掉
#SELINUXTYPE=targeted #注释掉
SELINUX=disabled #增加
:wq! #保存退出
```

- 使配置立即生效

```
$ setenforce 0
```

# 关闭firewall

```
$ systemctl stop firewalld.service #停止firewall
$ systemctl disable firewalld.service #禁止firewall开机启动
```

# 软件包

- net-tools

```
$ yum install net-tools
```



[我的博客](https://hans007.github.io)