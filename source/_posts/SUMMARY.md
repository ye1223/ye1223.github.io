---
title: SUMMARY
categories:
  - []
tags:
  - ¡importante!
wordCount: 3247
charCount: 64247
imgCount: 26
vidCount: 0
wsCount: 1
cbCount: 0
readTime: About 18 minutes
date: 2024-03-25 19:54:09
featured_image:
---

## 01 数据类型

基本数据类型：

- Null
  - `typeof null 等于 object`
- Undefined
  - Let var 声明未初始化值为 undefined
- Boolean
  - falsely=== `null undefined 0 '' NaN `
- Number
  - 10 进制 `1`
  - 8 进制 0 开头 `070`
  - 16 进制 0x 开头 `0x1`
  - 浮点数 包含小数点 `1.1` `1.0` `.1` `3.14e10` 科学计数法
  - NaN 不是数值 `Object.is(NaN,NaN) 返回true`
- String
- Symbol

（栈）值类型：String 、Number、 Boolean、 Symbol

（堆）引用类型：object{}、 Array、 **null**、function

## 02 数组方法

splice `增(return [])` ` 删return删除数组` `改return[]` 不影响原数组

splice(initIndex, deleteNum, InsertElement) 返回数组

slice (from, to) 截取数组，不会影响原数组，返回新数组

- 增

  - push() 尾部增加 返货数组新长度
  - unshift 首部增加 返回数组新长度
  - concat("yellow", ["red", "green"]) 不会改变原数组

- 删

  - pop() 返回删除元素

  - shift 返回删除元素

