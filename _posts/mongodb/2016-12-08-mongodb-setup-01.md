---
layout: post
title: MongoDB - 安装&运行&开机启动&删除&客户端
category: MongoDB
tags:
  - linux
  - MongoDB
---

# yum方式

## 配置.repo

- 创建 /etc/yum.repos.d/mongodb-org-3.4.repo

```
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

## 安装install

```
$ yum install -y mongodb-org
```

## 运行

```
$ systemctl start mongod.service
```

## 停止

```
$ systemctl stop mongod.service
```

## 设置开机启动

```
$ systemctl enable mongod.service
```

## 关闭开机启动

```
$ systemctl disable mongod.service
```

## 重启

```
$ systemctl restart mongod.service
```

## 删除包

```
yum erase $(rpm -qa | grep mongodb-org)
```

## 清理文件

```
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongo
```

## 检测后台进程是否存在

```
$ ps -ef |grep mongo
```

## 检测27017端口是否在监听

```
$ netstat -lntp | grep 27017
```

# 第三方GUI客户端

## Mongoclient

- 官网

> http://www.mongoclient.com/

- 下载

> https://github.com/rsercano/mongoclient

![User Management](https://camo.githubusercontent.com/4c0d09324a4265a0f11cbe9d22b160a05e4be7c2/687474703a2f2f6d6f6e676f636c69656e742e636f6d2f696d672f73732f756d2e706e67)

## rockmongo

> http://rockmongo.com/
> 需要PHP环境



[我的博客](https://hans007.github.io)