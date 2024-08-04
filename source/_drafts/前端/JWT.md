---
title: JWT
date: 2022-03-29 19:08:00
updated: 2022-04-22 20:00:00
tags: [JWT, token]
categories: 
  - ['JWT']
cover: 


---

# JWT



 JSON Web Token

JSON 格式

Web 

Token 令牌、凭证

cookie 只支持 http

Session 服务器保存

Session 是有状态的

JWT 是无状态的

Cookie 有 CSRF 漏洞，需要额外加 CSRF Token 解决。
JWT 放到 header 就没有这种漏洞了。

## JWT 的组成

Header 包含算法、类型等。例：{"alg": "HS256","typ": "JWT"}

Payload 可以设置数据、过期时间等。例：{"user_id": 1}

Signature 签名，将前两部分内容加密。

```js
const token = base64urlEncoding(header) + '.' + base64urlEncoding(payload) + '.' + base64urlEncoding(signature)
// eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoyMDF9.13gaC23r2h1Cx65iphWqBqSGDmUylST_p3ZoWX9Q_0M
```



## Refresh Token

下发 JWT 的同时，下发 Refresh Token

当 Token 过期就将 JWT 和 Refresh Token 发送给服务器，服务器从新下发新的 JWT

Refresh Token 也有过期时间