---
layout: post
title: web框架 - semantic-ui 修改默认字体 02
category: web
tags:
  - semantic-ui
---

# 前言

默认安装完 semantic-ui 你会发现写个demo页 很慢，调试发现。

```
Request URL:https://fonts.googleapis.com/css?family=Lato:400,700,400italic,700italic&subset=latin
Request Method:GET
Status Code:200  (from memory cache)
Remote Address:203.208.43.78:443
```

这是去 google 下载了，然后被墙...

啥也不说 按如下修改

## 动手修改

semantic-ui是模板机制的，打开目录

> /semantic/src/themes

打开所需的模板文件 

> /semantic/src/themes/default/globals/site.variables

## 修改默认字体

- 原来

```
@fontName          : 'Lato';
```

- 修改为

```
@fontName          : 'Microsoft YaHei';
```

## 屏蔽从谷歌下载

- 原来

```
@importGoogleFonts : true;
```

- 修改为

```
@importGoogleFonts : false;
```

## 重新编译

> 源码方式使用，请参考我前一篇文

```
cd semantic/
gulp build
```



[我的博客](https://hans007.github.io)