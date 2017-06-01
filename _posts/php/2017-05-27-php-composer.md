---
layout: post
title: php - Composer PHP依赖管理
category: php
tags:
  - php
  - Composer
---

# Composer

## 参考

- [getcomposer.org](https://getcomposer.org/)
- [中国镜像](https://pkg.phpcomposer.com/)

## 安装

- 下载包

```
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"

php composer-setup.php

php -r "unlink('composer-setup.php');"
```

- 全局安装

```
// macos
// composer.phar 文件移动到 /usr/local/bin/
sudo mv composer.phar /usr/local/bin/composer

// windows
// composer.phar 复制到 php.exe 在同一级目录
// 在 PHP 安装目录下新建一个 composer.bat 文件
@php "%~dp0composer.phar" %*
```

## 修改镜像

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
