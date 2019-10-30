---
title: hexo博客搭建
tags:
   - hexo
   - ubuntu
categories:
   - 服务器
comments: true   
---

本文主要介绍我是如何构建一个hexo博客，并且通过本地git上传可以更新到服务器的，我服务器使用的系统是ubuntu

### 前期准备

1. 服务器登录账号。

可以是root账号，但是不建议使用，因为root账号权限很大，可以自己[创建一个账号](/blog/2019/10/28/createAccount/)，并且给该账号赋予相应的权限


2. [配置GIT](/blog/2019/10/28/git-install/)

3. [安装NodeJs](/blog/2019/10/28/nodejs-install/)

4. [配置Nginx](/blog/2019/10/28/nginx-install/)

5. github账号（或者其他git托管项目账号）


#### 配置过程

1. 本地安装hexo-cli脚手架，用来快速创建hexo框架

``` bash
$ npm install hexo-cli -g
```

2. 本地安装hexo-server，可以再发布之前预览和检测代码页面

``` bash
$ npm install hexo-server -g
```

3. 使用脚手架新建项目

``` bash
$ hexo init myapp
$ cd myapp
$ npm install
```

4. 创建新页面,默认使用config.yml配置中的dafault_layout来做模板生成.md文件

每个.md文件必须要有他自己的front-matter设置，详情 （https://hexo.io/docs/front-matter.html）

dafault_layout有三个选项（post/page/draft），详情查看(https://hexo.io/docs/writing)

``` bash
$ hexo new pageName
$ hexo publish pageName #当default_layout不为page的时候，需要执行该发布语句，文章才能可见
```

5. 创建git仓库，用来托管源代码文件

    a. 登录github或者其他git托管网站，新建git仓库， 获取git仓库地址
    b. 添加本地项目到远程git

``` bash
git add .
git commit -m 'firstCommit'
git remote add origin https://github.com/XXX/XXX.git
git push -u origin master
```

6. 创建git仓库， 用来托管打包后的文件

    a. 登录服务器， 创建git远程仓库

    ```
    sudo mkdir /var/home/blog_bare #root用户不用打sudo
    sudo cd /var/home/blog_bare
    sudo git init --bare #创建一个git裸仓库
    sudo cd blog_bare
    sudo ls #列出blog_bare下的文件
    ```

    b. 创建git hook, 进入blog_bare下的hooks文件夹，使用文本编辑器nano 创建post-receive文件

    ```bash
    sudo cd blog_bare/hooks
    sudo nano post-receive
    ```

    c. 复制如下语句到文本编辑器：

    ```
    #!/bin/bash
    git --work-tree /var/home/blog --git-dir=/var/home/blog_bare
    ```

    该语句相当于把git init出的文件分成两部分。 一部分只有git信息的内容，该内容存放在blog_bare文件夹下， 另外项目代码部分则放置在blog文件夹下

    d. 给post-receive文件**付权限!****付权限!**, 不然之后会上传不了代码

    ```bash
    sudo chmod +x /var/home/blog_bare/hooks/post-receive
    ```

7. 如下所示， 设置 hexo 中 config.yml文件的deploy

```bash
deploy: 
    type: git
    repo: ssh://username@server1.example.com/var/home/blog_bare
    branch: master
```

这里的repo就是填写服务器上部署的git项目地址，如果本地git用户名和服务器的账户名称不一致，需要在repo地址上标明账户名称。
如果服务器没有域名，可以直接填写ip

8. 编译hexo文件, 编译后的文件就会自动部署到config.yml文件中设置的git地址中去

```bash
hexo clean
hexo generate
hexo deploy
```


9. 修改nginx配置
登入服务器，找到nginx配置文件

```bash
sudo nano /etc/nginx/sites-available/default
```

修改代理配置，我这里的配置如下

```bash
server {
    listen 45.76.223.152:80 default_server; #告诉nginx需要监听那个服务器的哪个端口
    serve_name ; #域名，要监听的地址，因为我都是在同一台服务器而且我也没有域名就不填写了 

    location / {
        root /var/home/blog; 
        index index.html index.htm;
    }
    # 如果项目需要通过二级域名访问，需要使用alias配置项目目录不能使用root。
    # alias表示使用/var/home/blog目录来替代域名中的/log，
    # root#表示在域名/log/var/home/blog
    # location /log{
    #    alias /var/home/blog
    #    index index.html
    # }
}
```

重启nginx

```bash
sudo nginx -s reload
```

如果项目最终要通过二级域名访问，需要设置config.yml的url以及root字段来设置访问路径，避免资源找不到的问题。

```bash
url: http://192.xxx.xxx.xx/blog/
root: /blog/
```



到了这里差不多就可以通过访问http:// xx.xx.xx/blog来访问我们的项目了。

### 其他

*现在执行hexo deploy的时候因为设置的ssh git方式，所以git push的时候需要填写服务器密码，如果想要一劳永逸需要设置ssh key*
*服务器配置mysql*
*hexo主题修改和优化*
*添加网站访问统计*
*修改favicon*
*添加顶部加载条*
*博客推广及谷歌搜索优化（必读）*
*添加热门阅读板块*
*页脚加上微信二维码*
*插入音乐和视频*
.....

[更多信息请查看](https://www.digitalocean.com/community/tutorials/how-to-create-a-blog-with-hexo-on-ubuntu-14-04#step-1-%E2%80%94-installing-and-initializing-hexo)
