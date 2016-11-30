---
layout: post
title: nodejs学习资料 - 第六节:全局对象与全局变量&常用工具
category: nodejs学习资料
tags:
  - nodejs
---

# 手册

> https://nodejs.org/api/globals.html
> https://nodejs.org/api/console.html
> https://nodejs.org/api/process.html
> https://nodejs.org/api/util.html

# 全局对象与全局变量

## `__filename` 当前正在执行的脚本的文件名

执行

```javascript
console.log( __filename );
```

输出

```
$ node main.js
/code/nodejs/main.js
```

## `__dirname` 当前执行脚本所在的目录

执行

```javascript
console.log( __dirname );
```

输出

```
$ node main.js
/code/nodejs
```

## `setTimeout(cb, ms) ` 只执行一次指定函数

执行

```javascript
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setTimeout(printHello, 2000);
```

输出

```
$ node main.js
Hello, World!
```

## `clearTimeout(t)` 清除定时器

执行

```javascript
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
var t = setTimeout(printHello, 2000);

// 清除定时器
clearTimeout(t);
```

输出

```
$ node main.js
```

## `setInterval(cb, ms)` 会不停地调用函数

执行

```javascript
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setInterval(printHello, 2000);
```

输出

```
$ node main.js
Hello, World!
Hello, World!
Hello, World!
Hello, World!
Hello, World!
```

## `clearInterval(t)` 函数来清除定时器

执行

```javascript
...
var t = setInterval(printHello, 2000);
clearInterval(t);
```

输出

```
$ node main.js
```

## `console` 控制台标准输出

方法   |    描述
----------------------------------------|-------------------------------------
console.log([data][, ...])              | 向标准输出流打印字符并以换行符结束。该方法接收若干 个参数，如果只有一个参数，则输出这个参数的字符串形式。如果有多个参数，则 以类似于C 语言 printf() 命令的格式输出。
console.info([data][, ...])             | P该命令的作用是返回信息性消息，这个命令与console.log差别并不大，除了在chrome中只会输出文字外，其余的会显示一个蓝色的惊叹号。
console.error([data][, ...])            | 输出错误消息的。控制台在出现错误时会显示是红色的叉子。
console.warn([data][, ...])             | 输出警告消息。控制台出现有黄色的惊叹号。
console.dir(obj[, options])	            | 用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。
console.time(label)	                    | 输出时间，表示计时开始。
console.timeEnd(label)                  | 结束时间，表示计时结束。
console.trace(message[, ...])           | 当前执行的代码在堆栈中的调用路径，这个测试函数运行很有帮助，只要给想测试的函数里面加入 console.trace 就行了。
console.assert(value[, message][, ...])	| 用于判断某个表达式或变量是否为真，接手两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会输出第二个参数，否则不会有任何结果。

执行

```javascript
// 程序运行耗时
console.time("程序运行耗时");
console.log('Hello world'); 
console.log('byvoid%diovyb'); 
console.log('byvoid%diovyb', 1991);
console.trace()
console.timeEnd('程序运行耗时');
console.info("程序执行完毕。")
```

输出

```
Hello world 
byvoid%diovyb 
byvoid1991iovyb
Trace
    at Object.<anonymous> (/Users/nodejs-do/09-全局对象与变量/main.js:30:9)
    at Module._compile (module.js:409:26)
    at Object.Module._extensions..js (module.js:416:10)
    at Module.load (module.js:343:32)
    at Function.Module._load (module.js:300:12)
    at Function.Module.runMain (module.js:441:10)
    at startup (node.js:134:18)
    at node.js:962:3
程序运行耗时: 22ms
程序执行完毕。
```

## `process` 是一个全局变量，即 global 对象的属性。

事件	| 描述
-------------------|--------------------------
exit               | 当进程准备退出时触发
beforeExit	       | 当 node 清空事件循环，并且没有其他安排时触发这个事件。通常来说，当没有进程安排时 node 退出，但是 'beforeExit' 的监听器可以异步调用，这样 node 就会继续执行。
uncaughtException  | 当一个异常冒泡回到事件循环，触发这个事件。如果给异常添加了监视器，默认的操作（打印堆栈跟踪信息并退出）就不会发生。
Signal             | 事件	当进程接收到信号时就触发。信号列表详见标准的 POSIX 信号名，如 SIGINT、SIGUSR1 等。

执行

```javascript
process.on('exit', function(code) {

  // 以下代码永远不会执行
  setTimeout(function() {
    console.log("该代码不会执行");
  }, 0);

  console.log('退出码为:', code);
});
console.log("程序执行结束");
```

输出

```
$ node main.js
程序执行结束
退出码为: 0
```

### 退出状态码

