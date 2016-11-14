---
layout: post
title: javascript模块研究之 - CMD模式 seajs使用
category: javascript
tags: 
  - seajs CMD
---

# 前言 - 什么是 CMD 模式

CMD是 `Common Module Definition` 的缩写，意思就是`通用模块加载规范`。

CMD 模式的 需要载入 `Sea.js` 这个包, 浏览器兼容ie5.5+ 所以放心用吧。

> 下载地址 http://seajs.org/

模块定义方式:

```javascript
define("ID",["依赖模块"],function (require, exports, module) {

});
```

> ID 当前模块的唯一
> ["..."] 依赖模块
> require 用来载入其它模块
> exports 导出方法
> module 导出模块

## 简单模块调用

- 定义一个模块 module1.js

```javascript
// 单例模式
define(function(require, exports, module) {

    console.log("module1 被装载~");

    exports.num = 10;

    exports.add = function(){
        this.num ++;
    };

    exports.get = function(){
        return this.num;
    };

});
```

> 通过 exports 导出了2个方法 add get，一个属性 num。
> 方法中访问成员变量 this.num 方式。

- 建立调用页面 1-seajs模块调用.html

```javascript
<script src="../sea-modules/core/sea.js" id="seajsnode" ></script>
<script>
    seajs.config({
        base: "../sea-modules/",
    });

    seajs.use(['lib/module1'],function(mod1){
        mod1.add();
        mod1.add();
        console.log(mod1.get());
    });

    seajs.use(['lib/module1'],function(mod1){
        mod1.add();
        mod1.add();
        console.log(mod1.get());
    });
</script>
```

> 导入sea.js包
> 定义seajs.config 的 base 代码根目录
> seajs.use方式 使用 module1
> 连续调用了两次

- 输出

> module1 被装载~
> 12
> 14
> 
> 我们可以发现这是单例模式，作用域是整个页面，所以num是累加了。

## 非单例模式

- 定义模块

```javascript
// 非单例模式
define(function(require, exports, module) {

    console.log("module2 被装载~");

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

    module.exports = function () {
        return new UserModel();
    };
});
```

> 模型方式导出 module.exports

- 定义入口 2-seajs非单例模块.html

```javascript
<script src="../sea-modules/core/sea.js" id="seajsnode" ></script>
<script>
    seajs.config({
        base: "../sea-modules/",
    });

    seajs.use(['lib/module2'],function(mod2){

        var mod = new mod2();
        mod.add();
        mod.add();
        mod.add();

        console.log(mod.show());
    });

    seajs.use(['lib/module2'],function(mod2){

        var mod = new mod2();
        mod.add();
        mod.add();
        mod.add();

        console.log(mod.show());
    });
</script>
```

> seajs.use 调用后 是一个 function() 
> 我们用 new 的方式实例一个新对象
> 这里也是重复调用两次

- 输出

> module2 被装载~
> 1003:AAAAA:1001
> 1003:AAAAA:1001
>
> 可以发现这次是非单例了，每次都是新的对象，互不影响。

## 模块依赖

- 定义模块

```javascript
// 模块依赖
define(['module1'],function(require, exports, module) {

    console.log("module3 被装载~");

    var mod1 = require('module1');
    mod1.add();

    console.log("module1 执行完成");
});
```

> 依赖模块 module1
> require装载 module1 ，并执行 add 操作

- 定义入口 3-seajs模块依赖.html

```javascript
<script src="../sea-modules/core/sea.js" id="seajsnode" ></script>
<script>
    seajs.config({
        base: "../sea-modules/",
        alias: {
            "module1": "lib/module1.js"
        }
    });

    seajs.use(['lib/module3'],function(mod3){

        console.log("module3 执行完成");
    });
</script>
```

> seajs.config alias 设置别名 module1

- 输出

> module3 被装载~
> module1 被装载~
> module1 执行完成
> module3 执行完成
> 
>
> 可以发现 CMD 果然是顺序执行，模块是懒加载，在需要的时候加载，不会像AMD方式先加载模块。
> 下载操作，是在执行定义依赖 `define(['module1'],...` 时浏览器客户端开始下载。

