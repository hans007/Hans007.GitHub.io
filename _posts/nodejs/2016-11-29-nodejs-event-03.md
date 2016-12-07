---
layout: post
title: nodejs - 第三节:回调函数&事件循环
category: nodejs
tags:
  - nodejs
---

# 本机开发环境

开发工具:Atom

自动提示插件:atom-ternjs

# 回调函数

新建数据文件 title.txt

```
京东(JD.COM)-综合网购首选-正品低价、品质保障、配送及时、轻松购物！
```

## 阻塞方式

- 新建  readSync.js

```javascript
var fs = require("fs");

var data = fs.readFileSync("title.txt");

console.log(data.toString());
console.log("--- end ---");
```

- 运行

```
node readSync.js
```

- 输出

```
京东(JD.COM)-综合网购首选-正品低价、品质保障、配送及时、轻松购物！
--- end ---
```

## 非阻塞方式

- 新建  read.js

```javascript
var fs = require("fs");

fs.readFile("title.txt", function(err, data){
  if(err){
    return console.error(err);
  }
  console.log(data.toString());
});

console.log("--- end ---");
```

- 运行

```
node read.js
```

- 输出

```
--- end ---
京东(JD.COM)-综合网购首选-正品低价、品质保障、配送及时、轻松购物！
```

# 事件循环

这是整个nodejs的核心，全是异步、全是事件接收、队列处理。

## 一个用户登录

- 新建 main.js

```javascript
var events = require("events");
var eventEmitter = new events.EventEmitter();

var listeneUserLogin = function(name,pwd){
  console.log("用户登录:"+name+pwd);
};

eventEmitter.addListener("userLogin", listeneUserLogin);

eventEmitter.emit("userLogin","hans","123456");

console.log("---end---");
```

- 运行&输出

```
$ node main.js
```

```
用户登录:hans123456
---end---
```

# 事件类方法

## addListener(event, listener)

为指定事件添加一个监听器到监听器数组的尾部。

## on(event, listener)

为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。

## once(event, listener)

为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。

## removeListener(event, listener)

移除指定事件的某个监听器，监听器 必须是该事件已经注册过的监听器。

## removeAllListeners([event])

移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。

## setMaxListeners(n)

默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。

## listeners(event)

返回指定事件的监听器数组。

## emit(event, [arg1], [arg2], [...])

按参数的顺序执行每个监听器，如果事件有注册监听返回 true，否则返回 false。

## 类方法 listenerCount(emitter, event)

返回指定事件的监听器数量。

## newListener

event - 字符串，事件名称;
listener - 处理事件函数 
该事件在添加新监听器时被触发。

## removeListener

event - 字符串，事件名称
listener - 处理事件函数
从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引。

## error 事件

emitter.emit('error');

# 代码

> https://github.com/hans007/JavaScriptCodes/tree/master/nodejs-do

[我的博客](https://hans007.github.io)