- 查
  - indexOf() 返回元素所在索引，没有返回-1
  - includes() 返回 true/false
  - find 返回第一个查找元素, 没有返回 undefined (finnIndex、findLast、findLastIndex）

### 排序方法：

- sort(?compareFn) ---toSorted
- reverse --toReverse

### 转换

- join(?separator) 转为由分隔符分隔的字符串
  - <img src="/Users/liuye/学习/前端/ForInternNotes/note/2数组方法.assets/image-20230918150627981.png" alt="image-20230918150627981" style="width:30%; float:left;" />

### 迭代

（都有 thisArg 形参，更改回调函数体内 this 指向，但是**直接使用**箭头函数 this 仍指向上一层 this）

- some() 某一个符合条件 true，every 全部符合条件 true
- forEach、filter、map

## 03 字符串方法

- 增 (返回副本)
  - `+` `${}`
  - concat()
- 删（返回副本）
  - slice(startIndex, endIndex)(与 substring 差不多，但 substring 不支持负数索引，start > end 交换索引)
  - substr(startIndex, length)
- 改
  - trim、trimLeft/Right 删空格
  - repeat(count) 重复几次这个字符串
  - padStart/End(maxLength, ?fillString) 不符合 maxLength 就长度填充， 默认填充空格
- 查
  - charAt(index) 根据索引返回元素，没有返回空串
  - indexOf
  - startWith include 查找返回 true/false

转换方法

- split() 字符串转数组
  - <img src="/Users/liuye/学习/前端/ForInternNotes/note/2数组方法.assets/image-20230918155323753.png" alt="image-20230918155323753" style="width:30%; float:left" />

模板匹配

- match(`whichpattern`)
- search
- replace(originString, replaceString) (reg, function(match, ....))

## 04 AJAX

Ajax 允许我们利用 js 和 xmlHttpRequest 在后台与服务器交换数据

允许我们不用刷新网页，与后端交换数据，局部更新部分网页

==通过 XmlHttpRequest 向服务器发送异步请求，从服务器获得数据，利用 js 操作 dom 更新页面==

```js
//1. 创建xhr对象实例
const xhr = new XMLHttpRequest()
//与服务器创建连接
xhr.open("GET", true)
//给服务端发送数据
xhr.send()
//接受服务端数据相应
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    //done 整个过程请求完成
    if (xhr.status >= 200 && shr.status <= 300) {
      console.log(xhr.responseText)
    }
  }
}
```

## 05 数据传输

multipart/form-data 可以**传输二进制文件**

application/x-www-form-urlencoded 用于传输普通的表单数据,

application/json 用于传输 JSON 对象,

二者都不会传输文件,主要传输**字符串**、**数字类型**的数据。

```js
function upload(url, Form) {
  // FormData格式提交给后端
  const params = new FormData()
  for (let i in Form) {
    params.append(i, Form[i])
  }
  //这里使用了 params.append(name, value) 方法,将字段名和值添加到 FormData 对象中。

  return axios
    .post(url, params, {
      headers: {
        "Content-Type": "multipart/form-data",
      },
    })
    .then((res) => res.data)
}
```

## 06 URL To Browser

url

协议 + 域名 + 资源名(目录名/文件名)

### DNS 解析

找缓存-

1. 浏览器
2. 操作系统
3. 本地 host

没有缓存

向域名服务器发送请求(dig +trace baidu.com)

1. 根域名 .
2. 顶级域名 .com
3. 权威域名 baidu.com

发送网络请求

### 建立 TCP 链接 三次握手

### 发送 HTTP 请求

options 请求，在其他请求之前发送

> post 请求格式为 application/json 格式才会触发 options
>
> 触发 options
>
> 1. 跨域，options 请求预先检测
> 2. 自定义请求头

### 浏览器缓存

强缓存 浏览器强制缓存服务端的数据（静态资源）

> `res.setHeader('Cache-Control', 'max-age=10')`或者设置 `res.setheader(expires GMT时间))`
>
> form disk cahce 磁盘缓存服务端的资源
>
> from memory cache 内存缓存服务端资源 ，多次刷新，就读内存里的

协商缓存

> Last-Modified GMT 时间 ==最后更改时间==
>
> if-Modified-Since
>
> 如果前后时间(或 xxx 字段)是一致的那么就代表资源没有改变，后端返回 304，
>
> 变了代表资源改变，后端返回新的资源，返回 200
>
> ETAG:'xxx' (文件 hash，or 版本号) ==唯一标识符，用于标识特定版本的资源==
>
> if-None-Match

### TCP 断链 4 次挥手

### HTML 渲染

html 解析器 解析标签为 DOM 树

样式计算

> 渲染引擎格式化计算 css 样式(rem->px bold->700..) CSSOM 构建

回流

> 计算得到几何信息、大小

> _触发回流_
>
> 1. 首次渲染
> 2. resize
> 3. 跟大小宽高有关的改变
> 4. hover
> 5. js 获取 clientWidth

重绘

> 元素样式的改变并不影响它在文档流中的位置(color backgoroung-color,visibility)，浏览器将新样式给他并重新绘制

GPU 渲染

v8 解析 js

渲染树？？

## 07 TCP HTTP

OSI 七层

应用层 （应用进程之间的交互规则） 万维网 HTTP 邮件 SMTP 域名系统 DNS

表示层 （通信的应用程序能够**解释交换数据的含义**）

会话层 （建立、管理和终止**表示层实体之间的通信会话**）

传输层 （为**进程之间的通信提供服务**，处理数据包错误、数据包次序，以及其他）**TCP UDP**

网络层 （选择合适的网间**路由和交换节点**，确保数据按时成功传送）

数字链路层 （两台主机之间的数据传输，总是在一段一段的链路上传送的，**将 IP 数据组合成帧，相邻节点传输帧**）

物理层 （定义传输媒介）

All People need data processing

## 08 VUE

v-if 切换有一个局部编译/卸载的过程，切换过程中合适的销毁和重建内部事件监听和子组件，v-show 只是简单的进行 css 的切换

> vif 切换消耗
>
> Vshow 初始渲染消耗

## 09 定义列表

自定义

```html
<dl>
  <dt>标题</dt>
  <dd>内容</dd>
</dl>
```

有序

```html
<ol>
  <li>1111</li>
  <li>2222</li>
</ol>
```

无序

```html
<ul>
  <li>11111</li>
  <li>22222</li>
</ul>
```

选择框---分组

![image-20231007174223914](https://s2.loli.net/2024/03/25/X4xb3CE5jMaNDsd.png)

filedset legend

![image-20231007174412245](https://s2.loli.net/2024/03/25/7TWLP8xkglwQc1i.png)

## 10 拖拽

![image-20230524160915054](https://s2.loli.net/2024/03/25/bt843AozKdD7Fi5.png)

## 11 `==`和`===`

### `==`

```js
// 双等号隐式类型转换 ALL true
console.log(100 == "100")
console.log(0 == "")
console.log(0 == false)
console.log("" == false)
console.log(null == undefined)
```

### 开发尽量使用`===`，

但是这种情况可以，两者等价 `obj==null` 与`obj===null || obj===undefined`

## 12 闭包

<font color=red>指函数和其周围的状态（即词法环境）的组合，作用域的特殊运用</font>

内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后

### 自由变量

在闭包函数中使用但**不是在该函数作用域**内声明的变量

自由变量的值：在**函数定义**的地方向上层寻找，与函数调用的地方无关

#### 以下是一些常见的触发闭包的情况：

1. 函数嵌套：内部函数可以访问外部函数的变量，导致形成了闭包。
2. 函数作为返回值：如果一个函数返回了一个内部函数，那么该内部函数可以访问到外部函数的变量，从而形成闭包。

​

1. 事件监听：如果在事件监听函数内部定义了函数，那么该内部函数可以访问到事件监听函数的变量，形成闭包。
2. 延迟调用：如果在一个函数内部调用了另外一个函数，并且该函数有一个定时器或者是回调函数，那么该回调函数可以访问到外部函数的变量，形成闭包。
3. IIFE（立即调用的函数表达式）：如果在一个函数内部定义了一个函数并立即调用，那么该内部函数可以访问到外部函数的变量，形成闭包。

## 13 防抖节流

<font color=red>事件被频繁触发时，减少事件执行的次数</font>

### 防抖 Debounce

在事件被触发 n 秒后再执行回调，如果在这 n 秒内又被触发，则重新计时。(input 输入，resize )

![image-20230403104255692](https://s2.loli.net/2024/03/25/KdzxpNmlB4w7MQE.png)

```html
<input type="text" id="ipt" />

<script>
  let ipt = document.querySelector("#ipt")
  ipt.addEventListener(
    "keyup",
    debounce(function () {
      console.log(ipt.value)
    })
  )

  function debounce(fn) {
    let timer = null
    return function () {
      if (timer) clearTimeout(timer)

      timer = setTimeout(() => {
        fn.apply(this, arguments)
        timer = null
      }, 500)
    }
  }
</script>
```

### 节流 Throttle

规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。（拖拽、scoll）

![image-20230403104138541](https://s2.loli.net/2024/03/25/yGBE2tDsi1QVarZ.png)

```html
<div
  class="box1"
  draggable="true"
  style="background-color: red; width: 100px;height: 100px;"
></div>

<script>
  let box1 = document.querySelector(".box1")
  box1.addEventListener(
    "drag",
    throttle(function (e) {
      console.log(e.clientX)
    })
  )

  function throttle(fn) {
    let timer = null
    return function () {
      if (timer) return
      timer = setTimeout(() => {
        fn.apply(this, arguments)
        timer = null
      }, 500)
    }
  }
</script>
```

## 14 浏览器相关

l 进程：一个程序的运行**实例**

运行一个程序，操作系统会创建一块内存， 给代码和运行时的数据使用 ，并且创建一个**线程**来处理任务

线程是 一个进程的执行任务或者控制单元，负责当前进程中程序的执行

一个进程<u>至少</u>有一个线程，多个线程可以进行数据的共享

<img src="./笔记.assets/image-20230405223549096.png" alt="image-20230405223549096" style="zoom:33%;" />

特点：

1. 一个线程崩溃，整个进程崩溃
2. 同一进程中，线程可以数据共享
3. 进程关闭后，内存会正确回收

普通单线程浏览器

- 不稳定，
- 容易卡，
- 内存泄漏（所有页面在一个进程中，单个页面因为代码编写问题数据泄露。关闭页面时，泄露的内存不会回收）
- 安全问题（插件和渲染线程拥有很高权限，各种脚本代码（通常第三方编写））

<img src="./笔记.assets/image-20230405224432228.png" alt="image-20230405224432228" style="zoom:33%;" />

Chrome 多线程

每个页面单独渲染进程和插件进程，装进沙箱，不能获取系统权限

权限由浏览器主线程操作

<img src="./笔记.assets/image-20230405234925822.png" alt="image-20230405234925822" style="zoom: 25%;" />

<img src="./笔记.assets/image-20230405235048594.png" alt="image-20230405235048594" style="zoom:33%;" />

主进程：页面的展示，页面的交互，管理子进程，提供存储功能

网络进程：下载网络资源

GPU 进程：绘制网页和 UI 界面

渲染进程：js v8 引擎，排版引擎 Blink （h5 css js 转换为网页）

### 网络协议

互联网协议 Internet protocol IP

网络层协议

计算机系统通过 ip 协议在网络中互相传输数据

IP 不好记 ----> 域名

DNS 域名系统 Domain name system

找到具体服务器的系统服务器的 IP 地址，如何指定访问具体程序

​ UDP 用户数据包协议 user datagram protocol 端口号访问指定的程序 <font color=red>传输层协议</font>

​ UDP 协议发送数据时，不能保证接收端一定收到

​ TCP 数据传输（控制）协议 transmission control protocol <font color=red>传输层协议  </font>

​ 将数据拆分成数据包的形式传输

​ 数据包的丢失，提供重传的机制

​ <font color=blue>面向连接</font>，在传输数据之前，他会和目标设备进行连接，在传输完成后和目标断开连接 （创建和断开的过程，三次握手，四次挥手）

|       特性       |                           TCP                            |                                 UDP                                  |
| :--------------: | :------------------------------------------------------: | :------------------------------------------------------------------: |
|     连接方式     |                          有连接                          |                                无连接                                |
|    可靠性保证    |                      提供可靠性保证                      |                           不提供可靠性保证                           |
| 传输数据顺序保证 |                   传输数据顺序得到保证                   |                          不保证传输数据顺序                          |
| 传输数据开销较大 |                            是                            |                                  否                                  |
|     适用场景     | 适用于数据传输要求可靠的应用程序，如文件传输、电子邮件等 | 适用于数据传输速度要求快的应用程序，如在线游戏、实时视频和音频通信等 |

### TCP 🤝👋🏻

**三次握手(建立连接)**

​ 1. 客户端请求连接的请求 SYN，<font color=blue>随机起始序列号 Sequence Number</font> 给 服务端

​ 2. 服务端接受到请求，发送<u>确认信息 ACK</u>、SYN（伴随<font color=red>随机起始序列号</font>）、客户端发送来的<font color=blue><u>序列号+1</u></font>（aka<u>确认序列号 Acknowledgement Number</u>）

​ 3. 客户端收到服务器的 SYN 和 ACK，向服务器发送一个 ACK 确认信息（<font color=red>服务器随机序列号+1</font>）

通过握手过程，TCP 协议可以保证连接的可靠性和正确性，同时还可以进行流量控制和拥塞控制，以确保网络的稳定性和可靠性。

**四次挥手 （关闭连接）**

1. 🅰️ 发送一个 FIN 消息，表示没有数据传输，但 🆎 双方仍可接受数据
2. 🅱️ 接受到 FIN，向对方发送 ACK，表示已经接受到该消息
3. 🅱️ 也发送一个 FIN 消息，表示数据全部传输完毕，但 🆎 双方 🈲 不可发送数据
4. 🅰️ 收到 FIN，返回一个 ACK 消息，表示已经接受到该消息

通过挥手过程，TCP 协议可以保证数据的完整性和可靠性，在数据传输结束后及时关闭连接，释放资源，以便其他应用程序可以使用它们。

### 规定传输格式

浏览器 http 协议

http 请求流程

​ url 给我网页数据 查找缓存（存在且没过期，暂停请求 网页更快加载，减轻服务器压力

​ 没有缓存，http 做应用层协议，tcp ip 发送到网络中（之前通过域名获取 IP 地址，找到缓存，下次直接使用对应 ip,,,,,,,访问域名没有加端口，默认加:80(http 默认端口)

​ 为 TCP 建立连接做 3 次握手，完成 发送 http 请求 ||服务端接受返回数据

#### 输入 URL 到页面展示

1. 浏览器判断输入的内容，不符合 URL 规则，内置搜索引擎搜索；符合规则，加上协议

2. 请求阶段：浏览器主进程 =<u>URL(进程之间通信)</u> => 网络进程=（转圈，接受服务器返回的 http 相应头 html）,让浏览器开启一个渲染进程，发送一个提交文档命令。

   网络进程=（HTTP 文档）=> 渲染进程

​ <img src="./笔记.assets/image-20230406143157791.png" alt="image-20230406143157791" style="zoom:25%;" />

​

渲染完毕 ，接受完毕指令，当前页面白屏

<img src="./笔记.assets/image-20230406143342974.png" alt="image-20230406143342974" style="zoom:25%;" />

渲染进程联合 GPU 进程渲染，并返回

<img src="./笔记.assets/image-20230406143530487.png" alt="image-20230406143530487" style="zoom:25%;" />

网络进程 ?缓存资源(返回给浏览器主进程，中断请求)

​ 没有，进入网络流程

​ DNS 解析 ip 地址

同一站点（根域名，协议相同 ）公用一条渲染进程

![image-20230406144818544](./笔记.assets/image-20230406144818544.png)

## 15 面向对象

**类的封装、继承、多态**

- 封装：低内聚高耦合

- 多态：重载和重写

​ 重载：方法名相同，形参个数或者类型不一样

​ JS 中不存在真正意义上的重载，JS 重载指的是使用同一个方法，根据传参不同，实现不同的效果

JS 中的面向对象是基于原型和原型链的

- 继承：子类继承父类的方法

### **JS 继承：**

- **原型继承**
  - 优点：父类方法可以被复用
  - 缺点：1. 父类的**引用类型**数据会被子类共享篡改 2. <u>子类实例不能给给父类构造函数传参</u>
- **call 继承** 借用构造函数
  - 优点：父类的**引用类型**数据不会被子类共享篡改
  - 缺点：不能访问父类原型属性上的方法
- **组合继承（**原型继承和 call 继承的结合）
  - 优点：
    - 父类方法复用
    - 父类引用数据不会被子类共享篡改
    - 子类可以访问父类原型上的方法
  - 缺点：
    - 会调用两次父类的数据，会有两份一样的属性和方法，影响性能
- **寄生组合继承**
  - 优点
    - 改变以上优缺点
  - 缺点
    -
- ES6 extends、super

## 16 判断数据类型

`typeof `

Array Object null 都是返回 object 类型 (set、map 也是 object，symbol 返回 symbol)

==`Object.prototype.toString.call()` 都可以区分 [] {} null==

## 17 手写代码

### instanceof

**`instanceof`** **运算符**用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

判断继承关系，在同一条原型链上，返回 true

```js
function myInstanceof(obj1, obj2) {
  let objProto = obj1.__proto__

  while (true) {
    if (objProto === null) return false
    if (objProto === obj2.prototype) return true
    // 一层一层往上找
    objProto = objProto.__proto__
  }
}
```

### bind

![image-20230403201715838](https://s2.loli.net/2024/03/25/iGepJNhtcAM5ZsF.png)

```js
Function.prototype.myBind = function () {
  // console.log(this) //fn函数
  const fn = this

  let arg = Array.from(arguments)
  const _this = arg.shift()

  return function () {
    return fn.apply(_this, arg)
  }
}

Function.prototype.myBind = function (context, ...args) {
  const fn = this
  return function () {
    return fn.apply(context, [...args, ...arguments])
  }
}
```

### 深拷贝

-

```js
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj))
}
```

需要注意的是，在一些复杂的对象中，比如包含**函数、正则表达式、日期等类型**的对象时，使用`JSON.parse(JSON.stringify(obj))`可能会导致数据丢失或类型错误，因为**这些类型的对象不能被序列化为 JSON 字符串**。这时候需要采用其他的深度复制方法，比如手动递归复制、使用第三方库等。

- 递归
  - (没有考虑循环引用) 如果对象 A 引用了对象 B，而对象 B 又引用了对象 A，那么在进行深拷贝的时候会陷入死循环。（哈希表）

```js
function deepClone(obj) {
  if (typeof obj !== "object" || obj == null) return obj
  let res = obj instanceof Array ? [] : {}
  //  for in会遍历到obj原型链上的属性，增加判断，健壮性
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 嵌套的一层不会被克隆，所以再加一次深度克隆
      res[key] = deepClone(obj[key])
    }
  }
  return res
}
```

- `lodash.cloneDeep()`

### 扁平化数组

借助 Array.prototype.concat.apply([], arr)

## 18 预编译

### JS 预编译

js 属于解释性语言，在执行过程中顺序执行，但是会分块先预编译再执行。因此 JS 存在一种变量提升的现象。

**但是是因为有预编译才有 所谓的变量提升**

#### 函数先于变量提升

##### 变量提升

var 声明变量提升、function 声明函数提升

无论这两者声明或调用的位置是前是后，系统总是会将其提升到调用前面，因此只值为 undefined

##### JS 代码运行的三大步骤

1. 词法分析
2. 预编译
3. 解释执行

##### 暗示全局变量 imply global

任何变量，<font color=red>未经声明就赋值</font>，此变量为**全局**所有

```html
<script>
  a = 100
   console.log(a, window.a)) //100 100

   function test(){
     b = 200
   }
   test()
   console.log(b, window.b) //200 200
</script>
```

、、、、、在 ES5 下、、、、、

##### Global Object 和 Activition Object

所谓的全局作用域，局部作用域

> 1. 创建 GO 对象
> 2. 寻找变量声明 var，值设定为 undefined
> 3. 寻找函数声明，将函数名作为 GO 属性名，值为函数体

> 1. 创建 AO 对象
> 2. 寻找函数的**形参**和**变量声明**var，将变量和形参名作为 AO key，value 为 undefined
> 3. 统一形参和实参，即更改形参后的 undefined 为实参值
> 4. 寻找函数声明，将函数作为 AO key，value 为函数体

匿名函数、函数表达式不参与预编译

![image-20230330165816238](https://s2.loli.net/2024/03/25/uCr87YZGxmKEwqi.png)

![image-20230330190058698](https://s2.loli.net/2024/03/25/xetFnNd1LAZo9Ec.png)

![image-20230330190037506](./笔记.assets/image-20230330190037506.png)

ES6 新增的块级作用域

---

```js
function test(a, b) {
  console.log(a) //1
  c = 0
  var c
  a = 3
  b = 2
  console.log(b) //2
  function b() {}
  function d() {}
  console.log(b) //2
}
test(1)
```

> 预编译
>
> AO：{
>
> ​ a:~~undefined~~ 1
>
> ​ b:~~undefined~~ fn
>
> ​ c:undefined
>
> ​ d: fn
>
> }
>
> 执行
>
> AO：{
>
> ​ a:~~undefined~~ ~~1~~ 3
>
> ​ b:~~undefined~~ ~~fn~~ 2
>
> ​ c:~~undefined~~ 0
>
> ​ d: fn
>
> }

---

```js
function test(a, b) {
  console.log(a) //fn a
  console.log(b) //udefined
  var b = 1
  console.log(b) //1
  a = 2
  console.log(a) //2
  function a() {}
  var a
  b = 3
  var b = function () {}
  console.log(a) //2
  console.log(b) //fn
}

test(1)
```

> 预编译
>
> AO：{
>
> ​ a:~~undefined~~ ~~1~~ fn
>
> ​ b:undefined
>
> }
>
> 执行
>
> AO：{
>
> ​ a:~~undefined~~ ~~1~~ ~~fn~~ 2
>
> ​ b:~~undefined~~ ~~1~~ ~~3~~ fn
>
> }

---

```js
a = 100
function test(e) {
  function e() {}
  arguments[0] = 2
  console.log(e) //2
  console.log(c) // undefined
  if (a) {
    var b = 123
    function c() {} //在if语句里面不声明（块级作用域）
  }
  var c
  var a
  a = arguments[1]
  console.log(a) //2
  console.log(b) //undefined
  f = 456
  console.log(c) //undefined
  console.log(f) //456
}
var a
test(1, 2)
console.log(a) //100
console.log(f) //456
```

> 预编译
>
> AO：{
>
> ​ e:~~undefined~~ ~~1~~ fn(e)
>
> ​ b: undefined
>
> ​ c: undefined
>
> ​ a:undefined
>
> }
>
> GO:{
>
> ​ a: undefined
>
> ​ test: fn
>
> }
>
> 执行
>
> AO：{
>
> ​ e:~~undefined~~ ~~1~~ ~~fn(e)~~ 2
>
> ​ b: undefined
>
> ​ c: undefined
>
> ​ a:~~undefined~~ 2
>
> }
>
> GO:{
>
> ​ a: ~~undefined~~ 100
>
> ​ test: fn
>
> }

## 19 `for in of`

#### For in 遍历得到 key

- 可枚举的数据 (`Object.getOwnPropertyDescriptors()`)

​ 数组、字符串、对象

<font color=red>`for...in` 循环可以遍历对象的可枚举属性，包括自有属性和继承属性。但是需要注意的是，它不能保证属性的遍历顺序，而且会遍历对象的原型链上的属性，所以需要使用 `hasOwnProperty()` 方法来判断属性是否为自有属性。</font>

#### for of 遍历得到 value

- 可迭代的数据 (`arr[Symbol.iterator]()`)

​ 数组、字符串、set、map

#### For await of

遍历一组 Promise

```html
<script>
  function generatePromise(num) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(num)
      }, 500)
    })
  }

  const p1 = generatePromise(1)
  const p2 = generatePromise(2)
  const p3 = generatePromise(3)
  const p4 = generatePromise(4)

  const list = [p1, p2, p3, p4]

  // 如何一下子拿到所有结果
  /*     (async function(){
        for await(let val of list){
            console.log(val) //1,2,3,4一行一行打印
        }
    })()

    Promise.all(list).then(res=>{
        console.log(res) //[1,2,3,4] 返回数组结果
    }) */

  // 如和一步一步拿到结果
  const data = [1, 2, 3, 4, 5]
  ;(async function () {
    for (let val of data) {
      const res = await generatePromise(val)
      console.log("??", res)
    }

    /* const res1 = await generatePromise(data[0])
        console.log(res1)
        const res2 = await generatePromise(data[1])
        console.log(res2)
        const res3 = await generatePromise(data[2])
        console.log(res3)
        const res4 = await generatePromise(data[3])
        console.log(res4)
        const res5 = await generatePromise(data[4])
        console.log(res5) */
  })()
</script>
```

## 20 JS 类型转换

js 是动态类型语言，我们在定义一个变量其实并没有指定这个变量到底属于那种类型，只有到程序执行阶段才确定当前数据类型。

而<font color=red>各种**运算符**对数据类型是有要求的</font>，所以就会触发类型转换机制（no matter 人为 or 隐式触发）

### 显示转换：

​ 通过 JS 内置的函数**明确转换的数据类型**

- Number

- parseInt(string, ?进制) 比 Number 宽松，一位一位解析遇到不能解析的停止
- String
- Boolean

### 隐式转换：

​ 运算操作符两边数据类型不一致（比较运算符、算术运算符）

- 转为 Boolean (需要布尔值的地方，借助的`Boolean()`函数)

  - falsely 变量：`undefined`、`null`、`false`、`+/-0`、`NAN`、`""`

- 转为 String （复合类型--->原始类型---->字符串）
  - 常发生在 `"5" + xxx` 加法运算符号
- 转为 Number
  - 除了加法运算符号，其他都有可能

### other

> `=== `在不进行类型转换情况下，双方的类型与值都相等

## 21 JS 事件机制

事件

就是 html 文档与浏览器的一些交互操作，使网页具备互动性

由于**DOM**是一个**树**结构，如果在**父子节点绑定事件**时候，当触发**子节点**的时候，就存在一个**顺序问题**，这就涉及到了**<u>事件流</u>**的概念

**_捕获 目标 冒泡_**

<img src="https://s2.loli.net/2024/03/25/7IP1hs6lGbOHMjW.png" alt="image-20230518210314566" style="zoom:50%;" />

```js
addEventLisener("click", () => {}, false) //默认false，不在捕获阶段执行

event.preventPropagation() //阻止在冒泡阶段执行
```

click 事件 冒泡

## 22 JS 异步机制

### JS 异步编程

单线程

DOM 操作，必须要使用单线程模型，否则出现多线程同步问题（一个线程增，一个线程删，麻烦）

假如说只用同步来解决，即排队执行代码，那么则简单、安全。但是遇到一段非常耗时的代码段，导致整个程序被拖延，导致假死（阻塞）情况。所以 JS 带来<u>同步</u>模式<u>异步</u>模式来解决此问题。

- 同步模式与异步模式
- 事件循环与消息队列
- 异步编程几种方式
- Promise 异步方案、宏任务/微任务队列 （ES2015）
- Generator 异步方案（ES2015），Async/Await 语法糖（ES2017）

### 同步模式与异步模式

**<font color=red>同步模式</font>**

<u>排队一步一步执行</u>

开始执行 JS 引擎将全部代码加载进来，`call stack`压入一个`(anonymous)` （全部代码放入匿名函数中执行，`call stack`是调用栈，函数声明不会压入栈中）

**<font color=red>异步模式</font>**

不会等待这个任务结束，才开始下一个任务。开启过后，立即开始下一个任务。后续逻辑一般会通过**回调函数**的方式定义

_代码执行顺序混乱？？_

倒计时器根本不会管 call stack 和 Queue，放在 WebAPI 中

call stack 正在执行的工作表

Queue 待办工作表

Event Loop 监听 call stack 和 Queue，

一旦 call stack 任务结束，事件循环会从 Queue 中取出第一个回调函数压入到 call stack

<img src="https://s2.loli.net/2024/03/25/icy4GVaP2uew3fp.png" alt="image-20230403162215658" style="zoom:50%;" />

<img src="https://s2.loli.net/2024/03/25/Nqmv6TrSUJFxVjO.png" alt="image-20230403162615098" style="zoom:50%;" />

<img src="https://s2.loli.net/2024/03/25/8NHPdJlVshjn1wz.png" alt="image-20230403162834422" style="zoom:35%;" />

JS 单线程，是指执行代码是单线程的，但浏览器并不是单线程。

而我们所说的同步异步问题是指**运行环境提供的 API 是以同步或异步模式的方式工作**

### 回调函数

所有异步编程方案的根基

理解为：`一件你想要做的事--->但不知道这件事所依赖的事情什么时候结束--->交给异步任务执行者，他知道`

​ **调用者定义 交给执行者执行**

_<font color=gray>事件机制、发布订阅基于回调</font>_

### Promise

一种更优秀的异步编程统一方案（CommonJS 社区提出，被纳入 ES2015）

![image-20230329145634209](https://s2.loli.net/2024/03/25/L3nQBlK6ejMhGgy.png)

明确成功或失败的<u>结果</u>（不可被修改）后，都会有相应的任务自动执行。

#### 链式调用（不是通过函数 return this 达到的链式调用）

`.then( )方法` 返回的是全新的 Promise 对象 （返回

每一个 then 方法实际上都是为上一个 then 返回的 Promise 对象添加状态明确的回调

- Promise 的 then 方法返回一个<font color=yellowgreen>全新</font>的 Promise 对象（所以可以采用.then 链式调用）

  - .then 后面会返回一个新的 promise 实例，又可以继续调用 then 或者 catch 方法

- 后面的 then 方法就是在为上一个 then 方法返回的 Promise 对象注册回调（添加状态明确的回调）
- 前面 then 方法 return 的返回值会作为下一个 then 方法回调的参数
  - 如果 return 的是 Promise 对象，那么后面的 then 方法会等待到他的结束

#### 异常处理

两个回调`(onFulfilled，onRejected)` （只能在当前 then 方法调用中接住）

.catch （相当于两个回调 其中`(undefined，onRejected)` ，异常可以被传递，而被 catch 接住

unHandledRejection（全局）

#### 静态方法

`Promise.reslove()` 将一个值快速转换为 Promise 对象

用此方法包装一个 Promise 对象得到的还是原本的 Promise 对象

```js
let p = new Promise((resolve, reject) => {
  resolve("111")
})

let p2 = Promise.resolve(p)

console.log(p === p2) //true
```

如果用此方法，包装如下带有 then 方法的对象 **(thenable 可以被 then 的对象)**

```js
Promise.resolve({
  then: (onFulfilled, onRejected) => {
    onFulfilled("qqqq")
  },
}).then((res) => {
  console.log(res) //qqqq
})
```

`Promise.reject()` 无论传什么参数，都会作为 Promise 失败的理由

#### Priomse 并行执行

在页面请求多个接口的情况，这些请求没有过多依赖，那么就可同时请求

怎么知道这些请求完成？传统用<u>计数器</u>

`Promise.all()` 多个 Promise 集中管理 （同步执行多个 Promise 的方式）等待所有 Promise 结束后

```js
Promise.all([xxx, yyy]) // xxx,yyy为Promise对象
```

`Promise.race()` 只会等待第一个完成的任务，就结束

#### Promise 执行时序 宏任务 vs 微任务

Promise 执行时序有点特殊

回调队列 Queue 中的任务 <font color=red>**『宏任务』**，宏任务执行过程期间可能会加上额外的需求</font>，

​ 这些需求可以<u>选择</u>作为<u>_新的宏任务_</u>进入到队列 Queue 中排队

​ 也可以作为当前任务的<font color=red>**『微任务』**，直接在当前任务结束之后立即执行</font>，而不是进入队列排队

**Promise 回调作为*微任务* 执行的** （本轮结束的末尾，自动执行，不进入队列）

​ 如果处于 pending 状态就不能进入这一次微任务

​ await 后面的语句加入到微任务队列

微任务：提高整体的响应能力

> 目前绝大多数异步调用都是作为宏任务执行
>
> 微任务：Promise、mutationObserver 以及 Node 中的 process.nextTick

### Generator 异步方案

> Promise 处理异步任务的串联执行，仍然有大量的回调函数，虽然没有嵌套，影响阅读
>
> <img src="https://s2.loli.net/2024/03/25/L257BeUSh9HpoNy.png" alt="image-20230329205937527" style="zoom:35%; float:left;margin-left:30px"  />
>
> 传统异步模式，更简洁，更利于阅读
>
> <img src="https://s2.loli.net/2024/03/25/X56H7Zl3Ns2MOwE.png" alt="image-20230329210419648" style="zoom:30%;float:left" />

#### Generator ES2015

Generator 函数返回的遍历器对象都有一个 throw 方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。外面 try catch 捕获

生成器函数执行器

```js
function* main() {
  try {
    const api = yield ajax("/api.json")
    console.log(api)
    const api2 = yield ajax("/api2.json")
    console.log(api2)
  } catch (e) {
    console.log(e)
  }
}

/* const g = main()

    handleResult(g.next())
    // 生成器函数执行器
    function handleResult(result) {
        if (result.done) return
        result.value.then(data => {
            handleResult(g.next(data))
        },
            error => {
                g.throw(error)
            }
        )
    } */

co(main)
// 生成器函数执行器
function co(generator) {
  const g = generator()

  handleResult(g.next())
  function handleResult(result) {
    if (result.done) return
    result.value.then(
      (data) => {
        handleResult(g.next(data))
      },
      (error) => {
        g.throw(error)
      }
    )
  }
}
```

<u>异步调用扁平化</u>

### Async Await

ES2017

不在需要类似 co 的执行器，语言层面的标准一步语法

async 函数返回的是一个 Promise 对象，利于控制整体

```js
console.log("start") //1
setTimeout(() => {
  //宏任务1
  console.log("children2")
  Promise.resolve().then(() => {
    console.log("children3")
  })
}, 0)

new Promise(function (resolve, reject) {
  console.log("children4") //2
  setTimeout(function () {
    //宏任务
    console.log("children5")
    resolve("children6")
  }, 0)
}).then((res) => {
  // 微任务
  console.log("children7")
  setTimeout(() => {
    console.log(res)
  }, 0)
})
// start children4 children2 children3  children5  children7 children6

//juejin.cn/post/6950786264941461541
```

#### async/await 执行顺序

我们知道`async`隐式返回 Promise 作为结果的函数,那么可以简单理解为，await 后面的函数执行完毕时，await 会产生一个微任务(Promise.then 是微任务)。

但是我们要注意这个微任务产生的时机，它是执行完 await 之后，直接跳出 async 函数，执行其他代码(此处就是**协程**的运作，A 暂停执行，控制权交给 B)。

<font color=red>其他代码执行完毕后，再回到 async 函数去执行剩下的代码，然后把 await 后面的代码注册到微任务队列当中</font>。我们来看个例子：

```js
console.log("script start")

async function async1() {
  await async2()
  console.log("async1 end") //1微任务
}
async function async2() {
  console.log("async2 end")
}
async1()

setTimeout(function () {
  console.log("setTimeout")
}, 0)

new Promise((resolve) => {
  console.log("Promise")
  resolve()
})
  .then(function () {
    console.log("promise1")
  })
  .then(function () {
    console.log("promise2")
  })

console.log("script end")
// 旧版输出如下，但是请继续看完本文下面的注意那里，新版有改动
// script start => async2 end => Promise => script end => promise1 => promise2 => async1 end => setTime
```
