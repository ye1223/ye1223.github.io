---
title: js红宝书
categories:
  - []
tags:
  - JavaScript
wordCount: 2517
charCount: 52602
imgCount: 10
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 12 minutes
date: 2023-08-04 12:12:52
featured_image:
---

### `<script>`标签

* 一些属性
  * defer 推迟
  * async 异步  期间不要动DOM
  * intergrity 检查安全

* src GET 跨域
* 引入外部文件文件，行内代码写了没用

> MIME 代码块中的脚本语言内容类型

* `<noscript></noscript>`





### 严格模式（ES5增加）

遵守的是es3的语法

开启严格模式，脚本开头写上`"use strict";` ，也可写在函数内部开头，单独开启严格模式



### 变量

​	`var`变量提升hoist ，函数作用域,全局声明成为window对象属性

​	`let`，块级作用域，暂存性死区（temporal dead zone)

​	`let`和`var` 只是指出变量在相关作用域如何让存在

​	`const` 适用于for-in(对象属性名)、for-of（数组）

##### 	声明风格

​		不使用`var`，` const`优先，`let`次之



### 3.4 数据类型

​	**Undefined、Null、Boolean、Number、String、Symbol、Object**



​	**typeof 操作符**

​		typeof null => onject (null被认为是空对象的引用)

​	**Number**
​		八进制: 0111 在严格模式下报错、0o111可以

​		`1.` 和 1.0 为整数，1.1、1.2浮点数

​		ES会将小数点后至少包括<u>6个0</u>的浮点值转换成科学计数法

​	**String**

​		ECMAScript中的字符串是**不可变的immutable**

​		`toString`  返回自身的一个副本

​			除了<font color=red>`null`</font> 和<font color=red> `undefined`</font>都有`toString`方法。对于数值类型可以传参(转换<u>进制数</u>)

​		对于变量未知类型的，转为字符串，利用**`String()`转型函数**

​		**模板字面量 ``**

​			保留换行字符，跨行定义字符串

​			字符串插值 ``${js表达式}` ，调用了toString方法强制转换为字符串

​			标签函数`(模板字面量隔开形成的字符串数组，...模板字面量的值)`		

​			原始字符串`String.raw`

```js
console.log(`\u00A9`) //© (输出转义后的符号
console.log(String.raw`\u00A9`) //\u00A9 (输出原始的字符串
```



​		**Symbol**

​			在编程中一个幂等操作的特点是其任意多次执行所产生的影响均与一次执行的影响相同

​			全局符号： 

​				symbol.for()执行幂等操作，第一次操作会检查全局运行时注册表，是否存在对应的symbol.for()

​				查询全局注册表`Symbol.keyFor(参数)`  (参数必须为symbol类型，否则抛出typeError)

​					查询成功返回symbol值，否则返回undefined

​			作为对象`[属性]`使用

​				defineProperty添加symbol类的值作为属性（在node下不能）

​			内置符号（不可写，不可枚举，不可配置）

​				`Symbol.iterator` for-of循环会使用这个属性

​				`Symbol.asyncIterator` for-await-of使用，异步迭代器

​				`Symbol.hasInstance`定义在Function的原型身上

​				`Symbol.isConcatSpreadable`（默认值undefined） **控制Array.prorotype.concat是否“打平”连接**

​					对于数组对象(默认打平），设置为falsely变量，不会打平连接，会让整个对象追加到数组末尾

​					对于类数组对象（默认不打平），设置为truely变变量，会打平连接，到数组实例

​			**Object**

​				ECMAScript只要求给构造函数提供参数时使用括号

```js
const obj = new Object //合法，但不推荐
```

​				`hasOwnProperty` 

​				`valueof()` 定义在对象里的方法。 return的值，

​						显示获取：`obj.valueOf()`

​						隐式获取：`++obj ` 对obj进行**数学运算**，直接调用的`valueOf()`方法

### 3.5 操作符

​	一元操作符

​		自增、自减、+、-

​			`+` 拼接字符串，`-` 先`Number()`转换，在减法操作

​	位操作符

​		ECMAScript 数值 以IEEE 745 64位格式存储

​		位操作，将值转为32位整数，再进行操作，最后再把结果转为64位 

········



### 3.6 语句

​	`for-in` 严格迭代语句，遍历对象的<u>可枚举</u><u>非symbol类型</u>属性

​	`for-of` 严格迭代语句，遍历可迭代对象

​	标签语句 配合continue和break使用

​	`with`语句，将代码的作用域设定为特殊的对象

```js
const a = process.pid
const b = process.ppid
const c = process.title
console.log(a, b, c)

