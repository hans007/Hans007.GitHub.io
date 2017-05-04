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

## 把某个用户加入组

```
usermod -a -G www yuu
```

## 设置目录所有者

```
chown -R yuu:yuu yuumanage
chown -R www:www yuumanage
```

## 设置目录权限

```
chmod 755 -R ask
```

> d rwx rwx r-x . 所有者 组其他 其他用户
> -R 所有子目录








[我的博客](https://hans007.github.io)