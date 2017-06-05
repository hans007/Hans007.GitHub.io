---
layout: post
title: php - lnmp1.4 安装配置 laravel 5.4
category: php
tags:
  - lnmp
  - laravel
---

## 本机环境

- lnmp 1.4
- centos 6.9
- php 5.6

## 项目位置

/home/wwwroot/default/blog

## 安装 lnmp集成环境

- lnmp.org
    https://lnmp.org/install.html

```
wget -c http://soft.vpser.net/lnmp/lnmp1.4.tar.gz && tar zxf lnmp1.4.tar.gz && cd lnmp1.4 && ./install.sh lnmp
```

## 安装 laravel项目

- 指南
    https://laravel.com/
    https://docs.golaravel.com/docs/5.4/installation/

## 安装 composer

- [安装composer](https://hans007.github.io/php/2017/05/27/php-composer)

## 修改nginx配置

```
cd /usr/local/nginx/conf/vhost/
vi host...

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
```

## 调试方便打开php.ini配置
**生产环境请关闭**

```
vi /usr/local/php/etc/php.ini
display_errors = On
```

## 添加新站点

- 客户机添加host测试网址配置

```
vi /private/etc/hosts
```

- centos添加新站点

```
lnmp vhost add
```

## 修改跨域目录

项目目录下的 `.user.ini` 保持不变，我们做加法。

```
vi /usr/local/nginx/conf/fastcgi.conf

生产环境，加入你的项目地址
fastcgi_param PHP_ADMIN_VALUE "open_basedir=$document_root/:/tmp/:/proc/:/home/wwwroot/default/blog";

如果本机测试开发，直接注释本行
# fastcgi_param PHP_ADMIN_VALUE ...
```

## 重启环境

```
lnmp reload
```
