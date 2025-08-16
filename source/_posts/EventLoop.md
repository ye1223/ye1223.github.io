---
title: Event Loop
categories:
  - []
tags:
  - JavaScript
  - nodejs
wordCount: 55
charCount: 664
imgCount: 2
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 42 seconds
date: 2023-10-07 17:30:21
featured_image:
---

JS单线程，为了不阻塞线程，很多代码通过回调的方式异步执行

JS代码执行顺序被打乱，就需要一种机制--事件循环，协调各个事件的执行顺序





### 浏览器

1. 同步代码，一行一行在Call Stack中执行（压栈弹栈）
2. 遇到异步代码，记录下代码，等时机到了将代码入队Callback Queue中
3. 当同步代码执行为空，Call Stack为空，Event Loop开始工作
4. Event Loop轮询查找Callback Queue中是否有可执行代码
   1. 有，将代码送入Call Stack执行
   2. 没有，将继续轮询查找



<font color=red>调用栈为空触发Event Loop执行先后顺序：微任务、DOM渲染、宏任务</font>



微任务： Promise, async/await

宏任务： 定时器、Ajax、DOM事件



<img src="https://s2.loli.net/2023/10/07/nFwAxKGfkaDUp8b.png" alt="浏览器事件循环" style="width:70%;" />



### Node

Node异步API

| 定时器                      | I/O操作                                   | node独有                           |
| --------------------------- | ----------------------------------------- | ---------------------------------- |
| setTimeout<br />setInterval | 文件读写<br />数据库操作<br />网络请求... | process.nextTick<br />setImmediate |



设计上，事件循环优先处理执行I/O事件

<img src="https://s2.loli.net/2023/10/07/Y7yhcei5f86PaS4.png" alt="Node事件循环" style="width:70%;" />



process.nextTick不属于事件循环一部分，优先于事件循环执行

从Timer到Check为一个Tick



> `setImediate(()=>{})`、与`setTimeout(()=>{}, 0)` 中执行时机不确定？
>
> setTimeout(, 0) ---> `浏览器 4ms`,` node 1ms`
>
> 解决： 将两者放在I/O操作回调中，能保证setImmediate回调先执行