---
title: iframe
tags:
  -- iframe
categories: 
  - js  
---

1. iframe的onload会在window的onload前执行
2. 获取window下的iframe对象： `document.getElementById('ifrm');`
3. 获取iframe的document对象： `document.getElementById('ifrm').contentDocument`
4. 获取iframe的window对象：   `document.getElementById('ifrm').contentWindow`
5. ifram调用window对象的属性或者方法：　`top.xxx`
6. 每当iframe加载页面，过程内会激活onreadystatechange事件三次，相应的状态分别是loading,interactive和complete，而最后一次才是complete. 
