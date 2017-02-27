---
layout: post
title: 编码工具 - 脚手架yeoman - 创建自己的脚手架
category: yeoman
tags:
  - 编码工具
---

# 前言

创建自己的项目脚手架

# 安装开发工具

```
npm install -g yo generator-generator
```

# 创建脚手架项目

```
yo generator
```

> 输入脚手架名称
> 说明、版权、作者、官网等等信息

# 目录分析

```
├── generators                    脚手架目录
│   └── app                       脚手架主程序目录
│       ├── templates             脚手架模板代码
│       └── index.js              脚手架主程序
├── test                          测试目录
│   └── app.js
├── .editorconfig                 书写规范文件
├── .gitattributes                指定非文本文件的对比合并方式
├── .gitignore                    用于忽略你不想提交到Git上的文件
├── .travis.yml                   Travis持续集成配置
├── LICENSE                       版权
├── README.md                     说明
├── gulpfile.js                   gulp配置
└── package.json                  nodejs包配置
```

# 入口程序index.js

```javascript
'use strict';
var Generator = require('yeoman-generator');
var chalk = require('chalk');
var yosay = require('yosay');

module.exports = Generator.extend({

  // 交互输入选项
  prompting: function () {
    
    // 启动画面文字
    this.log(yosay(
      'Welcome to the legendary ' + chalk.red('generator-itcast-webapp') + ' generator!'
    ));

    // 是非询问
    var prompts = [{
      type: 'confirm',
      name: 'someAnswer',
      message: 'Would you like to enable this option?',
      default: true
    }];

    // 保存交互输入信息
    return this.prompt(prompts).then(function (props) {
      // To access props later use this.props.someAnswer;
      this.props = props;
    }.bind(this));
  },

  // 写操作
  writing: function () {
    // 支持模板
    this.fs.copy(
      this.templatePath('dummyfile.txt'),
      this.destinationPath('dummyfile.txt')
    );
  },

  // 最后安装依赖项目
  install: function () {
    this.installDependencies();
  }
});
```

# 本地方式发布

```
npm link
```

> 你可以把脚手架项目放到git空间
> 用户可以git clone下来
> 如果有更新git pull操作

# npm平台发布

> 如果你有 npmjs 平台的账号直接发布上传
> 用户每次就可以直接 npm install -g generator-`你的脚手架项目名称`
> 貌似每次脚手架更新还是需要更新一次，性质和git一样，但是都是 `npm cli` 操作比较方便

- 注册账号

https://www.npmjs.com/signup

- cli登录

```bash
npm login
```

> 输入账号、密码、联系email

- 发布

```bash
npm publish
```

- 常见错误

> no_perms Private mode enable, only admin can publish this module
> 修改仓库地址 npm config set registry http://registry.npmjs.org
> 修改完后 还是需要重新 npm login

# 安装使用

```
yo 你的脚手架项目名称
```

> 如何使用就不多说了，yo 命令就行 ，你可以发现 `generator-` 是不用输入的