名称 | 描述
--------------------------------------------|----------------------------------------------------------
Uncaught Fatal Exception	                | 有未捕获异常，并且没有被域或 uncaughtException 处理函数处理。
Unused	                                    | 保留
Internal JavaScript Parse Error	            | JavaScript的源码启动 Node 进程时引起解析错误。非常罕见，仅会在开发 Node 时才会有。
nternal JavaScript Evaluation Failure	    | JavaScript 的源码启动 Node 进程，评估时返回函数失败。非常罕见，仅会在开发 Node 时才会有。
Fatal Error	                                | V8 里致命的不可恢复的错误。通常会打印到 stderr ，内容为： FATAL ERROR
Non-function Internal Exception Handler	    | 未捕获异常，内部异常处理函数不知为何设置为on-function，并且不能被调用。
Internal Exception Handler Run-Time Failure	| 未捕获的异常， 并且异常处理函数处理时自己抛出了异常。例如，如果 process.on('uncaughtException') 或 domain.on('error') 抛出了异常。
Invalid Argument	                        | 可能是给了未知的参数，或者给的参数没有值。
Internal JavaScript Run-Time Failure	    | JavaScript的源码启动 Node 进程时抛出错误，非常罕见，仅会在开发 Node 时才会有。
Invalid Debug Argument	                    | 设置了参数--debug 和/或 --debug-brk，但是选择了错误端口。
Signal Exits	                            | 如果 Node 接收到致命信号，比如SIGKILL 或 SIGHUP，那么退出代码就是128 加信号代码。这是标准的 Unix 做法，退出信号代码放在高位。

## Process 属性

属性 | 描述
--------------|--------------------
stdout        | 标准输出流。
stderr	      | 标准错误流。
stdin	      | 标准输入流。
argv	      | argv 属性返回一个数组，由命令行执行脚本时的各个参数组成。它的第一个成员总是node，第二个成员是脚本文件名，其余成员是脚本文件的参数。
execPath	  | 返回执行当前脚本的 Node 二进制文件的绝对路径。
execArgv	  | 返回一个数组，成员是命令行下执行脚本时，在Node可执行文件与脚本文件之间的命令行参数。
env	          | 返回一个对象，成员为当前 shell 的环境变量
exitCode	  | 进程退出时的代码，如果进程优通过 process.exit() 退出，不需要指定退出码。
version	      | Node 的版本，比如v0.10.18。
versions	  | 一个属性，包含了 node 的版本和依赖.
config	      | 一个包含用来编译当前 node 执行文件的 javascript 配置选项的对象。它与运行 ./configure 脚本生成的 "config.gypi" 文件相同。
pid	          | 当前进程的进程号。
title	      | 进程名，默认值为"node"，可以自定义该值。
arch	      | 当前 CPU 的架构：'arm'、'ia32' 或者 'x64'。
platform	  | 运行程序所在的平台系统 'darwin', 'freebsd', 'linux', 'sunos' 或 'win32'
mainModule	  | require.main 的备选方法。不同点，如果主模块在运行时改变，require.main可能会继续返回老的模块。可以认为，这两者引用了同一个模块。

执行

```javascript
// 输出到终端
process.stdout.write("Hello World!" + "\n");

// 通过参数读取
process.argv.forEach(function(val, index, array) {
   console.log(index + ': ' + val);
});

// 获取执行路局
console.log(process.execPath);

// 平台信息
console.log(process.platform);
```

输出

```
$ node main.js
Hello World!
0: node
1: /cpde/nodejs/main.js
/usr/local/node/0.10.36/bin/node
darwin
```

## Process 提供了很多有用的方法

方法 | 描述
--------------------------------|-----------------------------------
abort()                         | 这将导致 node 触发 abort 事件。会让 node 退出并生成一个核心文件。
chdir(directory)	            | 改变当前工作进程的目录，如果操作失败抛出异常。
cwd()	                        | 返回当前进程的工作目录
exit([code])	                | 使用指定的 code 结束进程。如果忽略，将会使用 code 0。
getgid()	                    | 获取进程的群组标识（参见 getgid(2)）。获取到得时群组的数字 id，而不是名字。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
setgid(id)	                    | 设置进程的群组标识（参见 setgid(2)）。可以接收数字 ID 或者群组名。如果指定了群组名，会阻塞等待解析为数字 ID 。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
getuid()	                    | 获取进程的用户标识(参见 getuid(2))。这是数字的用户 id，不是用户名。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
setuid(id)	                    | 设置进程的用户标识（参见setuid(2)）。接收数字 ID或字符串名字。果指定了群组名，会阻塞等待解析为数字 ID 。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
getgroups()	                    | 返回进程的群组 iD 数组。POSIX 系统没有保证一定有，但是 node.js 保证有。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
setgroups(groups)	            | 设置进程的群组 ID。这是授权操作，所有你需要有 root 权限，或者有 CAP_SETGID 能力。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
initgroups(user, extra_group)	| 读取 /etc/group ，并初始化群组访问列表，使用成员所在的所有群组。这是授权操作，所有你需要有 root 权限，或者有 CAP_SETGID 能力。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
kill(pid[, signal])	            | 发送信号给进程. pid 是进程id，并且 signal 是发送的信号的字符串描述。信号名是字符串，比如 'SIGINT' 或 'SIGHUP'。如果忽略，信号会是 'SIGTERM'。
memoryUsage()	                | 返回一个对象，描述了 Node 进程所用的内存状况，单位为字节。
nextTick(callback)	            | 一旦当前事件循环结束，调用回到函数。
umask([mask])	                | 设置或读取进程文件的掩码。子进程从父进程继承掩码。如果mask 参数有效，返回旧的掩码。否则，返回当前掩码。
uptime()	                    | 返回 Node 已经运行的秒数。
hrtime()	                    | 返回当前进程的高分辨时间，形式为 [seconds, nanoseconds]数组。它是相对于过去的任意事件。该值与日期无关，因此不受时钟漂移的影响。主要用途是可以通过精确的时间间隔，来衡量程序的性能。你可以将之前的结果传递给当前的 process.hrtime() ，会返回两者间的时间差，用来基准和测量时间间隔。

