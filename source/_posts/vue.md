---
title: vue的props设置多个类型
date: 2019-11-04 10:23:42
tags:
  -- vue
categories: 
  - 前端  
---

####  props传值类型设置

vue在父子组件传值的时候可以设置值有多个类型

```
props: {
  type: [String, Array],
  default: ''
}
```

#### vue全局组件注册


详情见[https://juejin.im/post/5ccaced86fb9a032196edd68]
