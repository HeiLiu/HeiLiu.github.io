---
title: gitError
copyright: true
top: 100
date: 2020-01-09 14:19:31
tags:
---

## git push 时报错  

在push到远程的时候收到报错： 
```
git push origin develop

error: src refspec develop does not match any.
error: failed to push some refs to 'https://gitlab-ci-token:***'

ERROR: Job failed: command terminated with exit code 1
```

主要原因是当时的暂存区中为空 没有文件  

背景：  
在处理 gitlab 的时候 有一个流程是：  
 生成版本 -> 生成CHANGELOG -> push到repo  

因为我在本地在做一个功能测试的时候，生成了CHANGELOG，在gtilab中再次生成log的时候生成的是一样的文件也就不存在任何修改。此时push暂存区是没有任何内容的，因此报错。