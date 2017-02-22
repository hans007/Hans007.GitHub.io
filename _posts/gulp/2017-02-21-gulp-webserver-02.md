---
layout: post
title: 前端自动化构建工具 - gulp - 02 - webserver环境
category: 前端自动化构建工具
tags: 
  - 前端
  - 构建工具
  - gulp
---

# 前言

配置一个可实时预览的环境

## 安装

```
npm install --save-dev gulp-live-server
```

## 配置

```javascript
var gulp = require('gulp');
var webserver = require('gulp-webserver');
 
gulp.task('webserver', function() {
  var server = gls.static('release/dist', 8080);
  server.start();
});
```

> release/dist 请指向你的发布目录

## 运行

- 输入

```
gulp webserver
```

- 输出

```javascript
[14:31:06] Using gulpfile ~/gulpfile.js
[14:31:06] Starting 'webserver'...
[14:31:06] Finished 'webserver' after 10 ms
livereload[tiny-lr] listening on 35729 ...
folder "release/dist" serving at http://localhost:8080
```

## 参考文

> https://www.npmjs.com/package/gulp-live-server

