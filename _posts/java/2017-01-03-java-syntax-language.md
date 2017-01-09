---
layout: post
title: java - 语法回顾整理[持续更新]
category: java
tags:
  - java
---

# java8新特性

## macos环境安装配置

- jdk8

http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

- eclipse ide

https://www.eclipse.org/downloads/

## lambda概述

```java

public static void main(String[] args){
    new Action(){
        @override
        public void execute(){
            System.out.println(content);
        }
    }.execute('aaaaaa');
    Action login = (String content) -> {
        System.out.println(content);
    };
    login.execute('bbbbbb');
}

static interface Action{
    void execute(String content);
}

```

## 

# 类

## static关键字

静态变量有JVM自动初始化，全局的。
静态方法不能调用非静态的函数或变量。

```java
static String name;
static char sex;
static short age;
static float height;
static String type;
public static void main(String[] args){
    Scanner scanner = new Scanner(System.in);
    System.out.println("姓名: ");
    name = sacnner.next();
    ...
}
```

## 定义无参方法

```java
static void 方法名(){
    方法体 - 具体代码实现
}
```

## 定义带参方法

```java
static void 方法名(类型1 变量1,类型2 变量2,...){
    方法体 - 具体代码实现
}
```

## 定义带返回值的方法

```java
static 返回值类型 方法名(类型1 变量1,类型2 变量2,...){
    方法体 - 具体代码实现
    return 返回的数据;
}
```

## 封装性

- private 私有
- get set 控制访问

## 匿名对象

```java
class Student{
    public void tell(){
        System.out.println("aaa");
    }
}

new Student().tell();
```

## 构造对象

```java
class People{
    public People(){
        System.out.println("aaa");
    }
}

People per = new People();
```

## final 关键字

- 表示最终
- 不能被继承

```java
final String NAME = "aaa";
NAME = "bbb";//编译失败不能被改写
```

> 被定义 final 的变量，必须用大写标识

## abstract 抽象类

- 声明而未被实现的方法
- 子类必须实现

```java
abstract class Aaa{
    public abstract void add();
}
class Bbb extends Aaa{
    public void add(){

    }
}
```

## interface 接口

- 一种特殊的类，里面全部是常量和公共的抽象方法
- 一个类可以同时继承抽象类和实现接口

```java
interface InterA{
    public static final int AGE = 1;
    public abstract void add();
}
interface InterB{
    public abstract void say();
}
class Aaa implements InterA,InterB{
    public void add(){

    }
    public void say(){

    }
}
```

## extends 继承

- 语法

```java
class Aaa{
    ...
}
class Bbb extends Aaa{
    ...
}
```

- 限制 只允许单继承

- 限制 子类不能访问父类的私有成员

- 子类，先调用父类构造，再去调用子类构造

## super 关键字

强行调用父类方法

```java
class A{
    public void tell(){
    }
}
class B extends A{
    public void tell(){
        super.tell();
    }

```
 
## 访问权限

private < default < public

子类重写父类方法，不能比父类方法权限严格

## 多态性

- 向上转型 只能用父类方法
- 向下转型 能用子类方法

## instanceof

bool 返回值 判断是否当前类或子类

## Collections 集合

- List、ArrayList（异步，非线程安全）、Vector（同步，线程安全）

- Set、HashSet、TreeSet

- Iterator

```java
Iterator<String> iter = lists.iterator();
while(iter.hasNext()){
    ...
}
```

- Map、HashMap、Hashtable
无需存放，key不允许重复

## Generic 泛型

```java
class Point<T,V> {
	private T x;
    private V y;

	public T getX() {
		return this.x;
	}

    public V getY() {
        return this.y;
    }
}
```

- 未知对象类型时，可以用 ? 通配符。

- 泛型方法

```java
class Gener{
    public <T> tell(T t){
        return t;
    }
}

Gener g = new Gener();
g.tell("aaa");
```

- 泛型数组

```java
public static <T> void tell(T arr[]){
    ...
}
```



# 变量与数据类型

- 基础8种类型

byte short int long float double char boolen

## 变量的命名、定义和初始化

- 首字母小写
- 驼峰方式 myImages
- 定义 类型 变量1, 变量2, ...;

## 变量的作用域

```java
{
    String name = "aaa";
}
String name = "bbb";

代码块、局部变量、全局变量

```

## 基本数据类型的包装类

- 首字母大写的 包装类
Byte Short Interger Long Float Double Character Bollean

- 包装常用方法

## 二进制补码

- 补码计算
(1101)
0010 取反
0011 加1
补码 -3







[我的博客](https://hans007.github.io)