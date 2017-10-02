---
layout: post
title: macos - 生成gitbook电子书
category: macos
tags:
  - macos
---

# 前言

## 安装

```
npm install -g gitbook-cli  
```

> 需要nodejs环境

## 命令 - 本地编译

```
gitbook build
```

## 命令 - 本地WEB服务方式

```
gitbook serve
```

## 命令 - 初始化

```
gitbook init
```

> 这将会生成一个标准项目

## `SUMMARY.md` 文件

```
# Summary

* [Introduction](README.md)
```

> 这里存放了目录与文件的对应关系

## `book.json` 配置文件

```json
{
    "title" : "课程产品研发管理系统 - API接口说明",
    "author" : "",
    "description" : "规范API设计",
    "language" : "zh-hans",
    "links" : {
        "gitbook" : false,
        "sidebar" : {
            "IT黑马" : "http://www.itheima.com/"
            }
    },
    "plugins": [
        "-sharing"
    ]
}
```

## 其他命令

```
gitbook build: 会生成相应的 HTML 文件供分发。
gitbook pdf: 生成 PDF 文件
gitbook epub: 生成 epub 文件
gitbook mobi: 生成 mobi 文件
```