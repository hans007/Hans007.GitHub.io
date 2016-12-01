---
layout: post
title: nodejs学习资料 - 第七节:文件系统&工具模块
category: nodejs学习资料
tags:
  - nodejs
---

# 文件系统

> https://nodejs.org/api/fs.html

## 异步和同步

> 方法名带有 Sync 的都是同步方式
> 建议大家是用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。

## open 打开文件

- 格式

> fs.open(path, flags[, mode], callback)

- 参数

> path - 文件的路径。
> flags - 文件打开的行为。具体值详见下文。
> mode - 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)。
> callback - 回调函数，带有两个参数如：callback(err, fd)。

- flags 参数可以是以下值

Flag | 描述
-----|--------
r	 | 以读取模式打开文件。如果文件不存在抛出异常。
r+	 | 以读写模式打开文件。如果文件不存在抛出异常。
rs	 | 以同步的方式读取文件。
rs+	 | 以同步的方式读取和写入文件。
w	 | 以写入模式打开文件，如果文件不存在则创建。
wx	 | 类似 'w'，但是如果文件路径存在，则文件写入失败。
w+	 | 以读写模式打开文件，如果文件不存在则创建。
wx+	 | 类似 'w+'， 但是如果文件路径存在，则文件读写失败。
a	 | 以追加模式打开文件，如果文件不存在则创建。
ax	 | 类似 'a'， 但是如果文件路径存在，则文件追加失败。
a+	 | 以读取追加模式打开文件，如果文件不存在则创建。
ax+	 | 类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。

> 带x的都是如果文件存在就失败
> 带+的都是写操作
> 带s的都是同步操作

- 代码

```javascript
var fs = require("fs");

// 异步打开文件
console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
  console.log("文件打开成功！");     
});
```

- 运行&输出

```javascript
$ node open.js
准备打开文件！
文件打开成功！
```

## stat 获取文件信息

- 格式

> fs.stat(path, callback)

- 参数

> path - 文件路径。
> callback - 回调函数，带有两个参数如：(err, stats), stats 是 fs.Stats 对象。

- stats类中的方法有

方法 | 描述
----|-------
stats.isFile()	           | 如果是文件返回 true，否则返回 false。
stats.isDirectory()	       | 如果是目录返回 true，否则返回 false。
stats.isCharacterDevice()  | 如果是字符设备返回 true，否则返回 false。
stats.isSymbolicLink()	   | 如果是软链接返回 true，否则返回 false。
stats.isFIFO()	           | 如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。
stats.isSocket()	       | 如果是 Socket 返回 true，否则返回 false。

- 代码

```javascript
var fs = require("fs");

console.log("准备打开文件！");
fs.stat('input.txt', function (err, stats) {
   if (err) {
       return console.error(err);
   }
   console.log(stats);
   console.log("读取文件信息成功！");

   // 检测文件类型
   console.log("是否为文件(isFile) ? " + stats.isFile());
   console.log("是否为目录(isDirectory) ? " + stats.isDirectory());    
});
```

- 运行&输出

```javascript
$ node stat.js
准备打开文件！
{ dev: 16777220,
  mode: 33188,
  nlink: 1,
  uid: 501,
  gid: 20,
  rdev: 0,
  blksize: 4096,
  ino: 11045667,
  size: 94,
  blocks: 8,
  atime: Wed Nov 30 2016 15:50:28 GMT+0800 (CST),
  mtime: Wed Nov 30 2016 15:50:28 GMT+0800 (CST),
  ctime: Wed Nov 30 2016 15:50:28 GMT+0800 (CST),
  birthtime: Wed Nov 30 2016 15:50:13 GMT+0800 (CST) }
读取文件信息成功！
是否为文件(isFile) ? true
是否为目录(isDirectory) ? false
```

## writeFile 写入文件

- 格式

> fs.writeFile(filename, data[, options], callback)

- 参数

> path - 文件路径。
> data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(流) 对象。
> options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
> callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。

- 代码

```javascript
var fs = require("fs");

console.log("准备写入文件");
fs.writeFile('input.txt', '京东(JD.COM)-综合网购首选-',  function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("数据写入成功！");
   console.log("--------我是分割线-------------")
   console.log("读取写入的数据！");
   fs.readFile('input.txt', function (err, data) {
      if (err) {
         return console.error(err);
      }
      console.log("异步读取文件数据: " + data.toString());
   });
});
```

