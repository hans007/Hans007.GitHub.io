---
layout: post
title: web框架 - semantic-ui 安装 01
category: web
tags:
  - semantic-ui
---

# 官网

http://www.semantic-ui.cn/

# 方式一：下载最终源码直接使用

## 下载地址

https://github.com/Semantic-Org/Semantic-UI

## 使用

/dist/ 目录就是最终发行代码文件
/dist/semantic.min.js
/dist/semantic.min.css

```
<script src=".public/jquery/jquery.min.js"></script>
<link rel="stylesheet" type="text/css" href="./public/semantic/semantic.css">
<script src="./public/semantic/semantic.js"></script>
```

> 请先包含jquery.min.js文件

# 方式二：源码编译方式

## 第一步: 安装NodeJS环境

- Homebrew 方式

```
brew install node
```

- Git 方式

```
git clone git://github.com/ry/node.git
cd node
./configure
make
sudo make install
```

## 第二步: 安装Gulp

```
npm install -g gulp
```

## 第三步: npm install 安装并编译

```
npm install semantic-ui --save
cd semantic/
gulp build
```

## 使用

```
<link rel="stylesheet" type="text/css" href="semantic/dist/semantic.min.css">
<script src="semantic/dist/semantic.min.js"></script>
```



[我的博客](https://hans007.github.io)