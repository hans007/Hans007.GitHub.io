---
layout: post
title: react native - 保存图片到相册
category: react-native
tags:
  - RN
---

# 组件

官方组件 [CameraRoll](https://facebook.github.io/react-native/docs/cameraroll.html)

## 安装

- RCTCameraRoll 库。进入到工程项目中的 node_module/react-native/Libraries/CameraRoll
- 把 RCTCameraRoll.xcodeproj 添加到在项目工程的 Liberaries 文件夹下
- 在 Build Phases -> Link Binary With Libraries 里添加 libRCTCameraRoll.a
- 由于苹果安全策略更新，还需要在 Info.plist 配置请求照片相的关描述字段（Privacy - Photo Library Usage Description）

## 代码

```js
    //保存图片
    saveImg(img) {
      var promise = CameraRoll.saveToCameraRoll(img);
      promise.then(function(result) {
        alert('保存成功！地址如下：\n' + result);
      }).catch(function(error) {
        alert('保存失败！\n' + error);
      });
    }
```





