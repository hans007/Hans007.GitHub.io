---
layout: post
title: 前端自动化构建工具 - gulp - 03 - js模块化 - commandjs
category: 前端自动化构建工具
tags: 
  - 前端
  - 构建工具
  - gulp
---


# es6转化

## 安装 gulp-babel

```bash
npm install --save-dev gulp-babel babel-preset-es2015
```

## 配置 gulpfile.js

```javascript
const gulp = require('gulp');
const babel = require('gulp-babel');
gulp.task('default', function() {
  return gulp.src('pages/index/index.js')
        .pipe(babel({
            presets: ['es2015']
        }))
        .pipe(gulp.dest('release/dist/js'));
});
```

# 依赖加载

## 安装 browserify

```bash
npm install --global browserify
```

## 配置1 gulpfile.js

```javascript
glup.taks('browserify', function() {
  browserify({
     //先处理依赖，入口文件
     entries: ['./foo.js','./main.js'],
     //进行转化
     transform: []
  })
   .bundle() // 多个文件打包成一个文件
   .pipe(source()) // browserify的输出不能直接用做gulp输入，所以需要source进行处理 
   .pipe(gulp.dest('./'));  
})
```

## 配置2 gulpfile.js

```javascript
gulp.task('browserify',()=> {
    browserify({
        entries: ['src/js/main.js','src/js/foo.js'],
        debug: true, // 告知Browserify在运行同时生成内联sourcemap用于调试
    })
        .transform("babelify", {presets: ["es2015"]})
        .bundle()
        .pipe(source('bundle.js'))
        .pipe(buffer()) // 缓存文件内容
        .pipe(sourcemaps.init({loadMaps: true})) // 从 browserify 文件载入 map
        .pipe(sourcemaps.write('.')) // 写入 .map 文件
        .pipe(gulp.dest('dist/js'))
        .pipe(notify({ message: 'browserify task complete' }));
})
```

# 参考文

## npm
> https://www.npmjs.com/package/gulp-babel/
> https://github.com/floridoo/gulp-sourcemaps

## es6语法 - 阮一峰 - ECMAScript 6
> http://es6.ruanyifeng.com/

## gulp模块化实践
> https://github.com/chenbin92/ES6-with-gulp-build

