---
layout: post
title: javascript模块研究之 - AMD模式 requirejs使用
category: javascript
tags: 
  - requirejs
---

# 前言 - 什么是 AMD 模式

AMD是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。

说白了就是先装载模块再使用，AMD 模式的 需要载入 `requirejs` 这个包, 浏览器兼容ie6+ 所以放心用吧。

> 下载地址 http://requirejs.org/

使用方式:

```javascript
require(['模块文件名称'], function (对象名){

    对象名.方法();
});
```

可以看到当require调用模块成功后，才会回调 function 这个函数。

## 模块调用 单例方式

- 首先定义模块 module1.js

```javascript
// 单例
define(function () {

    console.log('module1 被装载~');

    var num = 0;

    var add = function () {
        num++;
    };

    var get = function () {
        return num;
    };

    return{
        add : add,
        get : get
    };
});
```

define方式包裹一个 function。

- 主界面html

```html
<script src="../core/require.js" data-main="main1"></script>
```

页面中调入 require.js, 标签节点 `data-main` 表示主控制js文件。

- 主控制 main1.js

```javascript
// 第一次调用
require(['lib/module1'], function (mod1){

    mod1.add();
    mod1.add();
    console.log(mod1.get());
});

// 第二次调用
require(['lib/module1'], function (mod1){

    mod1.add();
    mod1.add();
    console.log(mod1.get());
});
```

为了看看 作用域 ，我这里 调用了两次。

- 输出

> module1 被装载~
> 2
> 4

可以看出 模块被`装载`后，整个页面全局有效。

## 非单例方式

有的模块需要非单例，否则每次都是全局使用。

- 首先定义模块 module2.js

```javascript
// 非单例
define(function () {

    console.log('module2 被装载~');

    function UserModel() {

        var num = 1000;
        var name = "AAAAA";
        var uid = "1001";

        this.add = function () {
            num ++;
        };
        this.show = function () {
            return num + ":" + name + ":" + uid;
        };
    }

    return function () {
        return new UserModel();
    };
});
```

我这里 嵌套了 function，然后 返回的时候 new 了一个返回。

- 主界面html

```html
<script src="../core/require.js" data-main="main2"></script>
```

这里主控制是 main2.js

- 主控制 main2.js

```javascript
// 第一次调用
require(['lib/module2'], function (mod2){

    var mod = mod2();
    mod.add();
    mod.add();
    mod.add();

    console.log(mod.show());
});

// 第二次调用
require(['lib/module2'], function (mod2){

    var mod = mod2();
    mod.add();
    mod.add();
    mod.add();

    console.log(mod.show());
});
```

这里，我还是调用了两次。

- 输出

> module2 被装载~
> 1003:AAAAA:1001
> 1003:AAAAA:1001

可以看出 每次调用的都是`全新`的实例对象，互不干扰。

## 继承调用

- 首先定义模块 module3.js

```javascript
// 继承
define(['module1'],function (mod1) {

    console.log('module3 被装载~');

    mod1.show = function () {
        return "number:"+mod1.get();
    };

    return function () {
        return mod1;
    };
});
```

define(['module1'] 第一个参数就是依赖的模块文件名。

对象被传入后，直接扩展，最后返回被修改的对象。

- 主界面html

```html
<script src="../core/require.js" data-main="main3"></script>
```

这里主控制是 main3.js。

- 主控制 main3.js

```javascript
// 配置
require.config({
    baseUrl: "lib/"
});

// 第一次调用
require(['module3'], function (mod3){

    mod3().add();
    mod3().add();
    mod3().add();

    console.log(mod3().show());
});

// 第一次调用
require(['module3'], function (mod3){

    mod3().add();
    mod3().add();
    mod3().add();

    console.log(mod3().show());
});
```

require.config 是配置项目。

baseUrl 指模块的默认路径。

这里，我还是调用了两次。

- 输出

> module1 被装载~
> module3 被装载~
> number:3
> number:4

可以看出 当多模块依赖的时候，都是先装载完成模块后 再依次执行命令，很符合浏览器行为。

## 别名调用

- 主界面html

```html
<script src="../core/require.js" data-main="main4"></script>
```

这里主控制是 main4.js。

- 主控制 main4.js

```javascript
// 配置
require.config({
    paths: {
        "jquery" : "../core/jquery-3.1.1.min"
    }
});

// 第一次调用
require(['jquery'], function (jq){

    var obj1 = jq(document);
    console.log(obj1[0].charset);

    var obj2 = $(document);
    console.log(obj2[0].charset);
});
```

paths 指定文件路径。

require的时候直接用别名就行。

- 输出

> windows-1252
> windows-1252

jquery被载入后 $ 符号 还是可以用的。

## 参考代码

> https://github.com/hans007/JavaScriptCodes/tree/master/requirejs%E6%A8%A1%E5%9D%97%E5%8C%96

[我的博客](https://hans007.github.io)