with(process){
    console.log(pid, ppid, title)
}
//严格模式不支持with，影响性能，难于调试，不推荐使用
```

​	switch 全等操作符，不会强制转换类型



## 第四章 变量、作用域与内存

### 4.1 原始值与引用值

​	原始值（基础数据类型值）

​	引用值，保存在内存中的对象

​	ECMAScript函数参数传递是**按值传递**，（传递的是值的副本而非值本身）

​	确定类型： 

​		**`instanceof`** **运算符**用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上

​		`typeof`....etc

### 4.2 执行上下文与作用域

​	`var`的函数作用域声明：变量会添加到**最近的上下文**，*作用域提升*

​	`with`语句中var声明的变量为在**全局**作用域中

​	`let`、`const`  {}块级作用域

> 使用`const`变量有助于JS引擎(谷歌V8引擎)优化，**编译时**就将所有实例替换成**实际的值**，而不会通过查询表进行变量查找

​	标识符查找：沿作用域链查找

### 4.3 垃圾回收♻️

​	基本思路：确定那个变量不会再使用，然后释放他占用的空间。（**周期性自动运行**）

​	主要标记策略

​		**标记清理**：

​			标记内存中的所有变量 ➡️ 去掉上下文或者上下文引用的变量的标记 ➡️ （这时候再被加上标记的变量就是待删除的了，任何上下文的变量都访问不到它们了）回收

 		**引用计数**(❗️导致循环引用)

​			变量声明并赋引用值，引用数为1；

​			如果同一个值又被赋值给另一个，引用数+1；

​			该引用值被其他值覆盖，则引用数-1。

​			当引用值为0时，就会被垃圾回收

​	将变量设为`null`，切断变量与其之前引用值之间的关系。

​	**性能**： 

​		现代垃圾回收程序根据**运行时的环境**来决定何时运行（以前IE是达到设定的阈值，就执行回收）

​		也有浏览器提供方法可以<u>主动触发垃圾回收</u>（❌）

​		提升性能：

​			使用`const let`声明变量：相对于函数作用域的`var`，块级作用域**可能**会更早终止，会让垃圾回收程序接介入，尽早回收内存

​			隐藏类和删除操作`delete`

​				V8引擎将JS**代码**编译成实际的**机器码**会利用”隐藏类”，<strong style="color:red">能够共享隐藏类的对象性能更高</strong>

​					**动态<font color=blue>添加、删除</font>属性导致不共享一个隐藏类**

```js
function Foo(){
    this.name = 'iam foo'
}
// 两个实例共用一个相同的隐藏类，因为两个实例共享一个共同的构造函数
let f1 = new Foo()
let f2 = new Foo()

//‼️但是如此修改（这种操作的频率和隐藏类的大小对性能产生明显影响
f2.name = 'iamf2'//Foo实例对应两个 不同 的隐藏类。
```

```js
//✅解决方案： 先创建再补充
function Foo(name){
  this.name = name
}
//这样对应相同的隐藏类
let f1 = new Foo('iamf1')
let f2 = new Foo()

//‼️但是，使用delete关键字动态删除属性导致，不再共享一个隐藏类
delete f1.name

