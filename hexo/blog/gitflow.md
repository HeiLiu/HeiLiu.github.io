---
title: Gitflow
copyright: true
date: 2021-05-12 15:53:56
tags:
---

# Git 操作流程（命令行操作）

---

## 基本概念：

- 工作区
- 暂存区
- 远程仓库  
  
[相关链接](https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576)
  <!-- more -->
  
### 分支

### 常设分支

- master 主分支 --保留完全稳定的代码，有可能仅仅是已经发布或即将发布的代码
- develop 开发分支 --做后续开发或者测试稳定性

### 临时分支

- feature 需求/功能分支 --为开发某个需求/功能创建的分支
- release 发布分支 --可能从 develop 分支分离而来，但是一定要合并到 develop 和 master 分支上
- hotfix 热修复分支

[示例仓库](https://gitlab.planetmeican.com/liujianglong/miniprogramdemo)

切换到工作目录下面，初始化一个 git 仓库
```git
$ git init
Initialized empty Git repository in /Users/feWork/gitflow/.git/
```

在 gitlab 创建一个空项目并关联远程仓库
```git
$ git remote add origin git@gitlab.planetmeican.com:***/gitflow.git
```

或者你也可以不在本地创建直接从远程把项目从远程 `clone` 到本地
当你从远程仓库克隆时，实际上`Git`自动把本地的 `master` 分支和远程的 `master` 分支对应起来了，并且，远程仓库的默认名称是 `origin`

```bash
$ git clone git@gitlab.planetmeican.com:***/miniprogramdemo.git
```

查看当前项目的分支情况

```
$ git branch
* master
(END)
```

`git branch`命令列出所有分支，当前分支前面标`*`号。

创建 `develop` 分支，且切换到 `develop` 分支：

```bash
$ git checkout -b dev
Switched to a new branch 'develop'
```

`git checkout`命令加上 `-b` 参数表示创建并切换，相当于以下两条命令：

```bash
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

在 `develop` 上完成项目的基础开发配置之后、提交到远程仓库

想要把 `develop` 分支的修改合并到 `master` 上

先切换到 `master` 分支 然后使用 `git merge` 命令

```bash
$ git checkout master
Switched to branch 'master'

$ git merge develop
Removing .gitlab-ci.yml
Merge made by the 'recursive' strategy.
 .gitlab-ci.yml | 25 -------------------------
 1 file changed, 25 deletions(-)
 delete mode 100644 .gitlab-ci.yml
```

`git merge` 命令用于合并指定分支到当前分支

通过 `git status` 查看工作区当前状态、发现 `merge` 完后 `master` 比远程多一个提交 需要 `push` 同步到远程

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

```bash
$ git push origin master
Total 0 (delta 0), reused 0 (delta 0)
To gitlab.planetmeican.com:***/miniprogramdemo.git
   b8c9984..b74cb20  master -> master
```

这个时候就实现工作区和远程的 `master` 和 `develop` 的内容完全一致了

使用分支完成某个任务，合并后再删掉分支 一般合并完成后可以将原分支删除、 `develop` 作为长期存在的开发分支 可以保留

```bash
$ git branch -d develop
Deleted branch develop (was b74cb20）
$ git branch
* master
(END)
```

### feature 分支

通常开发新功能会在 `develop` 上创建 `feature` 分支进行某个功能的开发、完成后合并 `feature` 到 `develop`，删除该 `feature` 分支。

```bash
$ git checkout -b develop
Switched to a new branch 'develop'
$ git checkout -b feature1
Switched to a new branch 'feature1'
```

在 `feature1` 上开发完当前功能经过 review 和测试后合入 `develop` 分支

### 冲突合并

在开发中容易与其他正在开发的 `feature` 文件产生冲突
在把 `feature1` 合入 `develop` 时、因为同一文件被他人修改了产生冲突如果自动合并失败需要本地手动修改再把这次修改进行提交

```bash
$ git merge feature1
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
$ git status
On branch develop
Your branch is up to date with 'origin/develop'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

在本地处理完冲突再进行提交
完成再删除本地和远程的 `feature` 分支
```bash
$ git branch -d feature1
Deleted branch feature1 (was f275c08).
$ git push origin --delete feature1
To gitlab.planetmeican.com:liujianglong/miniprogramdemo.git
 - [deleted]         feature1
```
### release 分支

当你认为现在 `develop` 分支的代码已经是一个成熟的 `release` 版本时，这意味着：第一，它包括所有新的功能和必要的修复；第二，它已经被测试过了。如果上述两点都满足，那就是时候开始生成一个新的 `release` 了

使用版本号命名 `release` 分支是一个明智的选择，这个命名方案还有一个很好的附带功能，那就是当我们完成了 `release` 后，使用 git tag 打上对应的版本标签 🏷️。

有了一个 `release` 分支，再完成针对 `release` 版本号的最后准备工作（如果项目里的某些文件需要记录版本号），并且进行最后的编辑

完成 `release`

将 `release` 的内容合并到 `master` 和 `develop` 两个分支中去，这样不仅产品代码为最新的版本，而且新的功能分支也将基于最新代码

```bash
$ git checkout master
$ git merge --no-ff release-xxx
$ git tag -a v1.0.1
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean$$
$ git push origin master
$ git checkout develop
$ git merge --no-ff release-xxx
$ git push origin develop
$ git branch -d release-xxx
```

依据你的设置，对 `master` 的提交可能已经触发了你所定义的 `CI/CD` 流程，或者你可以通过手动部署发布到线上

### 其他

 - [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
