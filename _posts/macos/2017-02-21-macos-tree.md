---
layout: post
title: macos - tree目录列表
category: macos
tags:
  - macos
---



# 安装

```
brew install tree
```

# 打印目录

- 过滤文件夹

```
tree -L 3 -I "node_modules"
```

- 只显示目录

```
tree d -L 3 -I "node_modules"
```

-- 排序目录优先

```
tree -L 3 -I "node_modules" --dirsfirst
```

-- 所有文件目录

```
tree -L 3 -I "node_modules" --dirsfirst -a
```

-- 支持中文

```
tree -N
```

# 帮助

```
tree --help
```