//✅解决方案 把不想要的属性设置为null
f1.name = null
```



​		内存泄漏

​			意外声明全局变量

​			定时器回调引用外部变量

​			使用闭包

​		**静态分配与对象池**





## 第五章 基本引用类型

### 5.1 Date

> UTC 协调世界时 GMT格林威治平时

​	

​	`Date.parse(创建的是本地日期)`  `Date.UTC(创建的是GMT日期)`  将一个表示日期的字符串解析为对应的时间戳（毫秒数）

​	new Date(传入时间字符串) ，根据字符串格式隐式调用上面两个构造函数

​	Date类重写了toLocaleString toString valueOf(返回时间戳)

### ❓5.2 正则

### 5.3 原始值包装类型

> ECMAScript提供了3种特殊的引用类型，`Boolean` `String` `Number`

​	正常来说原始值本身不是对象，按逻辑上不应该有方法

```js
let s1 = 'some text'
console.log(s1.substring(2)) //me text
```

> 包装类型让原始值拥有对象行为
>
> ​	创建一个String类型实例
>
> ​	调用实例上的方法
>
> ​	销毁实例

```js
// 显示创建原始值包装类型(不推荐)
const s = new String('i am a string')//构造函数
console.log(typeof s) //object

const s = 'i am a string' 
console.log(typeof s) //string

const s = String('i am a string') //转型函数
console.log(typeof s) //string
```

```js
// object工厂方法创建

const os = new Object('i am a string')
// 创建的是一个String实例
console.log(typeof os, os instanceof String)// object true
```



#### 	number

​		toFixed方法，返回的字符串保留几位小数（0~20位+，超过，四舍五入）

```js
const num = 10
console.log(num.toFixed(2))//10.00

console.log(Number(10.005).toFixed(2))//10.01
```

​	toPrecision返回最合理的数值(1-21小数位)

#### 	String

​	JS 一个字符16位

​	与模式匹配相关的方法: match search replace split



### 5.4 单例内置对象

#### Global

​	eval函数（⛔️XSS攻击）

​		这个函数就是个完整的ECMAScript解释器

​		定义在eval函数中的变量和函数，不存在函数提升

​		开启严格模式够，外部访问不到eval函数里的数据

### Math 



## 第六章 集合引用类型

### 6.1 Object

​	字面量{}形式创建一个对象，并不会new Object()

### 6.2 Array

​	字面量[]形式创建一个数组，也不会new Array()

```js
const arr = new Array(20) //定义数组长度为20
```

|              |                        |                                  |
| :----------- | ---------------------- | -------------------------------- |
| Array.from() | 将**伪数组**转为真数组 |                                  |
| Array.of()   | 将一系列参数转为数组   | `Array.of( 1, 2, 3)` ➡️ [1, 2, 3] |



#### 	数组空位

​		字面量一串**逗号形式**创建空位

​			<img src="JS红宝书.assets/image-20230616144658231.png" alt="image-20230616144658231" style="zoom:50%;" />

​		ES6新增的方法和迭代器，将空位当成存在的元素，值为`undefined`

​			<img src="JS红宝书.assets/image-20230616145322099.png" alt="image-20230616145322099" style="zoom:50%;" />

​		ES6之前的方法， 忽略空位（具体的行为因方法而异）

​			`map`跳过空位，`join`将空位视为空字符串

​		<img src="JS红宝书.assets/image-20230616145451935.png" alt="image-20230616145451935" style="zoom:50%;" />

#### 	数组索引

​		<font color=red>数组的length不是只读的</font>，利用这个特性删除数组末尾元素（当然也可添加数组空位）

​		<img src="JS红宝书.assets/image-20230616145938663.png" alt="image-20230616145938663" style="zoom:50%;" />

#### 	检测数组

​		在一个全局上下文中，使用`instanceof`。

> 多个 iframe多个全局上下文。然后每个里面都有 Array 这个对象。他们并不相等。
>
> 本质来讲 `instanceof` 是去找 prototype 之类的，看看是否有继承。

​		**`Array.isArray()`** 

#### 	迭代器方法

​		`arr.keys()` 返回数组索引的迭代器

​		`values() `返回数组元素的迭代器

​		`entries()` 返回 索引/值 对的迭代器

​	***<u>alert期待字符串</u>***

#### 	**排序方法**

​		`sort` ，事先对数组中的没项元素都使用的`String转型函数`

​			升序：compare(val1, val2) val1 > val2 return 1

​			降序：compare(val1, val2) val1 > val2 return -1

#### 操作方法

​	`concat` `splice`

#### 搜索方法

​	`indexof` `lastIndexOf` `includes`

​	`find` `findIndex`

#### 迭代方法

​	`every` `some`

​	`filter` `map` `forEach`

#### 归并方法

`reduce` `reduceRight`

### 6.3 定型数组



### 6.4 Map Set

​	定义时都接受一个可迭代对象初始化映射

​	使用`forEach`，`for-of`迭代值

### 6.5 weakMap weakSet 

> ​	weakMap存储键值对的**键**必须为引用类型数据

如果键的指向为空，自动称为垃圾回收的目标

weakMap实现真正的私有变量

都不可迭代值

### 6.8 迭代与扩展操作

​	定义的默认迭代器（`Array` `定型数组` `Map` `Set`）

​	支持for-of顺序迭代、兼容扩展操作符

> 浅复制：只会复制对象的引用



## 第七章 迭代器与生成器

​	迭代：按顺序反复执行一段程序

​	循环是迭代的基础	

### 迭代器模式

​	开发者无需知道如何迭代就能实现迭代操作

​	实现可迭代iterable接口的对象，都能被事件iterator接口的结构消费

​	**内置iterable接口的类型：`Stirng` `Array ` `Map` `Set` `arguments对象` `NodeList等DOM集合类型`**  

​	**接受可迭代对象的原生语言特性** 

​		`for-of ` `数组解构` `扩展操作符`

​		`Array.from` `new Set/Map` 

​		`Promise.all / race` `yeild*操作符`

*给对象添加可迭代的iterable接口，*

  ```js