- 运行&输出

```javascript
$ node writeFile.js
准备写入文件
数据写入成功！
--------我是分割线-------------
读取写入的数据！
异步读取文件数据: 京东(JD.COM)-综合网购首选-
```

## read 读取文件

- 格式

> fs.read(fd, buffer, offset, length, position, callback)

- 参数

> fd - 通过 fs.open() 方法返回的文件描述符。
> buffer - 数据写入的缓冲区。
> offset - 缓冲区写入的写入偏移量。
> length - 要从文件中读取的字节数。
> position - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。
> callback - 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。

- 代码

```javascript
var fs = require("fs");
var buf = new Buffer(1024);

console.log("准备打开已存在的文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件：");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }
      console.log(bytes + "  字节被读取");

      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }
   });
});
```

- 运行&输出

```javascript
$ node read.js
准备打开已存在的文件！
文件打开成功！
准备读取文件：
34  字节被读取
京东(JD.COM)-综合网购首选-
```

## 关闭文件

- 格式

> fs.close(fd, callback)

- 参数

> fd - 通过 fs.open() 方法返回的文件描述符。
> callback - 回调函数，没有参数。

- 代码

```javascript
var fs = require("fs");
var buf = new Buffer(1024);

console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件！");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }

      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }

      // 关闭文件
      fs.close(fd, function(err){
         if (err){
            console.log(err);
         } 
         console.log("文件关闭成功");
      });
   });
});
```

- 运行&输出

```javascript
$ node close.js
准备打开文件！
文件打开成功！
准备读取文件！
京东(JD.COM)-综合网购首选-
文件关闭成功
```

## 截取文件

- 格式

> fs.ftruncate(fd, len, callback)

- 参数

> fd - 通过 fs.open() 方法返回的文件描述符。
> len - 文件内容截取的长度。
> callback - 回调函数，没有参数。

- 代码

```javascript
var fs = require("fs");
var buf = new Buffer(1024);

console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("截取10字节后的文件内容。");

   // 截取文件
   fs.ftruncate(fd, 10, function(err){
      if (err){
         console.log(err);
      } 
      console.log("文件截取成功。");
      console.log("读取相同的文件"); 
      fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
         if (err){
            console.log(err);
         }

         // 仅输出读取的字节
         if(bytes > 0){
            console.log(buf.slice(0, bytes).toString());
         }

         // 关闭文件
         fs.close(fd, function(err){
            if (err){
               console.log(err);
            } 
            console.log("文件关闭成功！");
         });
      });
   });
});
```

- 运行&输出

```javascript
$ node ftruncate.js
准备打开文件！
文件打开成功！
截取10字节后的文件内容。
文件截取成功。
读取相同的文件
京东(JD.
文件关闭成功！
```

## 删除文件

- 格式

> fs.unlink(path, callback)

- 参数

> path - 文件路径。
> callback - 回调函数，没有参数。

- 代码

```javascript
var fs = require("fs");

console.log("准备删除文件！");
fs.unlink('output.txt', function (err) {
    if (err) {
        return console.error(err);
    }
    console.log("文件删除成功！");
});
```

- 运行&输出

```javascript
$ node file.js 
准备删除文件！
文件删除成功！
```

## 创建目录

- 格式

> fs.mkdir(path[, mode], callback)

- 参数

> path - 文件路径。
> mode - 设置目录权限，默认为 0777。
> callback - 回调函数，没有参数。

- 代码

```javascript
var fs = require("fs");

console.log("创建目录 ./test/");
fs.mkdir("./test/",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("目录创建成功。");
});
```

- 运行&输出

```javascript
$ node mkdir.js
创建目录 ./test/
目录创建成功。
```

## 读取目录

- 格式

> fs.readdir(path, callback)

- 参数

> path - 文件路径。
> callback - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

- 代码

```javascript
var fs = require("fs");

console.log("查看 ./ 目录");
fs.readdir("./", function (err, files) {
    if (err) {
        return console.error(err);
    }
    files.forEach(function (file) {
        console.log(file);
    });
});
```

- 运行&输出

```javascript
$ node readdir.js
查看 ./ 目录
close.js
ftruncate.js
input.txt
mkdir.js
open.js
read.js
readdir.js
stat.js
unlink.js
writeFile.js
```

## 删除目录

- 格式

> fs.rmdir(path, callback)

- 参数

> path - 文件路径。
> callback - 回调函数，没有参数。

