---
layout: post
title: 思考 - PC前端页面组件化架构设计
category: 思考
tags:
  - 前端
---

# 前言

## 设计范围

- PC前端项目
- 如果需要前后端协同，需要可以预览的调试环境，服务器端渲染支持标签方式，ajax-cross
- 考虑到PC前端浏览器的兼容性，技术用 bootstrap+jquery
- js需要模块化开发，amd cmd commondjs es6

# 页面的抽象思考

## 基调

- 版本

xx.xx.xx

- 字体

- 主色调

配置 1~5 种颜色，等级递减

## 元素

- 控件

文本、输入框、按钮、下拉框、日期框 等等

- 状态

hover、focus、disabled

## 组件

元素的组合，偏向于功能独立的模块

比如 导航栏、头像、卡片

## 组合

有组件+元素合并而成，更偏向于业务

比如用户登录框，商品列表

## 布局

栅格系统

## 页面

最终的融合，体现具体业务

如 首页、列表页、详情页

# 可配置的组件化实现

## 设计目标

- 按之前的分析，细化成低耦合的模块
- 通过配置+编译进行发布
- 符合4个原则：标准性、组合性、重用性、可维护性

## 技术采用

- 前端打包及构建工具

fis3

- css模块化

less sass stylus

- js模块化

require.js
sea.js

- 包管理

npm

## 目录设计

> 带 * 目录/文件 名都是固定的

```
|--node_modules           | * node包管理目录（静态前端项目不要启用）
|--plugins                | * 手动下载整理的第三方包目录
|  |--bootstrap           | * 手动整理的 bootstrap
|  |--jquery              | * 手动整理的 jquery
|  `--jqueryui            | * 手动整理的 jqueryui
|--components             | * 项目组件目录
|  |--css-modules         | * css模块目录
|     |--bootstrap-sass   | * bootstrap 重写元素目录
|     |--sass             | * sass目录
|        |--skins         | * 皮肤目录
|        |--variables     | * 参数目录
|        |--variables.scss| * 参数汇总文件
|        |--mixins        | * 宏目录
|        `--mixins.scss   | * 宏汇总文件
|  |--js-modules          | * js模块化目录
|  `--ui-modules          | * 前端模块目录
|     |--header           | 一个组件一个目录(独立)
|        |--setup.json    | * 组件呈现用的默认json文件
|        |--header.html   | 存放组件的所有文件
|        |--header.js     | 组件的js文件
|        |--header.sass   | 可编译的 sass less 样式文件
|        `--logo.png      | 组件用到的图片
|     |--tab              |
|     |--footer           |
|     `--banner           |
|--pages                  | * 前端页面目录
|  |--index               | 一个页面一个目录(独立)
|     |--setup.json       | * 页面呈现用的默认json文件
|     |--index.html       | 存放页面所产生的所有文件
|     |--index.js         |
|     |--index.less       |
|     `--ad1.png          |
|  |--list                |
|  `--info                |
|--assets                 | * 资源目录 js css image flash 可以放在cdn上
|  `--img                 | * 图片
|--release                | * 发布目录
|  |--dist                | * 交付目录（产品独立包）
|--doc                    | * 文档目录
|--README.md              | * 项目说明文件
|--.gitignore             | * git过滤文件
|--.jshintrc              | * jshint语法检查配置
|--package.json           | * 包管理配置文件
`--fis-config.js          | * fis配置文件
```

## sass 变量方式配置参数

变量分类成单独文件配置

### skins目录下配置颜色

抽象固定的配色方案

- 蓝色主题

> sass/skins/skin-blue.scss 

```sass
```

### variables目录下分类配置

- 颜色

> sass/variables/colors.scss

```sass
// 3色 4色 配色方案
$master-color-0: #fff;//  白色：组件背景、反转字体、、
$master-color-1: #666;// 黑色：字体颜色、、、、
$master-color-2: #57bc4c;// 绿色：元素背景、反转、、、
$master-color-3: #474747;//  黑色：组件背景、反转字体、、
$master-color-4: #f60;// 橙色：高亮凸显、、、、

$body-bg: #f4f4f4;// 灰色：主体背景、、、
```

- 字体类型及大小

> sass/variables/fonts.scss

```scss
$master-font: "Microsoft Yahei";  // “微软雅黑”

$font-size-base: 14px; //14px 基础大小
$font-size-16: ceil(($font-size-base * 1.14)); // 16px
$font-size-12: ceil(($font-size-base * 1.14)); // 12px

$font-size-h1: floor(($font-size-base * 2.6)); // h1:36px
$font-size-h2: floor(($font-size-base * 2.15));// h2:30px
$font-size-h3: ceil(($font-size-base * 1.7));  // h3:24px
$font-size-h4: ceil(($font-size-base * 1.25)); // h4:18px
$font-size-h5: $font-size-base;                // h5: 14px
```

- 间距

> sass/variables/space.scss

```scss
$line-height-base: 1.428571429;  // 基础行高
$line-height-components: floor(($font-size-base * $line-height-base)); // 基础行高：20px
$gutter-width: 5px;    //重置列与列之间的间距，间距以此为基础
$min-height: 40px;     //组件的最小高度
```

-  配置汇总

> sass/variables.scss

```scss
@import "./variables/colors";
@import "./variables/fonts";
@import "./variables/space";
...
```

## sass 重复性定义采用mixins

### mixins下放置sass宏

- form表单

> sass/mixins/form.scss
> 采用参数方式传入

```scss
.search-form(@var1, @var2){
  .form-group{
    position: relative;
    margin-bottom:0;
    .input-search{
      position: absolute;
      top:($min-height * .9 - $font-size-h4 ) / 2 ;
      left: $gutter-width * 4 ;
      font-size: $font-size-h4;
      color: #bababa;
    }
    ...
  }
```

-  配置汇总

> sass/mixins/mixins.scss

```scss
@import "./mixins/form.scss";
...
```

## .gitignore 配置

```

```

## package.json 配置

## fis-conf.js 配置

## 实时预览

## 发布