let obj = {
    list: [1, 2, 3, 4],
    [Symbol.iterator]() {
        let count = 0
        const { length } = this.list
        return {
            next: () => {
                return { done: count === length ? true : false, value: this.list[count++] }
            },
          	//提前终止，调用return方法
          	return() {
            	console.log('Exiting Early!')
           	 	return { done: true }
         	 	}
        }
    }
}
  ```

接收迭代器实例

```js
const iter = obj[Symbol.iterator]()
console.log(iter.next()) //调用next方法
console.log(iter.next())
```

> 不同迭代器实例之间没有联系
>
> 迭代器不与对象某时刻的快照绑定，也可根据实际情况动态变化
>
> 迭代器维护一个指向可迭代对象的引用，⛔️**阻止**垃圾回收可迭代对象



#### 	**提前终止迭代器**

​	如上`return`方法指定迭代器提前关闭时执行的逻辑

​		**for-of 循环 在 break continue return throw时，触发提前终止逻辑**

```js
for (let val of obj){
    if(val === 3){
        break
    }
    console.log(val)
}
// 1
// 2
// Exiting Early!
```

​		 **解构操作并未消费所有的值时**	

```js
const [item1, item2] = obj
console.log(item1, item2)
// Exiting Early!
// 1 2
```

> 如果迭代器没有关闭，可以从上次离开的地方继续迭代（数组的迭代器不能关闭）
>
> return() 方法是可选的
>
> 仅仅给一个不可关闭的迭代器器增加一个return方法并不能让他关闭。调用return方法并不会强制迭代器进入关闭状态。



### 生成器模式

> 临时的可迭代对象称为生成器

​	生成器拥有在<u>函数块</u>内**暂停**和**恢复代码**执行的能力 <font color=gray>可以用于自定义迭代器和实现协程</font>

#### 💬定义生成器

​	函数名前加『 *』，箭头函数不可用于定义生成器函数

```js
function * generatorFn(){}
//函数表达式方式 对象方法 类方法 类静态方法。。。
```

​	调用生成器函数产生生成器对象

```js
const g = generatorFn()
console.log(g)//
```

<div style="text-align:center; padding: 0 100px; overflow: hidden;">
  <div style="float: left; font-weight: bold;">开始暂时处于暂停执行状态，生成器对象也实现了iterator接口，具有next方法</div>
  <img src="JS红宝书.assets/image-20230617162726562.png" alt="image-20230617162726562" style="zoom:50%; display:block; float: right;" />
</div>


初次调用next()方法指明开始调用生成器

value属性是生成器返回值，默认undefined



#### :stop_sign:yeild中断执行

yeild关键字只能在生成器函数中使用



#### 作为可迭代对象使用

生成器显示调用next方法用处不大。

将生成器对象作为可迭代对象使用

```js
function *genneratorFn(){
    yield 1
    yield 2
    yield 3
}
for(let val of genneratorFn()){
    console.log(val)
}
```



#### yeild实现输入输出

yeild产出的值传给g.next()

g.next()传入的参数，作为是yeild的返回值



#### 产生可迭代对象

```js
// ...
yeild *[1, 2, 3]
// ...
```

yeild* 其实也就是将可迭代对象序列化为一串可以单独产出的值

```js
function* genneratorFnA(){
    for (const val of [1, 2, 3]){
        yield val
    }
}
// 等价！！！
function* genneratorFnB(){
    yield* [1, 2, 3]
}
```

#### ✅作为默认迭代器使用

```js
class Foo {
    constructor(){
        this.list = [1, 2, 3, 4]
    }
    * [Symbol.iterator](){
        yield* this.list
    }
}
const f = new Foo()
for(const val of f){
    console.log(val)
}
```



#### 提前终止生成器

`return() `强制生成器进入关闭状态

<img src="JS红宝书.assets/image-20230617170419253.png" alt="image-20230617170419253" style="zoom:50%;" />



`throw()` 将一个错误注入到生成器中

| 如果生成器没处理这个错误，生成器会closed                     | 生成器内部处理了这个错误生成器就不会关闭，而且会恢复执行<br />（只是跳过了这个值） |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="JS红宝书.assets/image-20230617170721610.png" alt="image-20230617170721610" style="zoom:50%;" /> | <img src="JS红宝书.assets/image-20230617171023335.png" alt="image-20230617171023335" style="zoom:50%;" /> |



## 第八章 对象、类与面向对象编程

### 8.1 对象

#### 定义

​	构造函数、字面量形式

#### 对象属性

##### 数据属性（默认都为true)

| `Configurable`                | <span style="font-weight: normal;">属性可由delete删除</span> |
| :---------------------------- | ------------------------------------------------------------ |
| <strong>`Enumerable`</strong> | 是否可由`for-in`枚举                                         |
| <strong>` Writable`</strong>  | 是否可被修改                                                 |
| <strong>` Value`</strong>     | 包含实际值，默认undefined                                    |

​	**使用`Object.defineProperty`对对象属性属性修改（不配置值，默认为fasle）**

<img src="JS红宝书.assets/image-20230617174454670.png" alt="image-20230617174454670" style="zoom:50%;" />

(严格模式下： 尝试对configurable: false; witable: false；的值修改，会抛出错误。)

不能对同一个属性，定义多次Object.defineProperty()

##### 访问器属性

`getter` ` setter`

```js
const obj = {
        _name: 'levy_init'
    }
    Object.defineProperty(obj, 'name', {
        get() {
            return this._name
        },
        set(newVal) {
            console.log('尝试修改name')
            this._name = newVal
            console.log('修改成功')
        }
    })

    const obj2 = {
        _name: 'levyy',
        get name() {
            return this._name
        },
        set name(newVal) {
            console.log('尝试修改name')
            this._name = newVal
            console.log('修改成功')
        }
    }
