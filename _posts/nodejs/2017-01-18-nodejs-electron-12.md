---
layout: post
title: nodejs - 第十二节: 用electron发布成独立程序
category: nodejs
tags:
  - nodejs
  - electron
---

# 前言

在nodejs的帮助下，我们可以用 javascript+html+css 创建出丰富的客户端

在某些情况下，希望自己写的程序能够独立运行，而不是打开浏览器的方式

一次开发在多个平台上运行 macos linux windows

## 安装electron环境

- 官网

http://electron.atom.io/

- 我们这里安装全局方式

```
npm install -g electron
```

## 创建一个helloword项目

- 创建

```
mkdir ele-app-demo
cd ele-app-demo
npm init
npm install electron --save
```

- 项目结构

```
\package.json
\index.html
\main.js
```

- 配置文件package.json

```json
{
  "name": "ele-app-demo",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "electron": "^1.4.1"
  }
}
```

> main 启动文件
> scripts->start 启动的脚本

- 主程序 main.js

```javascript
const {app, BrowserWindow} = require('electron')
const path = require('path')
const url = require('url')

// 全局窗口对象
let win

function createWindow () {

  // 创建浏览器窗口对象
  win = new BrowserWindow({width: 800, height: 600})

  // 读取index.html 为首页
  win.loadURL(url.format({
    pathname: path.join(__dirname, 'index.html'),
    protocol: 'file:',
    slashes: true
  }))

  // 打开调试工具，发布的时候请关闭
  win.webContents.openDevTools()

  // 关闭 - 消息处理
  win.on('closed', () => {
    
    // 关闭不必要的资源占用（内存、流）
    win = null
  })
}

// 首次启动环境加载完成 - 消息处理
app.on('ready', createWindow)

// 当所有的窗口被关闭 - 消息处理
app.on('window-all-closed', () => {
  // On macOS it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

// 激活窗口 - 消息处理
app.on('activate', () => {
  // On macOS it's common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (win === null) {
    createWindow()
  }
})

```

- 界面 index.html

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using node <script>document.write(process.versions.node)</script>,
    Chrome <script>document.write(process.versions.chrome)</script>,
    and Electron <script>document.write(process.versions.electron)</script>.
  </body>
</html>
```

## 运行

```
npm run

或者

electron .
```

## 打包发布 electron-packager 方式

- 官网

https://github.com/electron-userland/electron-packager

- 安装

```
项目内
npm install electron-packager --save-dev

全局使用
npm install electron-packager -g
```

- 命令格式

```
electron-packager <sourcedir> <appname> --platform=<platform> --arch=<arch> [optional flags...]
```

- 文档参考

https://github.com/electron-userland/electron-packager/blob/master/usage.txt

- 发布成3平台

```
electron-packager ./ --all
electron-packager ./ --platform=win32 --arch=x64 --overwrite
electron-packager ./ --platform=darwin --arch=x64 --overwrite

成功后项目根目录下有3个平台的文件夹
xxx-darwin-x64
xxx-linux-ia32
xxx-window-x64
```

## Electron API 文档

https://github.com/electron/electron-api-demos/

## 坑1 - node install.js 卡卡卡

```
vi ~/.npmrc
registry = https://registry.npm.taobao.org
sass_binary_site = https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl = http://npm.taobao.org/mirrors/phantomjs/
electron_mirror = http://npm.taobao.org/mirrors/electron/
```

## 坑2 - 发布windows下的包需要 wine

- 文档参考

> https://www.davidbaumgold.com/tutorials/wine-mac/
> https://github.com/electron-userland/electron-packager#building-windows-apps-from-non-windows-platforms
> https://www.winehq.org/announce/1.8.6

- Homebrew 方式安装
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor
sudo xcodebuild -license

brew tap caskroom/cask
brew cask install java xquartz
brew install wine
==> Installing dependencies for wine: jpeg, pkg-config, libtool, libusb, libusb-compat, fontconfig, libtiff, webp, gd, libgphoto2, little-cms2, cmake, jasper, libicns, makedepend, openssl, net-snmp, sane-backends, libtasn1, gmp, nettle, libunistring, libffi, p11-kit, gnutls
很多wine的依赖包都是国外的，耐心点下载吧
```


[我的博客](https://hans007.github.io)