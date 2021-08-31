---
title: 同一台电脑配置多个SSH
date: 2019-07-31
cover: https://cdn.pixabay.com/photo/2020/12/27/12/07/sunrise-5863751__480.png
tags: Git
categories:
  - 倒腾
  - Note
  - Git
---


## 配置多个SSH

在工作中多访问公司的gitlab仓库，而在生活中又有自己的github仓库  
记录一下方便日后查找

---  

一般电脑中的SSH key 存放在 `～/.ssh`目录中 如果有配置过的话存在 `id_rsa` \\ `id_rsa.pub` 私钥和公钥, 将公钥配置到需要的代码平台

生成SSH key

### 本地配置多个ssh key
- 1、为公司生成一对秘钥ssh key  
```js
  ssh-keygen -t rsa -C 'yourEmail@xx.com' -f ~/.ssh/gitlab_id_rsa
```

- 2、为github生成一对秘钥ssh key  
```js
  ssh-keygen -t rsa -C 'yourEmail2@xx.com' -f ~/.ssh/github_id_rsa
```

- 3、在~/.ssh目录下新建名称为`config`的文件（无后缀名）。  用于配置多个不同的host使用不同的ssh key，常用内容如下：
```js
  # gitlab
  Host gitlab.planetmeican.com
      HostName gitlab.planetmeican.com
      Port 2345
      User git
      PreferredAuthentications publickey
      IdentityFile ~/.ssh/gitlab_id_rsa
  # github
  Host github.com
      HostName github.com
      PreferredAuthentications publickey
      IdentityFile ~/.ssh/github_id_rsa
```

### 配置文件参数
```
  # Host : Host可以看作是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和ssh文件
  # port: 端口号，一般不需要配置
  # HostName : 要登录主机的主机名
  #PreferredAuthentications: 授权验证方式
  # User : 登录名
  # IdentityFile : 指明上面User对应的identityFile路径
```

- 4、分别往gitlab和github上添加生成的公钥 

## ssh-agent 与 SSH的区别

[参考链接](http://yijiebuyi.com/blog/4b5c272e7058cb331098250c8e98eb3e.html)