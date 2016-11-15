---
layout: post
title: JavaScript的构建工具 - Grunt
category: JavaScript的构建工具
tags: 
  - javascript
  - Grunt
---

# 0. 前言 - Grunt 到底是啥?

![grunt](http://oflimcy5e.bkt.clouddn.com/grunt%E6%A0%87%E5%BF%97.png)

工作中我们会遇到，对代码(js,css,html)就行加工：压缩、合并、代码检查、测试 等等。

我们需要一个集成的环境 就行操作，Grunt就是这样的工具。

是一个js开发框架，具体功能是以安装插件的形式实现的，所以扩展性无限。

## 1. 安装

### 1.1 安装 node.js

Grunt是基于Node.js为基础的，所以没装node.js的需要安装下。

> http://nodejs.org/

已经安装的可以更新下

```
npm update -g npm
```

> macos 下加 sudo

### 1.2 安装 Grunt CLI

```
npm install -g grunt-cli
```

## 2. 配置我们第一个项目

### 2.1 建立项目目录

```
/grunt-do
   src/
   css/
   dist/
```

> src 开发的js代码
> css 开发的css代码
> dist 发布代码

### 2.2 根目录下创建 `package.json`

```
{
  "name": "myapp",
  "version": "0.1.1",
  "devDependencies": {
  }
}
```

- 安装Grunt

```
npm install grunt --save-dev
```

> package.json `devDependencies` 被加入了依赖配置

```
{
  "name": "myapp",
  "version": "0.1.1",
  "devDependencies": {
    "grunt": "^1.0.1"
  }
}
```

> 多了个 `node_modules` 的目录，有空可以阅读下,暂时我们无视这个目录。

### 2.3 根目录下创建 `Gruntfile.js`

```
module.exports = function(grunt) {

    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json')
    });

};
```

> 这是Grunt的配置文件
> initConfig初始pkg属性，这都是套路，读取上面的package.json

## 3. 插件功能

### 3.1 压缩js代码

### 3.2 压缩css代码

### 3.3 合并代码

### 3.4 代码检查

### 3.5 代码测试

### 3.6 CMD模式代码发布

### 3.7 监控文件变化

### 3.8 清除中间文件

## 4. 配置git上传

## 5. 拿到一个现有项目

## 参考代码

>

[我的博客](https://hans007.github.io)