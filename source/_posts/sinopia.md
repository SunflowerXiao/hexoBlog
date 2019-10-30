---
title: 如何构建自己的npm私有库
date: 2019-10-30 09:06:23
tags:
---

现在前端进行开发的时候，都会使用npm包管理工具获取一些公共的包。但是在自己内部团队开发的时候，可能需要一些共用模块，但是我们又不想让别人看到他，这个时候就可以搭建一个自己私有的npm库。团队其他人只需要安装nrm，切换库源，就可以查看库里面包的信息进行下载。方便快捷。 而不需要从其他项目中找到对应模块功能，拷贝，还可以进行同步更新。

npm私有库拥有以下有点：
* 私密性更高
* 更容易维护，使用服务托管安全性更高

### 如何搭建一个私有npm库？

1. git + ssh方式
2. [sinopia](https://github.com/rlidwka/sinopia)
3. [cnpmjs.org](https://link.zhihu.com/?target=https%3A//github.com/cnpm/cnpmjs.org)

这里我使用sinopia, 因为他配置比cnpmjs更加简单，不用配置数据库等信息，而git + ssh搭建的库无法使用npm update命令

sinopia允许我们零配置搭建本地npm库。无需安装和复制整个couchDB数据库。sinopia在自己的内部集成了一个小的数据库，如果包不存在数据库中的话，他会询问[npmjs.org](https://www.npmjs.com/)

sinopia安装

```
npm install -g sinopia
```

sinopia启动

```
sinopia

warn  --- config file - /home/map/.config/sinopia/config.yaml
warn  --- http address - http://localhost:4873/
```

此时访问localhost:4873,可获取html文件并且服务端响应正常，表示安装成功。(如果是在服务器上面安装，就访问ip:4873)