```

**`Object.defineProperties` 对一个对象的多个属性一次性进行描述符规定**

```js
    const obj = {
        _name: 'levy'
    }
    Object.defineProperties(obj, {
        name: {
            get(){
                return this._name
            }
        },
        age: {
            value: 21
        }
    })
```

#### 读取属性特性

​	读取对象某一个： `Object.getOwnProperty(obj, 'aProperty')`

​	读取对象全部属性的特性：`Object.getOwnProperties(obj)` //其实也是对每个属性调用了上面的方法，在一个新对象返回



#### 合并对象（混入）

`Object.assign()`浅复制，只复制可枚举(PropertyIsEnumerable)、自身(hasOwnPropery)属性

不复制属性的getter setter

> 没有回滚之前赋值的状态，尽力赋值

#### 相等判断

`Object.is()` 

<img src="JS红宝书.assets/image-20230617185929270.png" alt="image-20230617185929270" style="zoom:50%;float:left;" />

#### 增强语法

属性值简写、可计算属性、方法简写、对象解构



### 8.2 创建对象

> ES6 Class Extends也是基于ES5原型链继承的语法糖

#### 工厂模式

```js
function createPerson(name, age){
    const o = new Object() //显式创建对象
    o.name = name
    o.age = age
    o.sayName = () => {
        return o.name
    }
    return o //return 对象
}
const p1 = createPerson('小明', 10)
```

#### 构造函数模式

```js
function Person(name, age){
    this.name = name
    this.age = age
    this.sayName = () => {
        return this.name
    }
}
const p2 = new Person('小红', 10)
```

> new操作符
>
> 1. 内存中创建一个新对象
> 2. 这个新对象的内部的[[Prototype]]属性赋值为构造函数的prototype属性
> 3. 构造函数内部的this被赋值为这个新对象（this指向新对象）
> 4. 执行构造函数内部代码（给新对象添加属性）
> 5. 如果构造函数返回**非空对象**，则返回该对象；否则，返回刚创建的对象
>
> ```js
> const myNew = (constructor, ...args) => {
>     const o = new Object()
>     o.__proto__ = constructor.prototype
>     const res = constructor.apply(o, args)
>     return res instanceof Object ? res : o
> }
> ```

​	构造函数如果不传参可以不用写括号

##### 问题

​	不同实例上的方法不是同一个，方法都是做同样的事，没必要定义两个不同的Function实例。

​	解决可以把方法定义在构造函数外部，构造函数内部方法直接引用

#### 原型模式

在构造函数原型定义的属性方法可以被实例共享

```js
function Person(){
    Person.prototype.name = '小小'
    Person.prototype.sayName = () => {
        return this.name
    }
}

