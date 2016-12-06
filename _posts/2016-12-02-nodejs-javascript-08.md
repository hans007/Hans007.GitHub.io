---
layout: post
title: nodejs - 第八节:ECMAScript语言整理
category: nodejs
tags:
  - nodejs
---

# 使用严格模式

## 定义

具有作用域特点

- 全局

```javascript
"use strict";
...
```

-局部

```javascript
(function(){
  "use strict";
  ...
})();
```

## 规定

> 定义变量必须用 var
> 属性名、参数名 不能重名
> 禁用八进制表示数字,整数的第一位如果是0，表示这是八进制数
> 不能删除变量 delete
> 保留字不能用作标识符
> 禁止使用with语句
> 创设eval作用域
> 禁止this关键字指向全局对象
> 禁止在函数内部遍历调用栈
> 函数必须声明在顶层

> 这里可以推荐大家阅读下阮老师的博文
> [阮一峰 - Javascript 严格模式详解](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)

## 注释

- 单行用 \\

- 多行用 

/* ... */

/*
*
*
*
*/

## 语句

> 一行一个语句
> 分号结尾
> 大括号表示代码段

## 关键字&保留字

- 参考 ECMA-262 中定义

# 数据类型

## 变量

- 弱类型 可以互相转换
- 性质 包存值的占位符
- 定义 用var来声明

## 数据类型分类

### 简单数据类型

- Undefined 没有对象
- NULL 空指针
- String

> 单引号 双引号 完全相同
> 都可以转义 反斜杠 \ ，换行 \n
> 整个代码上下文统一

- Number

> Number.MAX_VALUE
> Number.MIN_VALUE

> 检查是否能表示
> isFinite(13213213)

> var n = 1e

> NaN 非法数字
> isNaN('aaa') 是否非法数字
> true
> Number('1231') 数字转换

> e 是科学计数法
> var y=123e5; // 12300000
> var z=123e-5; // 0.00123

> parseInt 转整形
> parseFloat 转浮点

- Boolean

### 复杂数据类型

- Object

表示值或函数方法
无序

> var objs = new Object();
> Object {}

> var objs = {};

## typeof

> var a = 1e5
> typeof(a)
> "number"

# 操作符

## 一元操作符

> ++ --
> 前置 先计算和赋值一起操作
> 后置 计算后再赋值
>
> int n = 1
> ++n
> 2
> n++
> 2
> n
> 3

## 位操作符

## 布尔操作符

> 与 或 非

```javascript
alert(1&&2)的结果是2
只要“&&”前面是false，无论“&&”后面是true还是false，结果都将返“&&”前面的值;
只要“&&”前面是true，无论“&&”后面是true还是false，结果都将返“&&”后面的值;

alert(0||1)的结果是1
只要“||”前面为false,不管“||”后面是true还是false，都返回“||”后面的值。
只要“||”前面为true,不管“||”后面是true还是false，都返回“||”前面的值。
```

## 四则运算操作符

> 加、减、乘、除、求模

## 关系操作符

>  大于 等于 不等于 等于
> 全等于 === 会比较类型
> 全不等 !==

## 条件操作符

> var a = (a>b ? a : b)

## 赋值操作符

> =
> += -= *= /= %=
> 位运算 <<= 、>>= 、>>>=

## 逗号操作符

> var a = 1, b = 2, c= 3

# 语句

## 条件

> if(){}else{}else if{}

## 循环

> while(true){}
> do{}while(true)
> for(var i =0;i<=numMax;i++){}
> for(var key in 对象或数组){}

## 分支

> switch(表达式、变量或值){ case 值或表达式: ... break; }

## break 与 continue

## with语句

```javascript
with(req.session.user){
  log(name,sex);
}
```

## label语句

```javascript
var itemsPassed = 0;
var i, j;

top:
for (i = 0; i < items.length; i++){
    for (j = 0; j < tests.length; j++)
        if (!tests[j].pass(items[i]))
            continue top;
        itemsPassed++;
}
```

# 函数

## 定义

```javascript
function sum(a,b){
  console.log('sum : ', a+b);
}

var sum2 = new Function(
  'a',
  'b',
  'console.log('sum : ', a+b);'
);

var sum3 = function(a,b){
  console.log('sum : ', a+b);
};
```

## 默认返回值

```javascript
Undefined
```

## 保留字 arguments

```javascript
function testArg(){
  console.log('arguments: ', arguments);
}

testArg(1,2,3,4,5);
```

## 参数个数

```javascript
sum.length
2
```

## 特性

> 匿名
> 回调

# 变量

## 定义

> var 方式是局部
> 默认是全局(不推荐)

# ECMAScript 引用类型

## Object 类型

```javascript
var obj = {name:'aaa'}
obj.name
obj['name']
```

## 基本包装类型

> 基本类型自带的方法
> 如 String.toString()

## Global 对象

> 属性
> infinity nan undefined null

> 方法
> eval() isFinite() isNan() ...

## Math 对象

> 数学对象
> Math.PI Math.E Math.SQRT2 Math.SQRT1_2 Math.min() Math.max()
> Math.random()

# 数组

## 初始

```javascript
var arr = new Array(3);
var arr = new Array('a', 'b', false);
var arr[4] = 'ab';
```

## 检查

```javascript
arr instanceof Array
Array.isArray(arr)
```

## 转换与排序

```javascript
var obj = {a:1, b:2, c:3, d:false};
Object.keys(obj);
Object.keys(obj).length;

// 分割
var str = 'a b c d';
str.split(' ');

// 打印
str.toString();
a,b,c,d
str.join('|');
a|b|c|d

// 排序
var arr = [11,2,33,66,55];
function compareAB(a,b){
  if(a > b){
    return 1;
  } else if(a === b){
    return 0;
  }else if(a < b){
    return -1;
  }
}
arr.sort(compareAB);
console.log(arr);
```

## 栈和队列操作

```javascript
// 压入，加在尾端
arr.push(100,101);
// 弹出，尾端出栈
arr.pop();

// 队列，头部弹出
arr.shift();
// 队列，头部压入
arr.unshift(222);
```

## 其它

```javascript
// concat 连接另一个数组，加入尾端
var arr1 = ['a1','a2'];
arr.concat(arr1)

// slice 取出
arr.slice(2,5); // 从2位置取到5，取出3个

// splice 删除、插入
arr.splice(1,1); // 从1位置删除1个
arr.splice(1,0,'a3','a4'); // 从1的位置 删除0个 插入 a3 a4

// 定位
indexOf();// 头部开始
lastIndexOf();// 尾部开始

// 加工函数，需要传入自定义函数
// every() 所有的成员都返回true 才是 true
arr.every(function(m){
  return m > 10;
});

// some() 有一个是true 就是 true
arr.some(function(m){
  return m > 10;
});

//filter() 返回值为true的组成一个新数组
arr.filter(function(m){
  return m > 10;
});

//map() 对数据处理 返回新数组
arr.map(function(m){
  return m + 10;
});

//forEach() 数组循环遍历
arr.forEach(function(m){
  console.log(m);
});

// reduce() prev 是上次的return 结果
arr.reduce(function(prev, cur, index, arr){
  return prev + cur;
});

// reduceRight() 是反序列操作
```


[我的博客](https://hans007.github.io)