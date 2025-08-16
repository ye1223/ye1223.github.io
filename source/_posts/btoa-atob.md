---
title: btoa-atob
categories:
  - []
tags:
  - new found
wordCount: 73
charCount: 1619
imgCount: 1
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 36 seconds
date: 2024-02-19 17:36:07
featured_image:
---
今天在找登录token发先存于Cookie的access_token被加密成ASCII字符串，遂找到了两个window下的api

```js
// base64Encryption
export function encryptToken(token) {
  const encryptedToken = btoa(token)
  return encryptedToken
}
export function decryptToken(encryptedToken) {
  const decryptedToken = atob(encryptedToken)
  return decryptedToken
}
```



**Binary To ASCII**

**`btoa()`** 方法可以将一个*二进制字符串*（例如，将字符串中的每一个字节都视为一个二进制数据字节）编码为 [Base64](https://developer.mozilla.org/zh-CN/docs/Glossary/Base64) 编码的 ASCII 字符串。

**ASCII To Binary **

**`atob()`** 对经过 base-64 编码的字符串进行解码

![bta atob](https://s2.loli.net/2024/02/19/6sy4PoWhdcw5BXJ.png)