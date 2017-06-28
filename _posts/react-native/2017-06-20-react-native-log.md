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

<LazyloadScrollView name="lazyload-list" style={indexStyle.container}>
<LazyloadImage
                        host="lazyload-list"
                        style={indexStyle.rowImage}
                        source={{
                        uri: rowData.item.icon
                    }}/>
</LazyloadScrollView>

## remote debugger is in a background tab ...

chrome调试器，单独页打开不要跑到后台，就不会出现这个烦人的提示了~

## 








