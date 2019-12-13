---
title: 页面缓存问题
tags:
  -- cache
categories: 
  - js  
---

#### html页面js和css缓存
几天前，公司用ifram做了一个可以整合不同项目的架构。 但是因为ifram自身样式限制的问题， 一些有下拉或者弹框的页面不能实现整个页面查看的效果。比如说头部如果有鼠标放上去的hover效果的话，ifram会截掉下拉。

所以我们就把头部单独抽出来放在了页面的index.html里面。

但这个又有一个问题。因为我们的头部其实也是一个项目，只是将他的打包文件引入到了index.html中而已。每次如果头部文件需要修改，都要重新打包，重新修改引入链接。因为我们打包出来的文件是有hash随机数的。

因为这个问题，我修改了打包配置，让vue打包出来的文件名不要随机数。但这样就需要在引入那里添加随机数，才可以让页面不缓存。 写起来很简单，就是使用的 `document.write`

<script>
      document.write("<link rel='stylesheet' href='/header/css/chunk-vendors.css?v=" + Date.parse(new Date()) + "'>");
      document.write("<script type='text/javascript' src='/header/js/chunk-vendors.js?v=" + Date.parse(new Date()) + "'>");
</script>

#### ie11 GET请求缓存

IE浏览器会缓存网页中的GET和XHR的内容。 当请求为GET的时候，IE会进行识别。如果请求是第一次请求的话，IE会从服务器上读取接口，返回相应信息，但如果之前已经有请求过的话，会从缓存中读取这就导致了获取的数据不正确。

**解决方式**

1. 在get请求的url中增加随机标识
get请求后面添加时间戳 http://xxx.com?v=15895689242

2. 设置header请求头
设置请求头的header属性 `'Cache-Control': 'no-cache'`

