---
layout: post
title: javascript闭包实现 单例与非单例
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