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

fis/webpackage/glup/grunt

- css模块化

less sass stylus

- js模块化

require.js
sea.js

- 包管理

npm

## 目录设计

目录名称                   | 说明
--------------------------|---------------------------------
/components               |
  /css-module             |
  /js-module              |
  /ui-module              |
    /header               |
      /header.html        |
      /header.js          |
      /header.css         |
      /logo.png           |
    /tab                  |
    /footer               |
    /banner               |
/pages                    |
 /index                   |
 /list                    |
 /info                    |
README.md                 | 项目说明文件

## 





