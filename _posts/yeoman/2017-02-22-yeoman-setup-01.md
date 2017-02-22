---
layout: post
title: 编码工具 - 脚手架yeoman - 安装&使用模板
category: yeoman
tags:
  - 编码工具
---

# 前言

http://yeoman.io/

yeoman是一个项目脚手架的框架，基于node.js平台，我感觉这是一个可以生成任何项目框架环境的工具。

非常cool~

# 安装

- 全局安装

```
npm install -g yo
```

# 使用webapp模板

## 模板页介绍

> https://github.com/yeoman/generator-webapp

## 安装模板到本机

- 全局方式

```
npm install -g generator-webapp
```

- 官方提示安装方式

> npm install --global yo gulp-cli bower generator-webapp
> 如果本机已经有 yo gulp-cli bower 就不要重复安装了，时间比较长
> 其实就是一个 npm 包，规则是包名前缀 generator-

## 使用模板

```
mkdir myYeomanWebApp
cd myYeomanWebApp
yo webapp
```

> 通过交互式的问答选项创建项目环境并下载依赖包

# 目录分析

```
├── app
│   ├── fonts
│   ├── images
│   ├── scripts
│   │   └── main.js
│   ├── styles
│   │   └── main.scss
│   ├── apple-touch-icon.png
│   ├── favicon.ico
│   ├── index.html
│   └── robots.txt
├── bower_components
│   ├── bootstrap-sass
│   │   ├── assets
│   │   ├── CHANGELOG.md
│   │   ├── CONTRIBUTING.md
│   │   ├── LICENSE
│   │   ├── README.md
│   │   ├── bower.json
│   │   ├── composer.json
│   │   ├── eyeglass-exports.js
│   │   ├── package.json
│   │   └── sache.json
│   ├── chai
│   │   ├── CODE_OF_CONDUCT.md
│   │   ├── CONTRIBUTING.md
│   │   ├── History.md
│   │   ├── README.md
│   │   ├── ReleaseNotes.md
│   │   ├── bower.json
│   │   ├── chai.js
│   │   ├── karma.conf.js
│   │   ├── karma.sauce.js
│   │   ├── package.json
│   │   └── sauce.browsers.js
│   ├── jquery
│   │   ├── dist
│   │   ├── external
│   │   ├── src
│   │   ├── AUTHORS.txt
│   │   ├── LICENSE.txt
│   │   ├── README.md
│   │   └── bower.json
│   ├── mocha
│   │   ├── CHANGELOG.md
│   │   ├── CONTRIBUTING.md
│   │   ├── LICENSE
│   │   ├── README.md
│   │   ├── bower.json
│   │   ├── mocha.css
│   │   └── mocha.js
│   └── modernizr
│       ├── feature-detects
│       ├── media
│       ├── test
│       ├── grunt.js
│       ├── modernizr.js
│       └── readme.md
├── test
│   ├── spec
│   │   └── test.js
│   └── index.html
├── bower.json
├── gulpfile.js
└── package.json
```

> app                   前端代码目录
> bower_components      前端组件包
> test                  测试代码目录
> bower.json            bower包配置文件
> gulpfile.js           gulp工程化配合文件
> package.json          npm包配置文件

# 更多模板

已经有 5720 个项目模板了

> http://yeoman.io/generators/

