---
layout: post
title: javascript模块研究之 - 闭包实现
category: javascript
tags: 
  - closure
---

# 前言 - 为什么要闭包

我高度概括几点吧

> 1. 实现面向对象 类
> 2. 代码便于阅读
> 3. 代码之间不会互相污染
> 4. 提高复用性

## 单例闭包

代码:

```javascript
// 声明
var myobjectModel = new Object({
    num : 0,
    add : function() {
        myobjectModel.num ++;
    },
    get : function() {
        return myobjectModel.num;
    }
});

// 调用
myobjectModel.add();
myobjectModel.add();
myobjectModel.add();
var val = myobjectModel.get();
```

输出:

> 3

> 直接对象访问 无需 new , 特别适合功能对象函数

## 非单例闭包

代码:

```javascript
// 声明
var myobjectModel = function () {
     var instance = new Object();
     instance.num = 0;

     instance.add = function () {
         instance.num ++;
     };

     instance.get = function () {
         return instance.num;
     };

     return instance;
};

// 调用
var my1 = new myobjectModel();
my1.add();
my1.add();
var val1 = my1.get();
log(val1);

var my2 = new myobjectModel();
my2.add();
my2.add();
var val2 = my2.get();
log(val2);
```

输出:

> 2
> 2

> 对象被new后，互不影响。
> 适合可复用组件封装。

## 如果想内部对象私有化

代码:

```javascript
// 声明
var myobjectModel = function () {
     var instance = new Object();
     instance.num = 0;

     instance.add = function () {
         instance.num ++;
     };

     instance.get = function () {
         return instance.num;
     };

     return {
         add : instance.add,
         get : instance.get
     };
};
```

运行时:

![闭包私有化](http://oflimcy5e.bkt.clouddn.com/closure.png)

## 如果需要其它模块支持

代码:

```javascript
var myModule = (function(mod){
    
    ...
    
})(otherModule);
```

> 对象作为`参数`传入

## 如果各种原因模块没有载入成功

代码:

```javascript
var myModule = (function(mod){
    
    ...
    
})(otherModule || {});
```

> 默认一个`空对象`进去

## 参考代码

> https://github.com/hans007/JavaScriptCodes/tree/master/JS%E9%97%AD%E5%8C%85



[我的博客](https://hans007.github.io)