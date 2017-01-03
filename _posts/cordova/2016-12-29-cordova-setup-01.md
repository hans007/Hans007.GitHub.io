---
layout: post
title: cordova - 第一节:安装&运行&编译
category: cordova
tags:
  - cordova
---


# 安装

## 1. 需要Node.js环境

安装地址
https://nodejs.org/en/download/

## 2. 推荐安装git环境

安装地址
https://git-scm.com/downloads

## 3. 安装cordova cli环境

命令输入

```
sudo npm install -g cordova
```

## 4. 安装完成后检查当前版本

命令输入

```
npm info cordova version
```

输出

```
6.4.0
```

> 如果上面步骤正确，就会显示当前版本号

## 5. 升级cordova cli环境

命令输入

```
sudo npm update -g cordova
```

# 环境配置

## 检查环境

命令输入

```
$ cordova requirements
```

输出

```
Requirements check results for android:
Java JDK: installed 1.8.0
Android SDK: installed true
Android target: not installed
Please install Android target: "android-24".

Hint: Open the SDK manager by running: /Users/hans/Documents/Eclipse/sdk/tools/android
You will require:
1. "SDK Platform" for android-24
2. "Android SDK Platform-tools (latest)
3. "Android SDK Build-tools" (latest)
Gradle: installed

Requirements check results for ios:
Apple OS X: installed darwin
Xcode: installed 8.1
ios-deploy: not installed
ios-deploy was not found. Please download, build and install version 1.9.0 or greater from https://github.com/phonegap/ios-deploy into your path, or do 'npm install -g ios-deploy'
CocoaPods: installed
Error: Some of requirements check failed
```

> 会检查 android 和 ios 环境配置
> 检查项目包含 SDK 开发IDE 工具包
> 同时也会输出处理建议

## android 环境配置

### 1. 安装 Java Development Kit (JDK)

安装地址
http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html

### 2. 安装 Android SDK

安装地址
https://developer.android.com/studio/index.html

- 方案1
安装完整版 Android Studio , 包含 Android SDK

> Windows
> https://dl.google.com/dl/android/studio/install/2.2.3.0/android-studio-bundle-145.3537739-windows.exe

> Mac OS X
> https://dl.google.com/dl/android/studio/install/2.2.3.0/android-studio-ide-145.3537739-mac.dmg

- 方案2
只安装SDK工具包，不推荐

> Windows
> https://dl.google.com/android/repository/tools_r25.2.3-windows.zip

> Mac
> https://dl.google.com/android/repository/tools_r25.2.3-macosx.zip

### 3. 安装SDK对应的包

官方问阅读
https://developer.android.com/studio/intro/update.html

- 打开 SDK Manager
要打开 SDK 管理器，请点击 Tools > Android > SDK Manager 或点击工具栏中的 SDK Manager

- SDK对应

cordova-android Version | Supported Android API-Levels
------------------------|--------------------------------
5.X.X	                | 14 - 23
4.1.X	                | 14 - 22
4.0.X	                | 10 - 22
3.7.X	                | 10 - 21

> 当前6.4.0 那对应的包就要 >= 24

### 4. 设置环境变量

设置 JAVA_HOME , ANDROID_HOME

- OS X and Linux

```
vi ~/.bash_profile

export ANDROID_HOME=/Users/hans/Documents/Eclipse/sdk/
export PATH=${PATH}:$ANDROID_HOME/platform-tools
export PATH=${PATH}:$ANDROID_HOME/tools

source ~/.bash_profile
```

- Windows

开始菜单->环境变量->编辑

```
设置PATH
C:\Development\android-sdk\platform-tools
C:\Development\android-sdk\tools
```

## ios 环境配置

安装最新的苹果IDE , Xcode， 一步步 往下走自动配置的

安装地址
https://itunes.apple.com/cn/app/xcode/id497799835?mt=12

# 创建项目

## 1. 创建 myMobileApp

命令输入

```
cordova create myMobileApp com.myapp.mobile myMobileApp
```

输出

```
Using detached cordova-create
Creating a new cordova project.

$ ls -l

total 8
-rw-r--r--  1 hans  staff  984 12 29 11:09 config.xml
drwxr-xr-x  3 hans  staff  102 12 29 11:09 hooks
drwxr-xr-x  2 hans  staff   68 12 29 11:09 platforms
drwxr-xr-x  2 hans  staff   68 12 29 11:09 plugins
drwxr-xr-x  6 hans  staff  204 12 29 11:09 www
```

> cordova 目录名称|id标识|项目名称

## 2. 创建平台项目代码

- IOS

命令输入

```
cordova platform add ios --save
```

输出

```
Adding ios project...
Creating Cordova project for the iOS platform:
	Path: platforms/ios
	Package: com.myapp.mobile
	Name: myMobileApp
iOS project created with cordova-ios@4.3.1
Discovered plugin "cordova-plugin-whitelist" in config.xml. Adding it to the project
Fetching plugin "cordova-plugin-whitelist@1" via npm
Installing "cordova-plugin-whitelist" for ios
Saved plugin info for "cordova-plugin-whitelist" to config.xml
--save flag or autosave detected
Saving ios@~4.3.1 into config.xml file ...
```

- android

命令输入

```
cordova platform add android --save
```

输出

```

```

## 3. 更新平台代码

```
cordova platform update android --save
cordova platform update ios --save
```

# 编译

## IOS

![](http://7xosx4.com1.z0.glb.clouddn.com/xcode-run.png)

> xcode 直接打开文件 /myMobileApp/platforms/ios/myMobileApp.xcworkspace
> xcode 集成度很好 ，直接点击run， easy完成

## Android

![](http://7xosx4.com1.z0.glb.clouddn.com/android-studio-run.png)

> 用 android studio 打开 /myMobileApp/platforms/android
> 模拟器用 Genymotion , 自带的模拟器太慢
> https://www.genymotion.com/



[我的博客](https://hans007.github.io)