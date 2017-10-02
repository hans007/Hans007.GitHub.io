---
layout: post
title: react native - 踩坑笔记
category: react-native
tags:
  - 踩坑
---

# 当前环境

- RN 0.45

## 非 `https` 访问

在 `Info.plist` 中添加 `NSAppTransportSecurity` 类型 `Dictionary` 。
在 `NSAppTransportSecurity` 下添加 `NSAllowsArbitraryLoads` 类型 `Boolean` , 值设为 `YES`

添加例外的方式也很简单：
左键 `Info.plist` 选择 open with source code
然后添加类似如下的配置:

```js
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>qq.com</key>
        <dict>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
        <key>sina.com.cn</key>
        <dict>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
        </dict>
</dict>
```

## Text 换行问题

1. 设置属性

    <Text numberOfLines={3} style={indexStyle.rowTitle}>{rowData.item.title}</Text>

2. 计算宽度

    rowTitle: {
        width: Dimen.window.width - 100,
        margin: 5,
        fontSize: 12
    },

## 图片懒加载 `react-native-lazyload`

https://github.com/magicismight/react-native-lazyload

yarn add react-native-lazyload

```js
<LazyloadScrollView name="lazyload-list" style={indexStyle.container}>
<LazyloadImage
                        host="lazyload-list"
                        style={indexStyle.rowImage}
                        source={{
                        uri: rowData.item.icon
                    }}/>
</LazyloadScrollView>
```

## remote debugger is in a background tab ...

chrome调试器，单独页打开不要跑到后台，就不会出现这个烦人的提示了~

## clone对象

```js
import _ from 'lodash'

const obj = {
    a:1,
    b:2
}
const newObj = _.cloneDeep(obj);
```

## 对两个对象进行验证

```js
JSON.stringify(obj) == JSON.stringify(newObj)   //true
obj == newObj                                   //false
```

## 本地缓存

```js
import { AsyncStorage } from 'react-native';

// 设置
let storageKey = `index2:toy:${params.toyType}`;
AsyncStorage.setItem(storageKey, JSON.stringify(dataList), 
    err => {
        if(!err){
            console.log(`AsyncStorage set ok => ${storageKey}`);
        }
    }
);

// 读取
let storageKey = `index2:toy:${toyType}`;
AsyncStorage.getItem(storageKey).then((value) => {
    let jsonValue = JSON.parse(value);
    if(jsonValue!=null){
        console.log(`AsyncStorage get ok => ${storageKey}`);
        dispatch(setupData(jsonValue, params));
    }
});
```

## import 特点

默认文件 `index.js`

## 判断android还是ios

var Platform = require('Platform');
if (Platform.OS === 'android') {
    //todo  
}else{  
    //todo  
}  

## Toast 在Modal中使用 被覆盖

https://github.com/ant-design/ant-design-mobile/issues/547

## 本机调试 android

- 查看设备 `adb devices`
- 安装 `react-native run-android`
- 安装发布 `react-native run-android --variant=release`
- [签名](http://reactnative.cn/docs/0.46/signed-apk-android.html)
- 打开开发者菜单 `adb shell input keyevent 82`
- `Dev Settings` -> `Debug server host for device` -> `本机IP:8081` -> `Reload JS`
- 如果配置完还是白屏 ，重新安装 `react-native run-android`




