---
title: gitLabCI
copyright: true
top: 100
date: 2020-01-03 11:54:48
tags:
---

# gitLab CI 持续集成  

GitLab CI 最大的作用是管理各个项目的构建状态，运行构建任务这种浪费资源的事情就交给 GitLab Runner 来做 因为 GitLab Runner 可以安装到不同的机器上，所以在构建任务运行期间并不会影响到 GitLab 的性能~  
<!-- more -->
CI步骤： 
1. 添加 .gitlab-ci.yml 到项目的根目录
2. 配置一个Runner  

**跳过 CI**
如果 commit 信息包涵 [ci skip] 或者 [skip ci]，不论大消息，这个 commit 将会被创建，但是 job 会被跳过

## .gitlab-ci.yml 文件

.gitlab-ci.yml 是一个 `yaml` 类型的文件 能够受版本控制。  
在任何一次push操作中， gitLab 会先去项目根目录下面查找 .gitlab-ci.yml文件，并在此次 commit 开始 jobs。 也就是说CI可以在代码提交后触发自动化的单元测试、代码预编译等。。 


.gitlab-ci.yml 示例：  
```yml
# stages 默认有三个 stage
stages:
 - build
 - test
 - depoly

#jobs
job 1:
 stage: build
 script: make build dependencies

job 2:
 stage: build
 script: make build artifacts

job 3:
 stage: test
 script: make test

job 4:
 stage: depoly
 script: npm run prettier --check '*.js'

```

### 参数配置  

#### stage  
  stage 定义了所有的 job 可以使用的阶段、stages 顺序定义了作业执行的顺序，如果没有配置 stages 默认会按照 build test depoly 三个 stage 顺序走。具体的执行逻辑配置给 job 执行相应的 script  

#### job 

job 是该配置文件中的基本元素，通过一系列参数配置了所属的 stage，以及需要在该 stage 执行的 script。 每个 job 至少需要包含一条 script 语句，如果一个 job 没有显式的关联到某个 stage，会默认关联到 test stage。示例：  

```
job1:
stage: build
script: execute script for build
```

#### script  
script 是该 job 在交给 runner 执行的 shell 脚本  

 > script: execute script for build  

 ```
 job:
  script:
    - uname -a
    - bundle exec rspec
 ```

## 配置 Runner  

在 GitLab 中，Runners 将会运行你在 .gitlab-ci.yml 中定义的 jobs。Runner 可以是虚拟机，VPS，裸机，docker 容器，甚至一堆容器。GitLab 和 Runners 通过 API 通信，所以唯一的要求就是运行 Runners 的机器可以联网。

安装 runner  
  > sudo brew install gitlab-ci-multi-runner

```
 # For Debian/Ubuntu/Mint
 curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash

 # For RHEL/CentOS/Fedora
 curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash
```

## before_script 和 after_script  

当项目中的 job 越来越多的时候大概率上会发生逻辑重复，可以将这些重复的逻辑提出放在 before_script 和 after_script 两个配置项中执行。 这个两个配置项在所有的 job 的 script 执行前和执行后调用。如下：  

> job: 配置的before_script -> 该job中的script -> 配置的after_script

[GitLab Runner](https://gitlab.com/gitlab-org/gitlab-runner)  

[gitlab 文档](https://docs.gitlab.com/ee/ci/yaml/README.html)

[Gitlab CI持续集成 - GitLab Runner 安装与注册](https://juejin.im/post/5d3eb115e51d4561bf462000) 

[部分中文文档](https://github.com/Fennay/gitlab-ci-cn/blob/master/pipelines.md)  

[用 GitLab CI 进行持续集成](https://scarletsky.github.io/2016/07/29/use-gitlab-ci-for-continuous-integration/)