- 代码

```javascript
var fs = require("fs");

console.log("准备删除目录 ./test");
fs.rmdir("./test", function (err) {
    if (err) {
        return console.error(err);
    }
});
```

- 运行&输出

```javascript
$ node rmdir.js
准备删除目录 ./test
```

## 文件模块方法参考手册

方法 | 描述
----------------------------------------------------------------|----------------------------------------------
fs.rename(oldPath, newPath, callback)	                        | 异步 rename().回调函数没有参数，但可能抛出异常。
fs.ftruncate(fd, len, callback)	                                | 异步 ftruncate().回调函数没有参数，但可能抛出异常。
fs.ftruncateSync(fd, len)	                                    | 同步 ftruncate()
fs.truncate(path, len, callback)	                            | 异步 truncate().回调函数没有参数，但可能抛出异常。
fs.truncateSync(path, len)	                                    | 同步 truncate()
fs.chown(path, uid, gid, callback)	                            | 异步 chown().回调函数没有参数，但可能抛出异常。
fs.chownSync(path, uid, gid)	                                | 同步 chown()
fs.fchown(fd, uid, gid, callback)	                            | 异步 fchown().回调函数没有参数，但可能抛出异常。
fs.fchownSync(fd, uid, gid)	                                    | 同步 fchown()
fs.lchown(path, uid, gid, callback)	                            | 异步 lchown().回调函数没有参数，但可能抛出异常。
fs.lchownSync(path, uid, gid)	                                | 同步 lchown()
fs.chmod(path, mode, callback)	                                | 异步 chmod().回调函数没有参数，但可能抛出异常。
fs.chmodSync(path, mode)	                                    | 同步 chmod().
fs.fchmod(fd, mode, callback)	| 异步 fchmod().回调函数没有参数，但可能抛出异常。
fs.fchmodSync(fd, mode)	| 同步 fchmod().
fs.lchmod(path, mode, callback)	| 异步 lchmod().回调函数没有参数，但可能抛出异常。Only available on Mac OS X.
fs.lchmodSync(path, mode)	| 同步 lchmod().
fs.stat(path, callback)	| 异步 stat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。
fs.lstat(path, callback)	| 异步 lstat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。
fs.fstat(fd, callback)	| 异步 fstat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。
fs.statSync(path)	| 同步 stat(). 返回 fs.Stats 的实例。
fs.lstatSync(path)	| 同步 lstat(). 返回 fs.Stats 的实例
fs.fstatSync(fd)	| 同步 fstat(). 返回 fs.Stats 的实例。
fs.link(srcpath, dstpath, callback)	| 异步 link().回调函数没有参数，但可能抛出异常。
fs.linkSync(srcpath, dstpath)	| 同步 link().
fs.symlink(srcpath, dstpath[, type], callback)	| 异步 symlink().回调函数没有参数，但可能抛出异常。 type 参数可以设置为 'dir', 'file', 或 'junction' (默认为 'file') 。
fs.symlinkSync(srcpath, dstpath[, type])	| 同步 symlink().
fs.readlink(path, callback)	| 异步 readlink(). 回调函数有两个参数 err, linkString。
fs.realpath(path[, cache], callback)	| 异步 realpath(). 回调函数有两个参数 err, resolvedPath。
fs.realpathSync(path[, cache])	| 同步 realpath()。返回绝对路径。
fs.unlink(path, callback)	| 异步 unlink().回调函数没有参数，但可能抛出异常。
fs.unlinkSync(path)	| 同步 unlink().
fs.rmdir(path, callback)	| 异步 rmdir().回调函数没有参数，但可能抛出异常。
fs.rmdirSync(path)	| 同步 rmdir().
fs.mkdir(path[, mode], callback)	| S异步 mkdir(2).回调函数没有参数，但可能抛出异常。 mode defaults to 0777.
fs.mkdirSync(path[, mode])	| 同步 mkdir().
fs.readdir(path, callback)	| 异步 readdir(3). 读取目录的内容。
fs.readdirSync(path)	| 同步 readdir().返回文件数组列表。
fs.close(fd, callback)	| 异步 close().回调函数没有参数，但可能抛出异常。
fs.closeSync(fd)	| 同步 close().
fs.open(path, flags[, mode], callback)	| 异步打开文件。
fs.openSync(path, flags[, mode])	| 同步 version of fs.open().
fs.utimes(path, atime, mtime, callback) |	
fs.utimesSync(path, atime, mtime)	|修改文件时间戳，文件通过指定的文件路径。
fs.futimes(fd, atime, mtime, callback)|	
fs.futimesSync(fd, atime, mtime)	|修改文件时间戳，通过文件描述符指定。
fs.fsync(fd, callback)	|异步 fsync.回调函数没有参数，但可能抛出异常。
fs.fsyncSync(fd)	|同步 fsync
fs.write(fd, buffer, offset, length[, position], callback)	|将缓冲区内容写入到通过文件描述符指定的文件。
fs.write(fd, data[, position[, encoding]], callback)	|通过文件描述符 fd 写入文件内容。
fs.writeSync(fd, buffer, offset, length[, position])	|同步版的 fs.write()。
fs.writeSync(fd, data[, position[, encoding]])	|同步版的 fs.write().
fs.read(fd, buffer, offset, length, position, callback)	|通过文件描述符 fd 读取文件内容。
fs.readSync(fd, buffer, offset, length, position)	|同步版的 fs.read.
fs.readFile(filename[, options], callback)	|异步读取文件内容。
fs.readFileSync(filename[, options])	|
fs.writeFile(filename, data[, options], callback)	|异步写入文件内容。
fs.writeFileSync(filename, data[, options])	|同步版的 fs.writeFile。
fs.appendFile(filename, data[, options], callback)	|异步追加文件内容。
fs.appendFileSync(filename, data[, options])	|The 同步 version of fs.appendFile.
fs.watchFile(filename[, options], listener)	|查看文件的修改
fs.unwatchFile(filename[, listener])	|停止查看 filename 的修改。
fs.watch(filename[, options][, listener])	|查看 filename 的修改，filename 可以是文件或目录。返回 fs.FSWatcher 对象。
fs.exists(path, callback)	|检测给定的路径是否存在。
fs.existsSync(path)	|同步版的 fs.exists.
fs.access(path[, mode], callback)	|测试指定路径用户权限。
fs.accessSync(path[, mode])	|同步版的 fs.access。
fs.createReadStream(path[, options])	|返回ReadStream 对象。
fs.createWriteStream(path[, options])	|返回 WriteStream 对象。
fs.symlink(srcpath, dstpath[, type], callback)	|异步 symlink().回调函数没有参数，但可能抛出异常。

