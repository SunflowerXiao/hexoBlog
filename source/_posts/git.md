---
title: git
tags:
  -- git
categories: 
  - js 
---

1. 修改gitignore后使它重新生效

```
git rm -r --cached .  #清除缓存
git add . #重新trace file
git commit -m "update .gitignore" #提交和注释
git push origin master #可选，如果需要同步到remote上的话
```
2. 切换分支

```
git checkout master 

```