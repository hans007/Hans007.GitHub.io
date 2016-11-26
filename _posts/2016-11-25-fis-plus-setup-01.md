---
layout: post
title: 前端自动化构建工具 - FIS-PLUS - 第一节:安装并运行
category: 前端自动化构建工具
tags: 
  - 前端
  - 构建工具
  - FIS-PLUS
---

# 前言

FIS-PLUS 是基于 FIS，应用于后端是 PHP，模板是 Smarty 的场景。现在被大多数百度产品使用。

> 手册
> http://oak.baidu.com/fis-plus/document.html

# 安装

安装 nodejs

```
http://nodejs.org/
```

安装 fis-plus

```
$ sudo npm install -g fis-plus
```

安装 lights

```
$ sudo npm install -g lights
```

安装 Java

```
http://java.com/
```

安装 php-cgi

```
$ brew install php55 --with-cgi
```

如果提示找不到 `php55` ，执行

```
$ brew tap homebrew/homebrew-php
```

> 安装 JAVA 和 php-cgi 是由于 fis-plus 内置了 jetty 服务框架来解析 php 脚本
>
> 如果npm很慢，可以修改镜像
> 国内镜像 --registry=http://r.cnpmjs.org
> 百度内部可以使用公司内镜像 --registry=http://npm.internal.baidu.com

# 测试运行

```
$ fisp server start
```

> 如果发现错误 checking php-cgi support : unsupported php-cgi environment
> 安装 XAMPP
> https://www.apachefriends.org

安装成功后

```
使用zsh
$ echo 'export PATH=/Applications/XAMPP/bin:$PATH' >> ~/.zshrc
$ source ~/.zshrc

使用bash
$ echo 'export PATH=/Applications/XAMPP/bin:$PATH' >> ~/.zshrc
$ source ~/.zshrc
```

# 实战

## 下载代码

https://github.com/fex-team/fis-plus-pc-demo

## 初始fisp server

```
$ fisp server init
```

## 发布

```
$ fisp release -r common
$ fisp release -r home
```

## 运行

```
$ fisp server start #启动服务器
```

# 代码

> https://github.com/fex-team/fis-plus-pc-demo

[我的博客](https://hans007.github.io)