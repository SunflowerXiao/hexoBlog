---
title: ubuntu上传图片到服务器
date: 2019-10-29 09:04:17
tags:
---


## 本地上传文件到ubuntu

**详情（https://www.runoob.com/linux/linux-comm-scp.html）**
```bash
scp -r localfile.txt username@192.168.0.1:/home/username/ 
```

- scp scp(secure copy)是linux系统使用的用于复制文件和目录的命令

```bash
scp [可选参数] file_source file_target
```