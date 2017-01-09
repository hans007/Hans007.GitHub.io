---
layout: post
title: nodejs - 第十节:异步处理async
category: nodejs
tags:
  - nodejs
---

# 适用范围

大批量数据并行处理，比如批量插入数据库、批量下载资源。
程序流程控制
可用于浏览器

## 手册

https://www.npmjs.com/package/async
http://caolan.github.io/async/

当前版本 v2.1.4
由三部分组成
1. Collections 集合
2. Control Flow 流程
3. Utils 工具

## 安装

```
npm install --save async
```

## 函数规则

xxxLimit 控制并行处理的数量

xxxSeries 按顺序执行

## Collections 集合



名称 | 说明
----|---------
concat            | 将多个异步操作的结果合并为一个数组。
concatSeries      | 
detect            | 用于取得集合中满足条件的第一个元素。
detectLimit       | 
detectSeries      | 
each              | 如果想对同一个集合中的所有元素都执行同一个异步操作。
eachLimit         | 
eachSeries        | 
eachOf            | key value 方式迭代
eachOfLimit       | 
eachOfSeries      | 
every             | 如果集合里每一个元素都满足条件，则传给最终回调的result为true，否则为false
everyLimit        |
everySeries       | 
filter            | 使用异步操作对集合中的元素进行筛选, 需要注意的是，iterator的callback只有一个参数，只能接收true或false。
filterLimit       |
filterSeries      |
map               | 对集合中的每一个元素，执行某个异步操作，得到结果。所有的结果将汇总到最终的callback里。与each的区别是，each只关心操作不管最后的值，而map关心的最后产生的值。
mapLimit          | 
mapSeries         | 
mapValues         | key value 方式遍历
mapValuesLimit    |
mapValuesSeries   |
reduce            | 可以让我们给定一个初始值，用它与集合中的每一个元素做运算，最后得到一个值。reduce从左向右来遍历元素，如果想从右向左，可使用reduceRight。
reduceRight       | 从右侧开始
reject            | reject跟filter正好相反，当测试为true时则抛弃
rejectLimit       |
rejectSeries      |
some              | 当集合中是否有至少一个元素满足条件时，最终callback得到的值为true，否则为false.
someLimit         | 
someSeries        |
sortBy            | 对集合内的元素进行排序，依据每个元素进行某异步操作后产生的值，从小到大排序。
transform         | 对集合内元素加工 并返回最终结果

## Control Flow 流程

名称 | 说明
----|---------
applyEach          | 实现给一数组中每个函数传相同参数，通过callback返回。如果只传第一个参数，将返回一个函数对象，我可以传参调用。
applyEachSeries    | 按函数先后返回
auto               | 用来处理有依赖关系的多个任务的执行。
autoInject         |
cargo              | 一个串行的消息队列，类似于queue，通过限制了worker数量，不再一次性全部执行。不同之处在于，cargo每次会加载满额的任务做为任务单元，只有任务单元中全部执行完成后，才会加载新的任务单元。
compose            | 创建一个包括一组异步函数的函数集合，每个函数会消费上一次函数的返回值。把f(),g(),h()异步函数，组合成f(g(h()))的形式，通过callback得到返回值。
during             |
doDuring           |
until              | until与whilst正好相反，当test为false时循环，与true时跳出。其它特性一致。
doUntil            | doUntil与doWhilst正好相反，当test为false时循环，与true时跳出。其它特性一致。
whilst             | 相当于while，但其中的异步调用将在完成后才会进行下一次循环。
doWhilst           | 相当于do…while, doWhilst交换了fn,test的参数位置，先执行一次循环，再做test判断。
forever            | 无论条件循环执行，如果不出错，callback永远不被执行。
parallel           | 并行执行多个函数，每个函数都是立即执行，不需要等待其它函数先执行。传给最终callback的数组中的数据按照tasks中声明的顺序，而不是执行完成的顺序。
parallelLimit      |
priorityQueue      | 
queue              | 是一个串行的消息队列，通过限制了worker数量，不再一次性全部执行。当worker数量不够用时，新加入的任务将会排队等候，直到有新的worker可用。
race               | 
retry              |
retryable          |
seq                |
series             | 串行执行，一个函数数组中的每个函数，每一个函数执行完成之后才能执行下一个函数。
times              | 异步运行,times可以指定调用几次，并把结果合并到数组中返回
timesLimit         |
timesSeries        |
waterfall          | 按顺序依次执行一组函数。每个函数产生的值，都将传给下一个。

## Utils 工具

名称 | 说明
----|---------
apply          | 创建函数表达式 async.apply(函数, 参数1, 参数2),
asyncify       | 
constant       |
dir            | 与log类似，不同之处在于，会调用浏览器的console.dir()函数，显示为DOM视图。
ensureAsync    |
log            | 执行某异步函数，并记录它的返回值，日志输出。
memoize        | 让某一个函数在内存中缓存它的计算结果。对于相同的参数，只计算一次，下次就直接拿到之前算好的结果。
nextTick       |
reflect        |
reflectAll     |
setImmediate   |
timeout        |
unmemoize      | 让已经被缓存的函数，返回不缓存的函数引用。






[我的博客](https://hans007.github.io)