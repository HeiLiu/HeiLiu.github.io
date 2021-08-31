---
title: JWT (Json Web Token) 入门笔记
cover: https://cdn.pixabay.com/photo/2020/09/02/18/33/portland-head-light-5539153__480.jpg
coverHeight: 700
copyright: true
date: 2020-07-23 16:26:09
categories:
  - 笔记
---

JWT 兴起：RESTFUL 架构 + 前后端分离 + 微服务架构 （共享session问题、 session 复制问题）

Authorization：bearer <token>

## JWT 定义

> JSON Web Token (JWT)是一个开放标准(RFC 7519)，它定义了一种紧凑的、自包含的方式，用于作为JSON对象在各方之间安全地传输信息。该信息可以被验证和信任，因为它是数字签名的。

<!-- more -->
JSON Web Token 由三部分组成，格式为： header.payload.signature  
**示例**：  
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJPcGVuSUQiOiJvVVp3QTBlUXhyQ0hQaG9LdGtlUG9xRl91czBzIiwiU2Vzc2lvbktleSI6IlhUMG81MzJnckJHNmdmcTB4Y0V0aFE9PSIsIlRva2VuSUQiOiJkYjhiYTc2Ni04ZTcxLTRiY2MtYTdhNy02ZDhiYzRiN2NhNmIiLCJpYXQiOjE1OTU1NzI2MDR9.Y91tP5RYq6mQY-CF6RSgey6hLz_abk2S667TROpCu2o
```

通过**编码后的 header** 和**编码的后的 payload** 再加上一个**密钥**就可以进行签名

签名用于认证过程中判断 JWT 有没有被篡改，验证 JWT 的合法性。

![jwt 示例](/assets/jwt.png)

## 用户认证

互联网服务离不开各种用户认证，一般cookie-session流程如下:

1.用户通过账号密码登录、浏览器向服务器发送用户名和密码   
2.服务器验证通过后，在当前对话（session）里面保存相关数据，比如用户角色、登录时间等等  
3.服务器向用户返回一个 session_id，写入用户的 Cookie  
4.用户随后的每一次请求，都会通过 Cookie，将 session_id 传回服务器  
5.服务器收到 session_id，找到前期保存的数据，由此得知用户的身份  

session-cookie 的认证方式存在以下的几个缺点：  
- 如果使用服务集群、需要考虑共享 session、 session 复制问题
- session 存储用户信息会占用大量服务器内存、增加服务器的开销
- cookie 存在跨域的问题
- 用户容易受到 CSRF 攻击

## JWT 原理

JSON Web Token 认证流程如下：  
- 用户客户端通过账号和密码登录、浏览器向服务器发起登录请求 
- 服务器验证用户账号密码
- 应用提供一个 token 给客户端 
- 客户端存储 token，并且在随后的每一次请求的 header: Authorization 中都带着它 (如果是放在cookie携带去请求服务端就不能跨域) 
- 服务器校验 token 合法性以及是否过期，并返回数据

如此服务器成了无状态的了，更加的易于扩展服务

JWT 与 cookie-session机制的一个不同就是 session 是存在服务端，而 JWT 最大的特点就是把状态存在客户端。

个人认为jwt解决最大的问题不是跨域，而是前后端分离后，纯接口方面的用户认证问题。

### 参考
[JSON Web Token 入门教程](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)