const p = new Person()
console.log(p.name, p.sayName)

// 简化写法：
function Person(){
  Person.prototype = {
    name: '小小',
    sayName(){
      return this.name
    }
  }
}
Object.defineProperty(Person.prototype, 'constructor', {
  value: Person,
  enumerable: false //原生constructor不可枚举，用这种方式定义
})
```

`isPrototypeOf 检查原型` 原型链

```js
Person.prototype.isPrototypeOf(p) //true
```

`Object.getPrototypeOf()` 获取原型对象

```js
Object.getPrototypeOf(p) === Person.prototype //true
```

**更改原型对象**

​	`Object.setPrototypeOf()` 影响性能

​	`Object.create`

```js
let a = {
    name: 'a'
}
let b = {
    age: 18
}
console.log(b)
Object.setPrototypeOf(a, b)
console.log(Object.getPrototypeOf(a) === b) //true
console.log(a.__proto__ === b) //true

let c = Object.create(a)
console.log(Object.getPrototypeOf(c) === a)//true
console.log(c.__proto__ === a)//true
```



实例可以通过原型链查找属性

确定属性在自身还是是原型链上的 `hasOwnProperty()`

`in` 操作符是在自身以及原型链上查找



`Object.keys()` 遍历实例可枚举属性

`Object.getOwnPropertyNames()` 遍历实例无论是否可枚举属性（除了Symbol ）

`Object.getOwnPropertySymbols()`

遍历顺序

for-in Object.keys() 无序



#### 对象迭代

`Object.values() ` `Object.entries() `

浅复制对象、不迭代Symbol



### 8.3 继承

[参考掘金](https://juejin.cn/post/6844903696111763470)

#### 原型链继承

每个构造函数有一个<font color=red>原型</font>对象prototype， 这个<font color=red>原型</font>对象有个属性constructor指向构造函数本身。

而这个构造函数实例有一个内部指针`__proto__`，指向这个<font color=red>原型</font>

那如果这个原型是另一个类型的实例，就意味着这和<font color=red>原型</font>本身有个指针指向<font color=blue>另一个原型</font>，相应另一个原型也有个指针指向另一个构造函数。

这样形成了实例与原型之前的**原型链**

#### 盗用构造函数

#### 组合继承

#### 原型式继承

#### 寄生式继承

#### 寄生组合继承

寄生式继承父类原型，然后将返回的对象赋值给子类原型

#### 混入式继承

#### Class 继承



### 8.4 类

定义方式：

```js
// 类声明
    class Foo{}
