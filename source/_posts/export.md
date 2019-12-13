---
title: exports, module.exports, export default, export的区别
tags:
  -- js
categories: 
  - 前端  
---

经常在不同的项目里面使用到exports, module.exports, export default, export这几个方法，但是经常会用错，一直都不是很明白。今天来梳理一下。

```
require  node和es6都引入

export / import 只有es6支持的导出引入

module.exports / exports 只有node支持的导出
```

### node

Node里面有一个module模块系统,在node模块系统中，每个文件都可以被视为一个独立的模块。

模块系统遵循的是CommonJS规范。CommonJS定义的模块分为: 模块标识(module)、模块定义(exports) 、模块引用(require)。



#### module.exports 和 exports

在一个node执行一个文件时，会给这个文件内生成一个 exports和module对象，而module又有一个exports属性。他们之间的关系如，下都指向同一块{}内存区域。

```
// util.js
console.log(module.exports); // 打印出来一个{}对象
console.log(exports);    // 打印出来一个{}对象
console.log(module.exports === exports) // true
```

现在在特殊的exports对象上面定义一个属性 a, a的值为100

```
// util.js
let a = 100;
exports.a = a;
console.log(module.exports); // 打印出来一个{a: 100}对象
console.log(exports);    // 打印出来一个{a: 100}对象
console.log(module.exports === exports) // true
```

但如果这个时候直接将特殊的exports对象赋值给一个新的引用，打印结果就如下：

```
// util.js
let a = 100;
exports.a = a;
console.log(module.exports); // 打印出来一个{a: 100}对象
console.log(exports);    // 打印出来一个{a: 100}对象
console.log(module.exports === exports) // true

exports = {b: 100};
console.log(module.exports); // 打印出来一个{a: 100}对象
console.log(exports);    // 打印出来一个{b: 100}对象
console.log(module.exports === exports) // false
```

**发现exports打印出来的内容和module.exports打印出来的不一致的，因为他们从同一个指向对象变成了不同的指向对象。**

新建一个index.js

```
// index.js
var util = require('./index.js');
console.log(util.b); // underfined
console.log(util.a); // 100
```

**从上面可以看出，其实require导出的内容是module.exports的指向的内存块内容，并不是exports的。**

**简而言之，区分他们之间的区别就是 exports 只是 module.exports的引用，辅助后者添加内容用的。**


 
### es6

es6中混淆点： export, export default(), import a from 'x', import {a} from 'x'

#### export 和 export default

**异同点**
- export和export default 都可以导出常量、函数、文件、模块
- 在一个文件或模块中，export、import可以有多个，export default仅有一个
- 通过export方式导出，在导入时要加{ }，export default则不需要
- export能直接导出变量表达式，export default不行。

```
// testEs6Export.js

//导出变量
export const a = '100';  

 //导出方法
export const dogSay = function(){ 
    console.log('wang wang');
}

 //导出方法第二种
function catSay(){
   console.log('miao miao'); 
}
export { catSay };


//export default导出
const m = 100;
export default m; 
//export defult const m = 100;// 这里不能写这种格式。

```

调用方式

```
import { dogSay, catSay } from './testEs6Export'; // 导出了 export 方法 
import m from './testEs6Export';  // 导出了 export default 

import * as testModule from './testEs6Export';  //as 集合成对象导出
import { area as dogSay } from 'testEs6Export';
```