# OS 模块

一些基本的系统操作函数

> https://nodejs.org/dist/latest-v7.x/docs/api/os.htm

- 方法

方法 | 描述
--------------------------|--------------------------------------
os.tmpdir()	              | 返回操作系统的默认临时文件夹。
os.endianness()	          | 返回 CPU 的字节序，可能的是 "BE" 或 "LE"。
os.hostname()	          | 返回操作系统的主机名。
os.type()	              | 返回操作系统名
os.platform()	          | 返回操作系统名
os.arch()	              | 返回操作系统 CPU 架构，可能的值有 "x64"、"arm" 和 "ia32"。
os.release()	          | 返回操作系统的发行版本。
os.uptime()	              | 返回操作系统运行的时间，以秒为单位。
os.loadavg()	          | 返回一个包含 1、5、15 分钟平均负载的数组。
os.totalmem()	          | 返回系统内存总量，单位为字节
os.freemem()	          | 返回操作系统空闲内存量，单位是字节。
os.cpus()	              | 返回一个对象数组，包含所安装的每个 CPU/内核的信息：型号、速度（单位 MHz）、时间（一个包含 user、nice、sys、idle 和 irq 所使用 CPU/内核毫秒数的对象）。
os.networkInterfaces()	  | 获得网络接口列表。

- 属性

属性 | 描述
---------|---------
os.EOL	 | 定义了操作系统的行尾符的常量。

# Path 模块

用于处理文件路径

> https://nodejs.org/dist/latest-v7.x/docs/api/path.html

- 方法

