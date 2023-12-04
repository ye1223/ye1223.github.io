---
title: HTTP summary
categories:
  - []
tags:
  - http
  - Computer Network
wordCount: 150
charCount: 1420
imgCount: 3
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 1 minute
date: 2023-09-08 16:13:26
featured_image:
---

## ==HTTP==

**Hyper Text Transfer Protocol**

<img src="https://s2.loli.net/2023/09/08/7HasE3ePGXuqtYn.png" alt="image-20230908155626605" style="width:50%;" />

* <strong style="color:red;">Stateless</strong> : every Request is **INDEPENDENT**
  * regard each request as a single transaction
  * use `cookie`  `session` `localStorage` ... to enhance user experience( <u>memorize the state from the user</u> )



## ==HTTPS==

**Hyper Text Transfer Protocol <u>Secure</u>**

* Data sent is **encrypt**
  * **SSL** -- Secure Sockets Layer
  * **TLS** --  Transfer Layer Security
* install certificate on web host to use HTTPs






## Message
<img src="https://s2.loli.net/2023/09/08/u6OS2icBACHrtFf.png" alt="image-20230908164003822" style="width:50%;" />

### Headers
* General
  * Request URL、Request Method、Status Code、Remote Address、Referrer Policy
* Request
  * Cookies、Accept-xxx、Content-Type、Content-Length、Auhurization、User-Agent、Referrer
* Response 
  * Server、Set-Cookie、Content-Type、Content-Length、Date

## Status Code

| Status Code | Means                                   |
| ----------- | --------------------------------------- |
| 1xx         | request received / processing           |
| 2xx         | Success received, understood, accepted  |
| 3xx         | Further action must be taken / redirect |
| 4xx         | Client error                            |
| 5xx         | Server error                            |

* 200 - OK
* 201 - OK created
* 301 - Move to new URL
* 304 - Not Modified (Cached version)
* 400 - Bad Request （:x: not sending correct data）
* 401 - Unauthorized （missing token）
* 404 - Not Found (resources do not exsist)
* 500 - Internal Server Error





## HTTP2

`multiplexing`

<img src="https://s2.loli.net/2023/09/08/sf8H6glXiCnzEA2.png" alt="image-20230908164925817" style="width:50%;" />


