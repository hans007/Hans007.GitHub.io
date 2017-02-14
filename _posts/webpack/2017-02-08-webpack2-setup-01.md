---
layout: post
title: 前端自动化构建工具 - 第一节:webpack2 安装
category: 前端自动化构建工具
tags:
  - webpack2
---

# 开始

## 初始目录

```
npm init
```

## 本地安装

```
npm install webpack --save-dev
```

## 编译文件

- 新建 ./public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
    <script src="bundle.js"></script>
  </body>
</html>
```

- 新建 ./app/main.js

```javascript
module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = "Hi there and greetings!";
  return greet;
};
```

- 新建 ./app/greeter.js

```javascript
var greeter = require('./greeter.js');
document.getElementById('root').appendChild(greeter());
```

- 编译

```
node_modules/.bin/webpack app/main.js public/bundle.js
```

## 配置文件方式

- 添加 webpack.config.js

```javascript
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
```

- 编译

```
node_modules/.bin/webpack
```

## 简化本地调用

- 修改 package.json

```json
{
  "name": "02",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^2.2.1"
  }
}
```

> package.json中的脚本部分已经默认在命令前添加了node_modules/.bin路径，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。

- 运行

```
npm start
```

> npm的start是一个特殊的脚本名称，它的特殊性表现在，在命令行中使用npm start就可以执行相关命令，如果对应的此脚本名称不是start，想要在命令行中运行时，需要这样用npm run {script name}如npm run build

## Source Maps 帮助调试

- devtool选项

选项 | 配置结果
-----|-------------
source-map                   | 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包文件的构建速度；
cheap-module-source-map      | 在一个单独的文件中生成一个不带列映射的map，不带列映射提高项目构建速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便；
eval-source-map              | 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。不过在开发阶段这是一个非常好的选项，但是在生产阶段一定不要用这个选项；
cheap-module-eval-source-map | 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点；

- webpack.config.js

```
module.exports = {
    devtool: 'eval-source-map',
    entry:  ...
}
```

## 本地WebServer开发

- 安装

```
npm install webpack-dev-server --save-dev
```

- 配置选项

> https://webpack.js.org/configuration/dev-server/#components/sidebar/sidebar.jsx

- webpack.config.js

```
module.exports = {
    ...,
    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        compress: true,
        port: 9000
    }
}
```

- 运行

```
node_modules/.bin/webpack-dev-server
```

or package.json

```
{
  ...,
  "scripts": {
    "start": "webpack",
    "server": "webpack-dev-server"
  },
  ...
}
```

```
npm run server
```

## Loader