方法 | 描述
-----------------------------------|------------------------------------------
path.normalize(p)	|规范化路径，注意'..' 和 '.'。
path.join([path1][, path2][, ...])	|用于连接路径。该方法的主要用途在于，会正确使用当前系统的路径分隔符，Unix系统是"/"，Windows系统是"\"。
path.resolve([from ...], to)	|将 to 参数解析为绝对路径。
path.isAbsolute(path)	|判断参数 path 是否是绝对路径。
path.relative(from, to)	|用于将相对路径转为绝对路径。
path.dirname(p)	|返回路径中代表文件夹的部分，同 Unix 的dirname 命令类似。
path.basename(p[, ext])	|返回路径中的最后一部分。同 Unix 命令 bashname 类似。
path.extname(p)	|返回路径中文件的后缀名，即路径中最后一个'.'之后的部分。如果一个路径中并不包含'.'或该路径只包含一个'.' 且这个'.'为路径的第一个字符，则此命令返回空字符串。
path.parse(pathString)	|返回路径字符串的对象。
path.format(pathObject)	|从对象中返回路径字符串，和 path.parse 相反。

- 属性

属性 | 描述
------------|----------------
path.sep	|平台的文件路径分隔符，'\' 或 '/'。
path.delimiter	|平台的分隔符, ; or ':'.
path.posix	|提供上述 path 的方法，不过总是以 posix 兼容的方式交互。
path.win32	|提供上述 path 的方法，不过总是以 win32 兼容的方式交互。

# Net 模块

用于底层的网络通信的小工具，包含了创建服务器/客户端的方法

> https://nodejs.org/dist/latest-v7.x/docs/api/net.html

- 方法

方法	|描述
---|---
net.createServer([options][, connectionListener])	|创建一个 TCP 服务器。参数 connectionListener 自动给 'connection' 事件创建监听器。
net.connect(options[, connectionListener])	|返回一个新的 'net.Socket'，并连接到指定的地址和端口。当 socket 建立的时候，将会触发 'connect' 事件。
net.createConnection(options[, connectionListener])	|创建一个到端口 port 和 主机 host的 TCP 连接。 host 默认为 'localhost'。
net.connect(port[, host][, connectListener])	|创建一个端口为 port 和主机为 host的 TCP 连接 。host 默认为 'localhost'。参数 connectListener 将会作为监听器添加到 'connect' 事件。返回 'net.Socket'。
net.createConnection(port[, host][, connectListener])	|创建一个端口为 port 和主机为 host的 TCP 连接 。host 默认为 'localhost'。参数 connectListener 将会作为监听器添加到 'connect' 事件。返回 'net.Socket'。
net.connect(path[, connectListener])	|创建连接到 path 的 unix socket 。参数 connectListener 将会作为监听器添加到 'connect' 事件上。返回 'net.Socket'。
net.createConnection(path[, connectListener])	|创建连接到 path 的 unix socket 。参数 connectListener 将会作为监听器添加到 'connect' 事件。返回 'net.Socket'。
net.isIP(input)	|检测输入的是否为 IP 地址。 IPV4 返回 4， IPV6 返回 6，其他情况返回 0。
net.isIPv4(input)	|如果输入的地址为 IPV4， 返回 true，否则返回 false。
net.isIPv6(input)	|如果输入的地址为 IPV6， 返回 true，否则返回 false。

- net.Server

方法	|描述
---|---
server.listen(port[, host][, backlog][, callback])	|监听指定端口 port 和 主机 host ac连接。 默认情况下 host 接受任何 IPv4 地址(INADDR_ANY)的直接连接。端口 port 为 0 时，则会分配一个随机端口。
server.listen(path[, callback])	|通过指定 path 的连接，启动一个本地 socket 服务器。
server.listen(handle[, callback])	|通过指定句柄连接。
server.listen(options[, callback])	|options 的属性：端口 port, 主机 host, 和 backlog, 以及可选参数 callback 函数, 他们在一起调用server.listen(port, [host], [backlog], [callback])。还有，参数 path 可以用来指定 UNIX socket。
server.close([callback])	|服务器停止接收新的连接，保持现有连接。这是异步函数，当所有连接结束的时候服务器会关闭，并会触发 'close' 事件。
server.address()	|操作系统返回绑定的地址，协议族名和服务器端口。
server.unref()	|如果这是事件系统中唯一一个活动的服务器，调用 unref 将允许程序退出。
server.ref()	|与 unref 相反，如果这是唯一的服务器，在之前被 unref 了的服务器上调用 ref 将不会让程序退出（默认行为）。如果服务器已经被 ref，则再次调用 ref 并不会产生影响。
server.getConnections(callback)	|异步获取服务器当前活跃连接的数量。当 socket 发送给子进程后才有效；回调函数有 2 个参数 err 和 count。

