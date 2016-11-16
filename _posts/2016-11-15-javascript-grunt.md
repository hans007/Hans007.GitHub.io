---
layout: post
title: JavaScript的构建工具 - Grunt
category: JavaScript的构建工具
tags: 
  - javascript
  - Grunt
---

# 前言 - Grunt 到底是啥?

![grunt](http://oflimcy5e.bkt.clouddn.com/grunt%E6%A0%87%E5%BF%97.png)

工作中我们会遇到，对代码(js,css,html)就行加工：压缩、合并、代码检查、测试 等等。

我们需要一个集成的环境 就行操作，Grunt就是这样的工具。

是一个js开发框架，具体功能是以安装插件的形式实现的，所以扩展性无限。

---

# 安装

## 安装 node.js

Grunt是基于Node.js为基础的，所以没装node.js的需要安装下。

> http://nodejs.org/

已经安装的可以更新下

```
npm update -g npm
```

> macos 下加 sudo

## 安装 Grunt CLI

```
npm install -g grunt-cli
```

---

# 配置我们第一个项目

## 建立项目目录

```
/grunt-do
   src/
   css/
   dist/
```

> src 开发的js代码
> css 开发的css代码
> dist 发布代码

## 根目录下创建 `package.json`

```
{
  "name": "myapp",
  "version": "0.1.1",
  "devDependencies": {
  }
}
```

## 安装Grunt

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

## 根目录下创建 `Gruntfile.js`

```
module.exports = function(grunt) {

    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json')
    });

};
```

> 这是Grunt的配置文件
> initConfig初始pkg属性，这都是套路，读取上面的package.json
>

# 插件

上面我们完成了项目的初始，具体的工作grunt都是以插件的形式实现的。

## 合并代码

- 编写1个js文件

src/mods/module1.js

```javascript
// 模块1
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

src/mods/module2.js

```javascript
// 模块2
define(function(require, exports, module) {

    console.log("module2 被装载~");

    exports.num = 10;

    exports.add = function(){
        this.num ++;
    };

    exports.get = function(){
        return this.num;
    };

});
```

- 安装插件

命令行

```
npm install grunt-contrib-concat --save-dev
```

- 编写 Gruntfile.js

```
module.exports = function(grunt) {

    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),

        // 合并
        concat:{
            options:{
                separator:'\n',
                banner:'//model合并 \n',
                footer:'\n//--- end ---',
            },
            myapp:{
                src:['src/mods/*.js'],
                dest:'dist/<%= pkg.version %>/module.js'
            }
        },
    });

    grunt.loadNpmTasks('grunt-contrib-concat');

    grunt.registerTask('test', ['concat']);
};
```

- 运行&输出

命令行

```
grunt test
```

查看合并后的文件 dist/<版本号>/module.js

```
//model合并 
// 模块1
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
// 模块2
define(function(require, exports, module) {

    console.log("module2 被装载~");

    exports.num = 10;

    exports.add = function(){
        this.num ++;
    };

    exports.get = function(){
        return this.num;
    };

});
//--- end ---
```

---
---

## 压缩js代码



## 压缩css代码

## 代码检查

## 代码测试

## CMD模式代码发布

## 监控文件变化

## 清除中间文件

# 配置git `.gitignore`

过滤掉`node_modules`目录

```
node_modules/
```

# 拿到一个现有项目

- 安装需要的包文件

```
npm install
```

- 查看 `Gruntfile.js` 的 `grunt.registerTask` 定义

> 执行`grunt`看看是否有错误

```
grunt
```

# 参考代码

>

[我的博客](https://hans007.github.io)