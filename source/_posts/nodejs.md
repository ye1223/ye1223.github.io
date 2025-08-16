---
title: nodejs
categories:
  - []
wordCount: 185
charCount: 2694
imgCount: 4
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 2 minutes
date: 2023-06-01 21:17:23
tags:
featured_image:
---

# pre

1. **运行时（Runtime)**
"运行时"就是程序运行的时候，也就是**指令加载到内存并由CPU执行**的时候。

与之相对应的是“编译时”，其指代码编译的时候，也就是C代码编译成可执行文件的时候，此时指令没有被CPU执行。

2. **运行时库（Runtime Library）**
运行时库就是程序运行的时候所需要依赖的库。

3. **运行时环境（Runtime environment）**
运行环境（英语：Runtime environment）又称“运行时系统”（run-time system），指一种把半编译的运行码在目标机器上运行的环境。

----



# 简介

Nodejs 只是一个js**运行时环境**

访问系统内核，访问本地文件，链接服务器....

nodejs在**浏览器之外**运行**v8引擎**

**跨平台**

适合干IO密集型应用，不适合CPU密集型（单线程）

> CPU 密集型： 图像、音频处理需要大量的数据结构+算法
------
# 全局变量

node `global`

浏览器 `window`

`globalThis`根据环境自动判断

ECMAScript中有的全局，如Math..

* `__dirname` 当前文件所在目录(绝对路径)

* `__filename` 当前文件路径（绝对路径）

* `__extname` 文件后缀

* `Buffer`

* `process`  处理进程
  * `process.argv` 获取参数数组
  * `process.cwd()` 目录
  * `process.exit()` 终止进程
  * `process.on('exit', ()=>{console.log('退出')})` 监听事件




------





`nodejs`应用在（<font color=red>长期运行!!</font>`httpserver返回++counter`）**单个进程**中运行，无需为每个请求创建新的线程

（相比，Apache，每一个请求创建一个线程）

单线程，并发量为1

采用了**非阻塞**的开发范式（事件循环机制） + v8引擎加持，轻松应对**高并发**

> 主线程是单线程，io是libuv维护的线程池

 

<br/>

当函数调用栈内有函数运行时，js不能处理其他请求

<br/>

异步模块（多线程）和事件循环（监听 派发，不占用单独线程）

循环不停监听异步模块处理进度，等处理完成后，派发函数调用栈执行

<br/>  

最快的速度清空函数调用栈，把耗时的操作全部做异步处理

 node将【异步操作和对应的回调函数】封装成一个请求对象，交给底层的异步模块处理

 异步操作有结果之后，回调函数进入事件循环等待执行，

 事件循环在调用栈清空时，按照某个优先级顺序将回调函数推入到调用栈执行

------
# node异步API

1. 定时器： `setTimeOut` ` setInterval` （最小1ms，浏览器4ms）
2. I/O操作： 文件读写，数据库操作，网络请求
3. node独有的，`process.nextTick`、`setImmediate`

<br/>

> nextTick(**优先级比事件循环队列更高**)
>
> 微任务（promise）
>
> 
>
> timer
>
> -->poll（当执行到这里时卡住，检查timer或者check队列有误需要 执行的）
>
> check

Poll 阻塞，从设计上，是想优先处理IO事件的

Settimeout(,0)与setimmdeiate 放入io中使用，定时器，总会先执行check队列的操作

<br/>

> Timer -> check运行一周称为一个`Tick`
>
> nextTick先于下一个Tick执行

**异步代码进入异步模块以非阻塞的形式执行，对应的异步函数会在对应的异步代码执行完成后，派发到不同的队列中**

## 异常处理

为了健壮性：捕获并处理每一个错误

* 同步代码 try catch

* 异步代码 
  * Promise (catch)
  * async await trycatch



## 异步编程 流程控制

回调函数 --> Promise ---> async await

<br/>

 node官方的库

遵循，错误优先风格

<br/>

回调函数，需要顺序执行，就要嵌套的写，但是导致回调地狱

<br/>



> 平行
>
> 顺序



# module

module并不是全局变量，每一个模块有他相应的模块

<br/>

核心模块（随着node）

第三方模块

自定义模块（引用路径）

<br/>

运行时加载 cmj，知道运行时候再报错

编译时加载 esm

 `import from` 写在模块顶层

 `import()` （异步，返回promise）

<br/>

`V8`

预编译阶段 （ESM

  -分配内存空间

  -确定作用域链...

执行阶段 （CMJ

 <br/>

-----

# Buffer

js字符串不可变，所有对字符串的操作都要生成一个新的字符串

<br/>

`fs`模块读取，不生命文件类型，默认返回的都是文件二进制缓冲区

```js
const buffer1 = Buffer.alloc(5) //申请五个字节
console.log(buffer1.toString())
buffer1.write('string') //多写入的部分不会写入
console.log(buffer1, buffer1.toString())


console.log(Buffer.from('a string'))
```



----

# stream

![image-20230601204439944.png](https://s2.loli.net/2023/09/03/JmQjx7fKbWprE1C.png)

<br/>

i/o操作

端到端数据交换

<br/>

加载 缓冲区 处理

流模式 加载一点处理一点

<br/>

![image-20230601204653589.png](https://s2.loli.net/2023/09/03/LGmBcjDfOF5giHV.png)

<br/>



![image-20230618200922252.png](https://s2.loli.net/2023/09/03/iLAFjDZa3q7Sxd5.png)


----





# SEO

* TDK
  * title
  * description (meta)
  * Key(meta)

* 语义化标签

* `<a/>` href

* `<img/>` href alt

* 一个页面一个h1 和 main标签
* .....

借助第三方库jsdom（jsdom模拟浏览器环境的库，可以在 Node.js 中使用 DOM API），服务端渲染