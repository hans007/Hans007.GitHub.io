---
layout: post
title: 前端自动化构建工具 - 第三节:webpack2 Plugin
category: 前端自动化构建工具
tags:
  - webpack2
---

# Plugin

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

## 内置插件 - BannerPlugin

- webpack.config.js

```javascript

    var webpack = require('webpack');

    ...

    plugins: [
        new webpack.BannerPlugin("Copyright Flying Unicorns inc.")//在这个数组中new一个就可以了
    ],
```

- 运行

```
npm start
```

## 第三方插件 HtmlWebpackPlugin

- 安装

```javascript
npm install --save-dev html-webpack-plugin
```

- app/index.tmpl.html

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
  </body>
</html>
```

- webpack.config.js

```javascript
    plugins: [
        new HtmlWebpackPlugin({
            template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
        })
    ],
```

- 运行

```
npm start
```

## Hot Module Replacement 自动刷新

Hot Module Replacement（HMR）也是webpack里很有用的一个插件，它允许你在修改组件代码后，自动刷新实时预览修改后的效果。
在webpack中实现HMR也很简单，只需要做两项配置

> 在webpack配置文件中添加HMR插件；
> 在Webpack Dev Server中添加“hot”参数；

不过配置完这些后，JS模块其实还是不能自动热加载的，还需要在你的JS模块中执行一个Webpack提供的API才能实现热加载，虽然这个API不难使用，但是如果是React模块，使用我们已经熟悉的Babel可以更方便的实现功能热加载。

整理下我们的思路，具体实现方法如下

> Babel和webpack是独立的工具
> 二者可以一起工作
> 二者都可以通过插件拓展功能
> HMR是一个webpack插件，它让你能浏览器中实时观察模块修改后的效果，但是如果你想让它工作，需要对模块进行额外的配额；
> Babel有一个叫做react-transform-hrm的插件，可以在不对React模块进行额外的配置的前提下让HMR正常工作；

- 安装

```
npm install --save-dev babel-plugin-react-transform react-transform-hmr
```

- webpack.config.js

```javascript
    plugins: [
        ...,
        new webpack.HotModuleReplacementPlugin()//热加载插件
    ],
```

.babelrc

```javascript
{
    "presets": [
        "es2015",
        "react"
    ],
    "env": {
        "development": {
            "plugins": [
                [
                    "react-transform",
                    {
                        "transforms": [
                            {
                                "transform": "react-transform-hmr",
                                "imports": [
                                    "react"
                                ],
                                "locals": [
                                    "module"
                                ]
                            }
                        ]
                    }
                ]
            ]
        }
    }
}
```

## 产品阶段的构建

- 独立一个 webpack.production.config.js

```javascript
    ....
    output: {
        path: __dirname + "/release",//打包后的文件存放的地方
        filename: "bundle.js"//打包后输出文件的文件名
    },
    ...
```

- package.json

```javascript
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    "start": "webpack-dev-server --progress",
    "release": "NODE_ENV=production webpack --config ./webpack.production.config.js --progress"
  },
  "author": "Cássio Zen",
  "license": "ISC",
  "devDependencies": {...},
  "dependencies": {...}
}
```

> 就是指定配置文件

- 运行

```
npm run release
```

## 优化插件 extract-text-webpack-plugin

webpack提供了一些在发布阶段非常有用的优化插件，它们大多来自于webpack社区，可以通过npm安装，通过以下插件可以完成产品发布阶段所需的功能

> OccurenceOrderPlugin :为组件分配ID，通过这个插件webpack可以分析和优先考虑使用最多的模块，并为它们分配最小的ID
> UglifyJsPlugin：压缩JS代码；
> ExtractTextPlugin：分离CSS和JS文件

https://webpack.js.org/guides/production-build/


