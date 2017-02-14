---
layout: post
title: linux - centos7建立工作账号
category: linux
tags:
  - linux
  - centos7
---


# 工作账号

## 创建

```
useradd yuu
ls -l /home
```

## 设置密码

```
passwd yuu
vi /etc/passwd
```

## 设置目录所有者

```
chown -R yuu:yuu yuumanage
```

> -R 所有子目录

## 设置用户组








[我的博客](https://hans007.github.io)