---
layout: post
title: react native - xcode-ios-编译发布安装
category: react-native
tags:
  - RN
---

# 编译发布安装

## 1. 准备证书请求文件 `certSigningRequest`

- 钥匙串管理 => 证书助理 => 从证书颁发机构请求证书...

- 证书信息
    - 输入 电子邮件
    - 输入 常用名称（可以是苹果的账号名字）
    - 选择存储到磁盘

## 2. 登录开发者账号

https://developer.apple.com/account

## 3. 创建证书 `iOS Certificates`

https://developer.apple.com/account/ios/certificate/

- 推荐分别创建 开发证书 & 发布证书

## 4. 创建引用 ID `iOS App IDs`

https://developer.apple.com/account/ios/identifier/bundle

ID规则 com.myco.app 和域名很像，倒着写

## 5. 登记你的设备 `UDID`

https://developer.apple.com/account/ios/device/

可以通过 itunes => 我的设备 => 摘要 => udid（通过右键切换）

## 6. 创建部署配置 `iOS Provisioning Profiles`

https://developer.apple.com/account/ios/profile/

- 推荐分别创建 开发 & 发布 部署文件

## 7. xcode 下载配置文件

xcode => Preferences => Accounts => 添加 Apple IDS => Download All Profiles（全部下载） 或者 Manage Certificates（单独下载）

## 8. xcode 设置签名

Targets => General => Signing

默认是自动的，如果编译出错，请选择手动配置

## 9. xcode 设置发布选项

Product => Scheme => Edit Scheme => Run => Build Configuration => Release

## 10. xcode 编译发布文件

Product => Archive

## 11. xcode 发布adHoc 本地最后测试

Windows => Organizer => iOS Apps => Export

## 12. xcode 发布到 app store

Windows => Organizer => iOS Apps => Upload to App Store

