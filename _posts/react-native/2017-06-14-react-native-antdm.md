---
layout: post
title: react native - 配置RN环境+antdm蚂蚁金服界面库
category: react-native
tags:
  - Ant-Design-Mobile
---

# 配置RN环境+antdm蚂蚁金服界面库

本机都是在macos下操作

## 安装包

```sh
brew install node
brew install watchman
brew install flow

npm install -g yarn
npm install -g react-native-cli
```

> 如果没有 Homebrew
>
> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
>

## 配置 `yarn` `npm` 国内镜像

参考: https://npm.taobao.org/

- npm

```sh
npm --registry https://registry.npm.taobao.org info underscore
```

- yarn

```sh
yarn config set registry https://registry.npm.taobao.org
```

## 创建项目

```sh
react-native init myapp
```

过程漫长，请先 `科学上网`

## 运行

```sh
react-native run-ios
```

第一次运行，自动配置项目环境，进行编译，时间会有点长，正常将看到 `ios模拟器`` 启动程序

`Cmd+D` 调出 `调试面板`

## 加入蚂蚁金服界面库

- 安装包

```sh
yarn add antd-mobile
yarn add babel-plugin-import -dev
```

- 修改 `.babelrc`

参考: https://github.com/ant-design/babel-plugin-import

```json
{
  "presets": ["react-native"],
  "plugins": [["import", { "libraryName": "antd-mobile", "style": true }]]
}
```

- 修改 `index.ios.js`

```js
import React, { Component } from 'react';
import { AppRegistry } from 'react-native';
import { Button } from 'antd-mobile';

class HelloWorldApp extends Component {
  render() {
    return <Button>Start</Button>;
  }
}

AppRegistry.registerComponent('HelloWorldApp', () => HelloWorldApp);
```

在模拟器中 `Cmd+R` 重新载入程序