事件	|描述
---|---
listening	|当服务器调用 server.listen 绑定后会触发。
connection	|当新连接创建后会被触发。socket 是 net.Socket实例。
close	|服务器关闭时会触发。注意，如果存在连接，这个事件不会被触发直到所有的连接关闭。
error	|发生错误时触发。'close' 事件将被下列事件直接调用。

- net.Socket

事件|	描述
---|---
lookup	|在解析域名后，但在连接前，触发这个事件。对 UNIX sokcet 不适用。
connect	|成功建立 socket 连接时触发。
data	|当接收到数据时触发。
end	|当 socket 另一端发送 FIN 包时，触发该事件。
timeout	|当 socket 空闲超时时触发，仅是表明 socket 已经空闲。用户必须手动关闭连接。
drain	|当写缓存为空得时候触发。可用来控制上传。
error	|错误发生时触发。
close	|当 socket 完全关闭时触发。参数 had_error 是布尔值，它表示是否因为传输错误导致 socket 关闭。

属性|	描述
---|---
socket.bufferSize	|该属性显示了要写入缓冲区的字节数。
socket.remoteAddress	|远程的 IP 地址字符串，例如：'74.125.127.100' or '2001:4860:a005::68'。
socket.remoteFamily	|远程IP协议族字符串，比如 'IPv4' or 'IPv6'。
socket.remotePort	|远程端口，数字表示，例如：80 or 21。
socket.localAddress	|网络连接绑定的本地接口 远程客户端正在连接的本地 IP 地址，字符串表示。例如，如果你在监听'0.0.0.0'而客户端连接在'192.168.1.1'，这个值就会是 '192.168.1.1'。
socket.localPort	|本地端口地址，数字表示。例如：80 or 21。
socket.bytesRead	|接收到得字节数。
socket.bytesWritten	|发送的字节数。

方法|	描述
---|---
new net.Socket([options])	|构造一个新的 socket 对象。
socket.connect(port[, host][, connectListener])	|指定端口 port 和 主机 host，创建 socket 连接 。参数 host 默认为 localhost。通常情况不需要使用 net.createConnection 打开 socket。只有你实现了自己的 socket 时才会用到。
socket.connect(path[, connectListener])	|打开指定路径的 unix socket。通常情况不需要使用 net.createConnection 打开 socket。只有你实现了自己的 socket 时才会用到。
socket.setEncoding([encoding])	|设置编码
socket.write(data[, encoding][, callback])	|在 socket 上发送数据。第二个参数指定了字符串的编码，默认是 UTF8 编码。
socket.end([data][, encoding])	|半关闭 socket。例如，它发送一个 FIN 包。可能服务器仍在发送数据。
socket.destroy()	|确保没有 I/O 活动在这个套接字上。只有在错误发生情况下才需要。（处理错误等等）。
socket.pause()	|暂停读取数据。就是说，不会再触发 data 事件。对于控制上传非常有用。
socket.resume()	|调用 pause() 后想恢复读取数据。
socket.setTimeout(timeout[, callback])	|socket 闲置时间超过 timeout 毫秒后 ，将 socket 设置为超时。
socket.setNoDelay([noDelay])	|禁用纳格（Nagle）算法。默认情况下 TCP 连接使用纳格算法，在发送前他们会缓冲数据。将 noDelay 设置为 true 将会在调用 socket.write() 时立即发送数据。noDelay 默认值为 true。
socket.setKeepAlive([enable][, initialDelay])	|禁用/启用长连接功能，并在发送第一个在闲置 socket 上的长连接 probe 之前，可选地设定初始延时。默认为 false。 设定 initialDelay （毫秒），来设定收到的最后一个数据包和第一个长连接probe之间的延时。将 initialDelay 设为0，将会保留默认（或者之前）的值。默认值为0.
socket.address()	|操作系统返回绑定的地址，协议族名和服务器端口。返回的对象有 3 个属性，比如{ port: 12346, family: 'IPv4', address: '127.0.0.1' }。
socket.unref()	|如果这是事件系统中唯一一个活动的服务器，调用 unref 将允许程序退出。如果服务器已被 unref，则再次调用 unref 并不会产生影响。
socket.ref()	|与 unref 相反，如果这是唯一的服务器，在之前被 unref 了的服务器上调用 ref 将不会让程序退出（默认行为）。如果服务器已经被 ref，则再次调用 ref 并不会产生影响。

# 代码

> https://github.com/hans007/JavaScriptCodes/tree/master/nodejs-do

[我的博客](https://hans007.github.io)