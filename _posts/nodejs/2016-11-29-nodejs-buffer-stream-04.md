---
layout: post
title: nodejs - 第四节:Buffer&Stream
category: nodejs
tags:
  - nodejs
---

# Buffer

## 创建 Buffer

```javascript
var buf1 = new Buffer(10);
var buf2 = new Buffer([10, 20, 30, 40, 50]);
var buf3 = new Buffer("京东(JD.COM)-综合网购首选-正品低价、品质保障、配送及时、轻松购物！", "utf-8");
```

> utf-8 是默认的编码方式，此外它同样支持以下编码："ascii", "utf8", "utf16le", "ucs2", "base64" 和 "hex"。

## 写入缓冲区 

语法

```javascript
buf.write(string[, offset[, length]][, encoding])
```

```javascript
buf3 = new Buffer(256);
len = buf3.write("京东(JD.COM)-综合网购首选-");

console.log("写入字节数 : "+  len);
```

## 从缓冲区读取数据

语法

```javascript
buf.toString([encoding[, start[, end]]])
```

```javascript
console.log(buf3.toString('utf-8',0,6));
```

## 将 Buffer 转换为 JSON 对象 

```javascript
var json = buf3.toJSON();
console.log("转换为 JSON 对象 : "+json);
```

## 缓冲区合并

语法

```javascript
Buffer.concat(list[, totalLength])
```

```javascript
var buffer1 = new Buffer('京东(JD.COM)-综合网购首选-');
var buffer2 = new Buffer('正品低价、品质保障、配送及时、轻松购物！');
var buffer3 = Buffer.concat([buffer1,buffer2]);
console.log("buffer3 内容: " + buffer3.toString());
```

## 缓冲区比较

语法

```javascript
buf.compare(otherBuffer);
```

> 返回一个数字，表示 buf 在 otherBuffer 之前，之后或相同。

```javascript
var buffer1 = new Buffer('ABC');
var buffer2 = new Buffer('ABCD');
var result = buffer1.compare(buffer2);

if(result < 0) {
   console.log(buffer1 + " 在 " + buffer2 + "之前");
}else if(result == 0){
   console.log(buffer1 + " 与 " + buffer2 + "相同");
}else {
   console.log(buffer1 + " 在 " + buffer2 + "之后");
}
```

## 拷贝缓冲区

语法

```javascript
buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])
```

```javascript
var buffer1 = new Buffer('ABC');
var buffer2 = new Buffer(3);
buffer1.copy(buffer2);
console.log("拷贝缓冲区 : " + buffer2.toString());
```

## 缓冲区裁剪

语法

```javascript
buf.slice([start[, end]])
```

```javascript
var buffer1 = new Buffer('京东(JD.COM)-综合网购首选-');
var buffer2 = buffer1.slice(0,6);
console.log("缓冲区裁剪 : " + buffer2.toString());
```

## 缓冲区长度

```javascript
console.log("缓冲区长度 : " + buffer1.length);
```

## 参考手册

> https://nodejs.org/api/buffer.html

# Stream

## 从流中读取数据

```javascript
var fs = require("fs");
var data = '';

// 创建可读流
var readerStream = fs.createReadStream('input.txt');

// 设置编码为 utf8。
readerStream.setEncoding('UTF8');

// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
   data += chunk;
});

readerStream.on('end',function(){
   console.log(data);
});

readerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```

## 写入流

```javascript
var fs = require("fs");
var data = '京东(JD.COM)-综合网购首选-正品低价、品质保障、配送及时、轻松购物！';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```

## 管道流

```javascript
var fs = require("fs");

// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');

// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);

console.log("程序执行完毕");
```

## 链式流 

- 压缩

```javascript
var fs = require("fs");
var zlib = require('zlib');

// 压缩 input.txt 文件为 input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));

console.log("文件压缩完成。");
```

- 解压缩

```javascript
var fs = require("fs");
var zlib = require('zlib');

// 解压 input.txt.gz 文件为 input.txt
fs.createReadStream('input.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('input.txt'));

console.log("文件解压完成。");
```

## 参考手册

> https://nodejs.org/api/stream.html

# 调试工具

> npm install -g devtool

```
$ devtool main.js
```

> 参考
> http://www.open-open.com/lib/view/open1456741326062.html

# 代码

> https://github.com/hans007/JavaScriptCodes/tree/master/nodejs-do

[我的博客](https://hans007.github.io)