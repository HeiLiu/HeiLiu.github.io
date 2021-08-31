---
title: Hexo 博客文章内容加密
date: 2021-08-25 21:15:25
tags:
  - Hexo
---

- 安装对应依赖

  ```bash
    $ npm install --save hexo-blog-encrypt 
    或
    $ yarn add hexo-blog-encrypt 
  ```
<!-- more -->
- 修改文章开头部分 添加 password 到   [ Front-matter](https://hexo.io/zh-cn/docs/front-matter) 中
```
---
title: Hello World
date: 2013/7/13 20:46:25
password: 123
---
```

- 修改博客根目录下 _config.yml 文件

添加如下安全相关配置项：
```
# Security
encrypt: # hexo-blog-encrypt
  abstract: 简单描述。
  message: 加密文章，可能是个人情感宣泄或者生活记录。
  tags:
    - { name: tagName, password: 密码A }
    - { name: tagName, password: 密码B }
  template:
    <div id="hexo-blog-encrypt" data-wpm="{{hbeWrongPassMessage}}" data-whm="{{hbeWrongHashMessage}}">
    <div class="hbe-input-container">
    <input type="password" id="hbePass" placeholder="{{hbeMessage}}" />
    <label>{{hbeMessage}}</label>
    <div class="bottom-line"></div>
    </div>
    <script id="hbeData" type="hbeData" data-hmacdigest="{{hbeHmacDigest}}">{{hbeEncryptedData}}</script>
    </div>
  wrong_pass_message: 抱歉，密码不太对，再瞅瞅？
  wrong_hash_message: 抱歉，这个文章不能被校验，不过您还是能看看解密后的内容。
```
