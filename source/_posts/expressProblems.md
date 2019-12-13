---
title: express问题列表
date: 2019-11-04 10:23:42
tags:
  -- express
categories: 
  - express  
---

**这里收集在使用express的时候出现的一下问题**

### express配置静态资源目录

```
app.use(express.static('dirName'));
```

### express开启gzip

开启gzip可以减少带宽使用，优化下载速度

```
var compression = require('compression');
app.use(compression()); //使用 gzip/deflate 压缩响应文件
```

### express解析post参数失败

post传递的参数，需要使用body-parse进行解析

```
var bodyparser = require('body-parse');
app.use(bodyParser.json());
```

### Can't set headers after they are sent to the client

res对象相当于是Nodejs里面 [http.ServerResponse]()对象的子类。 你可以重复设置头部信息 res.setHeader()很多次， 只要你没有写入过数据.

Can't set headers after they are sent to the client表示你已经在Body或者Finished状态了，但是仍然有一些方法想要修改set a header 或者 statusCode.
如果报这个错误了，找到任何尝试在正文body已经被写入之后想要send a header 的地方。


### 服务下的资源访问跨域解决

```
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*"); 
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  res.header("Access-Control-Allow-Credentials", "true");
  next();
});
```

这里讲一下下面几个参数: 

[详情](http://www.ruanyifeng.com/blog/2016/04/cors.html)
[详情2](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

**Access-Control-Allow-Origin**


**Access-Control-Allow-Headers**

**Access-Control-Allow-Credentials**

### 注意

(一) 当前端配置withCredentials=true时, 后端配置Access-Control-Allow-Origin不能为*, 必须是相应地址
(二) 当配置withCredentials=true时, 后端需配置Access-Control-Allow-Credentials
(三) 当前端配置请求头时, 后端需要配置Access-Control-Allow-Headers为对应的请求头集合

