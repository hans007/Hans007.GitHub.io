---
layout: post
title: 前端自动化构建工具 - 第二节:webpack2 Loader
category: 前端自动化构建工具
tags:
  - webpack2
---

# Loader

## 介绍

Loaders是webpack中最让人激动人心的功能之一了。

- 配置选项

> test：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）
> loader：loader的名称（必须）
> include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
> query：为loaders提供额外的设置选项（可选）

## 处理 json 的例子

- 安装 json-loader

```
npm install --save-dev json-loader
```

- webpack.config.js

```
    module: {
        loaders: [
            {
                test: /\.json$/,
                loader: "json-loader"
            }
        ]
    },
```

- 运行

```
npm start
```

## Babel 一个编译JavaScript的平台

- 安装插件

```
核心包
npm install --save-dev babel-core babel-loader

解析Es6的babel-preset-es2015包
npm install --save-dev babel-preset-es2015

解析JSX的babel-preset-react包
npm install --save-dev babel-preset-react

使用React包
npm install --save-dev react react-dom
```

- Greeter.js

```javascript
import React, {Component} from 'react'
import config from './config.json';

class Greeter extends Component{
  render() {
    return (
      <div>
        {config.greetText}
      </div>
    );
  }
}

export default Greeter
```

- main.js

```javascript
import React from 'react';
import {render} from 'react-dom';
import Greeter from './greeter';

render(<Greeter />, document.getElementById('root'));
```

- webpack.config.js

```
    module: {
        loaders: [
            {
                test: /\.json$/,
                loader: "json-loader"
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',//在webpack的module部分的loaders里进行配置即可
                query: {
                    presets: ['es2015','react']
                }
            }
        ]
    },
```

- 运行

```
npm start
```

- webpack会自动调用.babelrc里的babel配置选项

```javascript
//.babelrc
{
  "presets": ["react", "es2015"]
}
```

## CSS

- 安装

```
npm install --save-dev style-loader css-loader
```

- main.css

```css
html {
  box-sizing: border-box;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}

*, *:before, *:after {
  box-sizing: inherit;
}

body {
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  background-color: blanchedalmond;
}

h1, h2, h3, h4, h5, h6, p, ul {
  margin: 0;
  padding: 0;
}
```

- main.js

```javascript
import React from 'react';
import {render} from 'react-dom';
import Greeter from './greeter';

import './main.css';//使用require导入css文件

render(<Greeter />, document.getElementById('root'));
```

- Greeter.css

```css
.root {
  background-color: #eee;
  padding: 10px;
  border: 3px solid #ccc;
}
```

- Greeter.js

```
import React, {Component} from 'react'
import config from './config.json';
import styles from './Greeter.css';//导入

class Greeter extends Component{
  render() {
    return (
      <div className={styles.root}>
        {config.greetText}
      </div>
    );
  }
}

export default Greeter
```

- webpack.config.js

```javascript
module.exports = {
    devtool: 'eval-source-map',
    entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
    output: {
        path: __dirname + "/build",//打包后的文件存放的地方
        filename: "bundle.js"//打包后输出文件的文件名
    },

    module: {
        loaders: [
            {
                test: /\.json$/,
                loader: "json-loader"
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader'//在webpack的module部分的loaders里进行配置即可
            },
            {
                test: /\.css$/,
                loaders: [
                    'style-loader',
                    'css-loader?modules',
                    'postcss-loader'
                ]
            }
        ]
    },

    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        compress: true,
        port: 9000
    }
}
```

- 运行

```
npm start
```

## PostCSS

> 一个CSS的处理平台-PostCSS
> https://github.com/postcss/postcss

- webpack.config.js

```javascript
    {
        test: /\.css$/,
        loaders: [
            'style-loader',
            'css-loader?modules',
            'postcss-loader'
        ]
    }
```

- 配置文件 postcss.config.js

```javascript
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}
```