执行

```javascript
// 输出当前目录
console.log('当前目录: ' + process.cwd());

// 输出当前版本
console.log('当前版本: ' + process.version);

// 输出内存使用情况
console.log(process.memoryUsage());
```

输出

```
$ node main.js
当前目录: /code/nodejs
当前版本: v4.3.2
{ rss: 12541952, heapTotal: 4083456, heapUsed: 2157056 }
```

# `util` 常用工具

提供常用函数的集合，用于弥补核心JavaScript 的功能 过于精简的不足。

很多方法已经不推荐使用，详见官方手册。

## util.inherits

通过原型复制来实现的。

执行

```javascript
var util = require('util');

// 基类
function Base(){
    this.name = 'base';
    this.say = function(){
        console.log('say:'+this.name);
    };
}
Base.prototype.show = function(){
    console.log('show:'+this.name);
};

// 继承类
function Sub(){
    this.name = 'sub';
}

util.inherits(Sub,Base);

var objSub = new Sub();
//objSub.say();
objSub.show();
```

输出

```
$ node inherits.js
show:sub
```

> 只是复制方法函数，但是构造函数内部不复制。
> 所有如果去掉say() ，报错。

## util.inspect 

> 将任意对象转换 为字符串的方法
> util.inspect(object,[showHidden],[depth],[colors])
> showHidden 是一个可选参数，如果值为 true，将会输出更多隐藏信息。
> depth 表示最大递归的层数，如果对象很复杂，你可以指定层数以控制输出信息的多 少。如果不指定depth，默认会递归2层，指定为 null 表示将不限递归层数完整遍历对象。 
> 如果color 值为 true，输出格式将会以ANSI 颜色编码，通常用于在终端显示更漂亮 的效果。
> 特别要指出的是，util.inspect 并不会简单地直接把对象转换为字符串，即使该对 象定义了toString 方法也不会调用。

执行

```javascript
var util = require('util'); 
function Person() { 
    this.name = 'byvoid'; 
    this.toString = function() { 
    return this.name; 
    }; 
} 
var obj = new Person(); 
console.log(util.inspect(obj)); 
console.log(util.inspect(obj, true, 2, true));
```

输出

```
$ node inspect.js
Person { name: 'byvoid', toString: [Function] }
Person {
  name: 'byvoid',
  toString:
   { [Function]
     [length]: 0,
     [name]: '',
     [arguments]: null,
     [caller]: null,
     [prototype]: { [constructor]: [Circular] } } }
```


## util.format(format[, ...args])

> 格式化输出
> %s - 字符串
> %d - 数字
> %j - 输出 json
> %% - 输出自定义字符 这里是 '%'

执行&输出

```javascript

util.format('%s:%s', 'foo');
// Returns: 'foo:%s'

util.format('%s:%s', 'foo', 'bar', 'baz');
// Returns: 'foo:bar baz'

util.format(1, 2, 3);
// Returns: '1 2 3'

```

## util.deprecate(function, string)

标记函数被弃用

执行

- deprecate.js

```javascript
const util = require('util');

exports.puts = util.deprecate(function() {
  for (var i = 0, len = arguments.length; i < len; ++i) {
    process.stdout.write(arguments[i] + '\n');
  }
}, 'util.puts: Use console.log instead');
```

- main.js

```javascript
var obj = require('./deprecate');
obj.puts();
```

输出

```
$ node main.js
util.puts: Use console.log instead
```

# 代码

> https://github.com/hans007/JavaScriptCodes/tree/master/nodejs-do

[我的博客](https://hans007.github.io)