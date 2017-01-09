---
layout: post
title: springmvc - 第一节:helloworld!
category: springmvc
tags:
  - java
---

# 前言

helloworld!的意义重要,就无需我多说了吧。按步骤来就行。

## 1. 安装 eclipse for ee


## 2. 配置 tomcat 服务


## 3. 下载 spring jar

http://maven.springframework.org/release/org/springframework/spring/

我这里选择 4.3.5.RELEASE

http://maven.springframework.org/release/org/springframework/spring/4.3.5.RELEASE/

## 4. 下载 Commons Logging jar

http://commons.apache.org/proper/commons-logging/download_logging.cgi

## 5. 新建项目 springMVC01

- 项目类型 Dynamic Web project

![](http://7xosx4.com1.z0.glb.clouddn.com/new-project-type.png)

- 输入项目名称 及 配置

![](http://7xosx4.com1.z0.glb.clouddn.com/new-project-setting.png)

- 完成后项目结构

![](http://7xosx4.com1.z0.glb.clouddn.com/new-project-ls-l.png)

## 6. 加入所需的 jar 文件

![](http://7xosx4.com1.z0.glb.clouddn.com/helloword-jars.png)

## 7. 编辑 web.xml

/WEB-INF/web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
    id="WebApp_ID" version="2.5">
    
    <!-- 配置DispatchcerServlet -->
    <servlet>
        <servlet-name>springDispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 配置Spring mvc下的配置文件的位置和名称 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springDispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

> mvc的配置都在文件 springmvc.xml 中
> servlet-mapping 表示拦截模式， `/` 表示对所有请求

## 8. springmvc.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">
        
        
        <!-- 配置自动扫描的包 -->
        <context:component-scan base-package="com.yuu.springmvc"></context:component-scan>
        
        <!-- 配置视图解析器 如何把handler 方法返回值解析为实际的物理视图 -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name = "prefix" value="/WEB-INF/views/"></property>
            <property name = "suffix" value = ".jsp"></property>
        </bean>
</beans>
```

## 9. 编写 HelloWorld.java

- 新建包

com.yuu.springmvc.controller

- HelloWorld.java

```java
package com.yuu.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloWorld {
	
    /**
     * 1. 使用RequestMapping注解来映射请求的URL
     * 2. 返回值会通过视图解析器解析为实际的物理视图, 对于InternalResourceViewResolver视图解析器，会做如下解析
     * 通过prefix+returnVal+suffix 这样的方式得到实际的物理视图，然后会转发操作
     * "/WEB-INF/views/success.jsp"
     * @return
     */
    @RequestMapping("/helloworld")
    public String hello(){
        System.out.println("hello world");
        return "success";
    }
}
```


[我的博客](https://hans007.github.io)