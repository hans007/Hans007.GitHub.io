---
layout: post
title: nodejs - 第九节:第三方包整理
category: nodejs
tags:
  - nodejs
---

# 基础扩展

## array

> 数组array
> https://www.npmjs.com/package/array

## string format

> 字符串格式化
> https://www.npmjs.com/package/string-format

## async

> 同步处理
> https://www.npmjs.com/package/async
> http://caolan.github.io/async/index.html

## eventproxy

> event事件处理框架
> https://www.npmjs.com/package/eventproxy
> https://github.com/JacksonTian/eventproxy

# string

> http://stringjs.com
> https://www.npmjs.com/package/string

# 功能性

## config-lite

> 项目配置
> https://www.npmjs.com/package/config-lite

## superagent

> client-side HTTP request library
> https://www.npmjs.com/package/superagent

## ejs

> 页面模板引擎
> https://www.npmjs.com/package/ejs

## cheerio

> dom Query 操作
> https://www.npmjs.com/package/cheerio

## log4js

> 日志
> https://www.npmjs.com/package/log4js

# 服务端

## expressjs

> web框架
> http://www.expressjs.com.cn/

## 七牛

> npm install qiniu
> https://www.npmjs.com/package/qiniu
> http://developer.qiniu.com/code/v6/sdk/nodejs.html

## mongolass

> mongodb lib ^2.3.3
> https://www.npmjs.com/package/mongolass

# mongoose

> http://mongoosejs.com/
> https://github.com/Automattic/mongoose

# mysql

> https://github.com/mysqljs/mysql

# another-json-schema

> 定义数据模型
> https://github.com/nswbmw/another-json-schema
>
> 数据模型定义json
> https://docs.mongodb.com/manual/data-modeling/

# node-http-proxy

> 代理服务器
> https://github.com/nodejitsu/node-http-proxy

# express-http-proxy

> express的代理服务 
> https://github.com/villadora/express-http-proxy

```javascript
var express = require("express");
var proxy = require("express-http-proxy");
var app = express();
var port = 8088;

var apiProxy = proxy("localhost:8080",{
	forwardPath:function(req,res){
		return req._parsedUrl.path
	}
})
app.use("/restapi/*",apiProxy);
app.use("/admin/*",apiProxy);
app.use("*.xls",apiProxy);
app.use(express.static('src'));

app.listen(port);
console.log('Now serving the app at http://localhost:'+port);
```



[我的博客](https://hans007.github.io)