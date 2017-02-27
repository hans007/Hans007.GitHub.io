---
layout: post
title: cordova - 第二节 创建自己的内容&使用插件
category: cordova
tags:
  - cordova
---

# 编辑内容

## 1. 修改内容

/www 目录就是我们的内容目录

可以把你的网站放入

## 2. 修改配置文件

/config.xml 是配置的核心文件

<content src="index.html" />

这里配置你的启动文件

## 3. 同步到目标平台 android & IOS

执行命令 cordova prepare android

/www/ 文件会复制到 /platforms/android/assets/www

## 4. 编译

这里还是推荐用 Android Studio 打开目录 /platforms/android/

# 插件扩展

cordova plugins 可以让你的程序访问设备端功能

## 下载地址

https://www.npmjs.com/search?q=cordova-plugin

目前已经有 2268个 开放包

## 访问设备的电量

- 下载插件

cordova plugin add cordova-plugin-battery-status

- 编写代码

打开文件 /www/index.html

```javascript
window.addEventListener("batterystatus", onBatteryStatus, false);

function onBatteryStatus(status) {
    var str = "Level: " + status.level + " isPlugged: " + status.isPlugged;
    $("#status01").html(str);
}
```

> 然后运行 cordova prepare android
> Android Studio 运行到模拟器/真机
> 很easy




[我的博客](https://hans007.github.io)