## 异步方式加载

```javascript
define(function(require) {

  require.async('module1', function(mod1) {
    mod1.add();
  });

});
```

> `require.async` 异步加载一个模块，在加载完成时，执行回调

## 模块化第三方代码 jquery

规范了模块的定义虽然好，但是用第三非CMD定义模块时，就有点麻烦了，比如这个jquery，你直接 require 直接返回 `null`。

- 修改`jquery`的代码

```javascript
define(function () {

    // jquery代码
    ...
    
    // 加入return
    return $.noConflict();
});
```

> 用一个 `define` 包裹下
> 最后 `return $.noConflict();`

- 定义模块

```javascript
define("lib/module4", ["./module4-ui"], function(require) {
    var ui = require("./module4-ui");
    ui.setup();
});

define("lib/module4-ui", ["jquery"], function(require, exports, module) {

    var $ = require("jquery");

    exports.setup = function () {

        $("#myButton").click( function () {
            alert(123);
        });
    };
});
```

> 我把UI处理单独出来了，设置模块ID `define("lib/module4"`
> `var $ = require("jquery");` 方式载入jquery模块 测试一个按钮事件

- 定义入口

```javascript
<script src="../sea-modules/core/sea.js" ></script>

<script>
    seajs.config({
        base: "../sea-modules/",
        alias: {
            "jquery": "core/jquery-3.1.1.seajs.min.js"
        }
    });

    seajs.use("lib/module4");
</script>

<button id="myButton">点击我</button>
```

> 用`alias`配置了别名 模块中可以直接使用
> 放置了一个按钮`button`

- 输出

![seajs-jquery-cmd化](http://oflimcy5e.bkt.clouddn.com/seajs-jquery.png)

> 触发成功

## 模块化 调用jquery插件

同样你要用相关依赖jquery的插件，也需要修改相关代码。

我这里修改了一个确认对话框插件 [artDialog](https://github.com/aui/artDialog)

- 修改插件主程序 `dialog.js`

```javascript
define("lib/artDialog/dialog", ["jquery"], function (require, exports, module) {

    var jQuery = require("jquery");

    // 放置原来的插件代码

    module.exports = function (options, ok, cancel) {
        return new dialog(options, ok, cancel);
    };
});
```

> 首先包一个 `define` 定义
> 然后创建依赖需要的 `var jQuery = require("jquery");` 对象
> 最后导出定义模块定义 `module.exports = function ... `

- 修改上面的模块代码

```javascript
define("lib/module4", ["./module4-ui"], function(require) {
    var ui = require("./module4-ui");
    ui.setup();
});

define("lib/module4-ui", ["jquery","dialog"], function(require, exports, module) {

    var $ = require("jquery");
    var dialog = require("dialog");

    exports.setup = function () {

        $("#myButton").click( function () {
 
            var d = dialog({
                title: '欢迎',
                content: '欢迎使用 artDialog 对话框组件！'
            });
            d.showModal();
        });
    };
});
```

> 读取调用 `var dialog = require("dialog");`

- 修改上面的入口

```javascript
<script src="../sea-modules/core/sea.js" ></script>
<script src="../sea-modules/core/seajs-css.js" ></script>

<script>
    seajs.config({
        base: "../sea-modules/",
        alias: {
            "jquery": "core/jquery-3.1.1.seajs.min.js",
            "dialog": "lib/artDialog/dialog.js"
        }
    });

    seajs.use("lib/artDialog/ui-dialog.css");
    seajs.use("lib/module4");


</script>

<button id="myButton">点击我</button>
```

> 加入alias别名 `dialog`
> 这里用了一个 `seajs-css` 的插件动态读取了css
> 插件地址 https://github.com/seajs/seajs-css/blob/master/README.md

- 输出

![seajs-jquery-cmd化-插件](http://oflimcy5e.bkt.clouddn.com/seajs-jquery-plus.png)

> 弹出成功!
 

## 参考代码

> https://github.com/hans007/JavaScriptCodes/tree/master/seajs%E6%A8%A1%E5%9D%97%E5%8C%96/app

[我的博客](https://hans007.github.io)