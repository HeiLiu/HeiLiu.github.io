---
title: Git 笔记  
date: 2019-07-28  
tags: Git  
categories: 
  - Skill
  - Note
  - Git
copyright: true
---

## Git 常用的命令笔记

Commitizen是一个撰写合格 Commit message 的工具。 

### 本地分支重命名

git branch -m old new
<!-- more -->
### 链接远程仓库 
```
  git remote add origin 仓库地址
  仓库地址：https://git.coding.net/xxxxxxxxxxxxx/my-project.git  
  git push -u orgin master
```

### 删除远程仓库

```
git remote remove origin
```

### 修改远程仓库（源地址）

```
  git remote set-url origin []
```
如下：

```
Project mctDev/cafeteria-supplier-desktop was moved to another location

The project is now located under cafereria-fe / cafeteria-supplier-desktop

To update the remote url in your local repository run (for ssh):

git remote set-url origin git@gitlab.planetmeican.com:cafereria-fe/cafeteria-supplier-desktop.git

or for http(s):

git remote set-url origin https://gitlab.planetmeican.com/cafereria-fe/cafeteria-supplier-desktop.git
```

#### 原理解析  
  git remote set-url --add origin 就是往当前git项目的config文件里增加一行记录  
  config文件打开方式有两种：

  使用命令`git config -e`
  在当前git项目的根目录下，文件位于 .git/config (.git目录为隐藏文件)
  你每执行一次git remote set-url --add origin 就会增加一行，如下图：

  `git remote -v`:显示当前所有远程库的详细信息，显示格式为 远程库名字 url连接(类型)  
  [参考链接](https://my.oschina.net/shede333/blog/299032)


### git checkout的用法
```
  git checkout  // 不跟参数，则对工作区进行检查 可以返回工作区文件的状态
  git checkout -b 分支名 // 切换到对应分支 如果没有则新建一条分支
  git checkout -B 分支名 // 强制切换分支 如果存在同名分支会发生覆盖
  git checkout commit_id(hash) // 切换到对应的commit版本 （分离头指针） 
    //此时的HEAD不指向分支，指向对应的commit_id 通过 git checkout master 切回分支
```
  [checkout命令详解参考](https://www.cnblogs.com/hutaoer/archive/2013/05/07/git_checkout.html)

### 撤销本地修改的文件  
```
git checkout [filename] // 撤销某个文件的修改
git checkout .  // 撤销所有修改的文件 
```

### 撤回 add/commit
#### 将add到暂存区后的提交撤回
  git reset head 文件名

#### 修改commit标注
```
  commit -m "aaaa" // 提交一个commit
  git commit --amend => 开启vim编辑器 编辑后保存退出
```
#### 撤回commit
  ```
    git reset --soft commit_id // 撤回commit 到commit_id soft 只是撤回commit 本地文件不会修改
    git reset --hard commit_id // 撤回commit 到commit_id hard 只是撤回commit 本地文件回退
  ```
### 拉取远程分支
```
git pull origin [分支名]
```

### 添加/删除远程分支

  + 将本地分支推送到远程分支上，如果远程分支不存在，则创建此远程分支
  ```
    git push origin 本地分支名:远程分支名

    $ git push origin test:master         // 提交本地test分支作为远程的master分支 
    $ git push origin test:test           // 提交本地test分支作为远程的test分支
    //好像只写这一句，远程的github就会自动创建一个test分支
  ```

  + 如果想删除远程的分支呢？类似于上面，如果:左边的分支为空，那么将删除:右边的远程的分支。
  ```
    git push origin :远程分支名(你要删除的远程分支名)
  ```

### git reflog  
 可以显示已删除的操作

### git rebase


