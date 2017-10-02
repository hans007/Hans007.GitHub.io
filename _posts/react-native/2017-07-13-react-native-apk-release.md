---
layout: post
title: react native - android-apk-编译发布安装
category: react-native
tags:
  - RN
---

# APK编译发布安装

## 1. 生成key

```
mdidr /keystores/apk
cd /keystores/apk
keytool -genkey -v -keystore my-release-key.keystore -alias jpfigure-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

## 2. 修改 `/android/app/build.gradle`

```js
// key store
MYAPP_RELEASE_STORE_FILE    =   "../../keystores/apk/my-release-key.keystore"
MYAPP_RELEASE_KEY_ALIAS     =   "jpfigure-key-alias"
MYAPP_RELEASE_STORE_PASSWORD=   "0p9o8i7u"
MYAPP_RELEASE_KEY_PASSWORD  =   "0p9o8i7u"

android {

    defaultConfig {
    }
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
```

## 3. 编译

cd android && ./gradlew assembleRelease

## 4. 编译并安装到手机

cd android && ./gradlew installRelease

## 错误: `Couldn't follow symbolic link.`

- 错误信息

```
Could not list contents of '/Users/lise/WorkSpace/idlebox/node_modules/react-native/third-party/glog-0.3.4/test-driver'. Couldn't follow symbolic link.
```

- 方案, unlink

```
unlink node_modules/react-native/third-party/glog-0.3.4/test-driver
```



