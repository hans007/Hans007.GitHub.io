---
layout: post
title: 思考 - 前端页面组件化
category: 思考
tags:
  - 前端
---

# 前言

提高前端设计的复用性和规范考虑，进行组件化设计。

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

目录名称                   | 说明
--------------------------|---------------------------------
|--node_modules           | * node包管理目录（静态前端项目不要启用）
|--assets                 | 资源目录 js css image flash 可以放在cdn上
|  `--img                 | 图片
|--plugins                | * 手动下载整理的第三方包目录
|  |--bootstrap           | 手动整理的 bootstrap
|  |--jquery              | 手动整理的 jquery
|  `--jqueryui            | 手动整理的 jqueryui
|--components             | * 项目组件目录
|  |--css-modules         | * css模块目录
|     |--bootstrap-sass   | * bootstrap 重写元素目录
|     |--sass             |
|        |--skins         | * 皮肤目录
|        |--variables     | * 参数目录
|        |--variables.scss| * 参数汇总文件
|        |--mixins        | * 宏目录
|        `--mixins.scss   | * 宏汇总文件
  /js-modules             | * js模块化目录
    /app.config.js        | 项目配置js文件
    /ajax.js              | ajax调用js文件
  /ui-modules             | * 前端模块目录
    /header               | 一个组件一个目录(独立)
      /setup.json         | * 组件呈现用的默认json文件
      /header.html        | 存放组件的所有文件
      /header.js          | 组件的js文件
      /header.sass        | 可编译的 sass less 样式文件
      /logo.png           | 组件用到的图片
    /tab                  |
    /footer               |
    /banner               |
/pages                    | * 前端页面目录
  /index                  | 一个页面一个目录(独立)
    /setup.json           | * 页面呈现用的默认json文件
    /index.html           | 存放页面所产生的所有文件
    /index.js             |
    /index.less           |
    /ad1.png              |
  /list                   |
  /info                   |
/release/dist             | * 发布目录
  /img                    | * 图片目录
  /js                     | * js目录
  /css                    | * css目录
  /page                   | * 页面目录
/doc                      | 文档目录
README.md                 | 项目说明文件
.gitignore                | git过滤文件
.jshintrc                 | jshint语法检查配置
package.json              | 包管理配置文件
fis-config.js             | fis配置文件

## css 模块设计

- 目录文件

- 依赖关系

- 使用方式

## .gitignore 配置

## package.json 配置

## fis-conf.js 配置

## 实时预览

## 发布