// 类表达式
    const Foo1 = class FooName{ //表达式类名FooName可选
        identify(){
            console.log(Foo1.name ,Foo2.name) //name字段获取类名
          	// class后定义了类名就是指定类名FooName，否则类名就是Foo1
        }
    }
```



类是一种特殊的函数`typeof`，但是并不会有作用域提升

类声明受块级作用域影响，而函数生命则受函数作用域影响



#### `constructor`构造函数

构造函数默认返回this，**构造函数返回的对象用作实例化的对象**

如果这个构造函数返回的不是this对象，而是其他对象，那么通过`instanceof`操作符不会检测出这个对象与这个类有关。

> 因为在`new`操作时，会自动绑定this，如果

```js
class Foo {
    constructor(override) {
        this.foo = 'foo'
        if (override) {
            return {}
        }
    }
}
const foo1 = new Foo()
console.log(foo1 instanceof Foo) // true
const foo2 = new Foo(true)
console.log(foo2 instanceof Foo)// false
```





类中定义的方法成为原型方法

***类块*中定义的方法都会定义在类的原型上**



静态类方法

迭代器、生成器



#### 继承

`extends` 继承一个类或者一个普通的构造函数

super只能在**派生类构造函数和静态方法**中使用

调用super()函数会调用父类构造函数，并将返回的实例赋值给this

给父类传参，super()手动传参

在类构造函数中不能在super()之前调用this

在派生类中显示定义了构造函数，必须要调用super或者返回一个对象



#### 抽象基类

供其它类继承，却不被实例化

利用new.target实现

```js
class Foo{
  constructor(){
    console.log(new.target) //返回Foo类
    if(new.target === Foo){
      throw new Error('Foo 不能直接被实例化')
    }
  }
}
let f = new Foo() //报错
```





## 第九章 代理与反射

### 9.1 代理

用作目标对象的替身，但独立于对象

```js
new Proxy(target, handlerObj) //参数两者缺一不可
```

`Proxy.proptotype` 为undefined，所以不能使用`instanceof`操作符

`===`严格相等可以用来区分代理和目标

#### 捕获器

`get`

​	接受参数（目标对象，要查询的属性，代理对象)	

重建被捕获的原始行为：





使用捕获器，被代理的属性如果同时not Configurable and not Writable，则TypeError报错



> 反射API Reflect
>
> delete函数属性--->Refelect.deleteProperty
>
> name in obj ---> Reflect.has(obj, 'name')



可撤销代理

**撤销函数**和**代理对象**是同时在实例化时生成的

```js
const obj = {
    foo: 'bar'
}

// 解构 代理对象和撤回函数
const { proxy, revoke } =Proxy.revocable(obj, {
    get(){
        return '1234'
    }
})
console.log(proxy.foo)//1234
revoke() //撤销代理

console.log(obj)
console.log(proxy)//error
```



#### 实用反射Reflect API

反射API不局限于捕获程序处理

代替Object上的方法（错误必须try catch捕获 到 反射API返回布尔值）

> 反射方法return 的值称为“状态标记”的布尔值



代替一些操作符

| Reflect.       |                |
| -------------- | -------------- |
| get            | in             |
| set            | = 赋值操作符   |
| has            | in 或者 with() |
| deleteProperty | delete         |
| Construct      | new            |



使用`Reflect.apply`调用函数(被调用函数，this指向，[实参..])



#### 构建多层拦截网

​	代理另一个代理



#### 代理模式

一种编程模式

##### 跟踪属性访问

##### 隐藏属性

```js
const hiddenProperties = ['age', 'sex']
const person = {
    age: 18,
    sex: '男',
    name: '小明'
}
const proxy = new Proxy(person, {
    get(TrapTarget, property){
        if(hiddenProperties.includes(property)){
            return undefined
        } else {
            return Reflect.get(...arguments)
        }
    },
    has(target, property){
        if(hiddenProperties.includes(property)){
            return false
        } else {
            return true
        }
    }
})

console.log(proxy.name)
console.log(proxy.age) //undefined

console.log('name' in proxy) //true
console.log('sex' in proxy)//false
```



##### 属性验证

​	赋值操作触发`set`，根据情况决定赋值

##### 函数与构造函数参数验证

##### 数据绑定和可观察对象





