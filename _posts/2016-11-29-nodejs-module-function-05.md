---
layout: post
title: nodejs学习资料 - 第五节:模块系统&函数&路由
category: nodejs学习资料
tags:
  - nodejs
---

# 模块系统

## 创建模块

- mod1.js

```javascript
exports.say = function(val){
    console.log("say : "+val);
};
```

- mod2.js

```javascript
function Hello() {
    var num;
    this.add = function (val) {
        num = val;
    };
    this.say = function () {
        console.log("hello : " + num);
    };
};
module.exports = Hello;
```

- main.js

```javascript
var mod1 = require("./mod1");
var Hello = require("./mod2");

mod1.say("hello!");

var mod2 = new Hello();
mod2.add(12);
mod2.say();
```

## 模块调用规则

> 原生模块缓存 -> 原生模块目录 -> 文件模块缓存 -> 文件模块目录

# 函数参数

```javascript
function say(word) {
  console.log(word);
}

function execute(someFunction, value) {
  someFunction(value);
}

execute(say, "Hello");
```

# 匿名函数

```javascript
function execute(someFunction, value) {
  someFunction(value);
}

execute(function(word){
    console.log(word)
    },
    "Hello");
```

# 路由

- 用到两个模块

> url 分析地址
> querystring 解析参数

> 手册
> https://nodejs.org/api/url.html
> https://nodejs.org/api/querystring.html

- router.js

```javascript
exports.route = function (pathName) {
    console.log("route : " + pathName);
};
```

- server.js

```javascript
var http = require("http");
var url = require("url");

function start(route) {
    function onRequest(request, response) {
        var pathname = url.parse(request.url).pathname;
        console.log("Request for " + pathname + " received.");

        route(pathname);

        response.writeHead(200, { "Content-Type": "text/plain" });
        response.write("Hello World");
        response.end();
    }

    http.createServer(onRequest).listen(8888);
    console.log("Server has started.");
}

exports.start = start;
```

- main.js

```javascript
var server = require("./server");
var router = require("./router");

server.start(router.route);
```

- 运行

```
$ node main.js
```

> 浏览器输入 http://127.0.0.1:8888/

- 输出

```
Server has started.
Request for /%EF%BC%8C%E8%BE%93%E5%87%BA%E7%BB%93%E6%9E%9C%E5%A6%82%E4%B8%8B%EF%BC%9A received.
route : /%EF%BC%8C%E8%BE%93%E5%87%BA%E7%BB%93%E6%9E%9C%E5%A6%82%E4%B8%8B%EF%BC%9A
```

# 代码

> https://github.com/hans007/JavaScriptCodes/tree/master/nodejs-do

[我的博客](https://hans007.github.io)