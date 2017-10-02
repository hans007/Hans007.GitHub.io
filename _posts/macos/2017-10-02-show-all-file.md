---
layout: post
title: macos - Finder 显示所有隐藏文件
category: macos
tags:
  - macos
---

## 切换配置

```
defaults write com.apple.finder AppleShowAllFiles -bool true
```

> 将 true 改成 false, 就可恢复隐藏状态。

## 重启Finder

```
KillAll Finder
```

