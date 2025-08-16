---
title: React小记
categories:
  - []
tags: react
wordCount: 333
charCount: 7577
imgCount: 32
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 7 minutes
date: 2023-07-25 16:13:21
featured_image:
---

## React库和框架的区别是什么？

* 页面组件化
* 数据驱动

MVC V层

> react库 
>
> ​	react.js是一个开放的js工具库，用于基于UI自检构建用户界面

> react框架
>
> ​	通过脚手架工具搭建的一套完善的前端环境，包括：路由、状态管理、数据获取、第三方的UI组件库和第三方Hooks库(ahooks react-use) 



## 严格模式

​	检查组件是否为**纯函数**

​	及早的发现useEffect中的错误

​	警告过时的API



## ESLint

​	代码规范插件

> npm run lint
>
> vite-plugin-eslint （vite构建下）

## Prettier

​	代码格式化插件



## react模块

​	核心功能

​	组件

## react-dom

​	操作浏览器DOM	

​	`react-dom/client` 客户端渲染使用

​	`react-dom/server `服务端渲染使用

> 分离不同代码库，可跨平台使用
>
> 比如 编写`react-native`应用，当然不用使用`react-dom`模块



利用**编译器**编译成中间这种`reactElement`对象格式(虚拟DOM)，再利用`react-dom`库完成

![image-20230719112407499.png](https://s2.loli.net/2023/07/25/cHAiE6MDN71R9pW.png)

![image-20230719140934815.png](https://s2.loli.net/2023/07/25/94UeQaLNsGli1JP.png)



## fragment 

`<></>` 简写，这种方式不能写key



## classnames 控制样式
![image.png](https://s2.loli.net/2023/07/25/no2bQfZGqpJ3rz4.png)



## 事件

​	合成事件：处理事件有差异（onmouse<u>enter</u> 实际使用的是<u>over</u>）

​	事件委托：委托到容器'root'元素

![image-20230719143026471.png](https://s2.loli.net/2023/07/25/4doOvBUxuV2z8fc.png)

​	传参处理

​		箭头函数

​		高阶函数



## 条件渲染 

​	条件分支if else switch case

​	三目 ? :

​	逻辑运算符 || ?? &&

> 不会渲染的值：null、undefined、boolean、'' 、对象、函数
>
> 利用JSON.stringify() 或者 {undefied +''}(拼接空串)

​	

## 数组渲染

> ​	<font color=red>jsx默认对数组进行join()操作</font>

​	循环语句

​	![image-20230719144614400.png](https://s2.loli.net/2023/07/25/Gc83QLyR5SVbFKW.png)

​	数组方法（map）

> 必须写key
>
> 帮助react推断发生了什么，从而得以正确的更新DOM树
>
> 跟踪列表每一项的身份， 唯一标识





## 组件的<u>点标记</u>写法

### 	对象形式

​		组件写为对象的方法	

​		可解构使用

![image-20230719145530569.png](https://s2.loli.net/2023/07/25/ZSJRGjxKWVdFlE9.png)

### 	函数形式（更好的进行组件分类

​	将组件直接挂载在上级组件上

​		![image-20230719145906028.png](https://s2.loli.net/2023/07/25/mMoLItqRXs6hyA5.png)





## 组件通信方式

props传递值

​	整体接收， 解构接收

props传递事件

通过扩展运算符{...}批量上传



## 组件组合方式

> 插槽

利用props的children属性

![image-20230719151126002.png](https://s2.loli.net/2023/07/25/fgLQPnTCZsOKjov.png)



​	指定顺序

![image-20230719151821461.png](https://s2.loli.net/2023/07/25/l326jsNfERpq4n8.png)

![image-20230719153233192.png](https://s2.loli.net/2023/07/25/35jOCBYAGLvbMSr.png)





## props默认值

​	利用es6的默认值方式

![image-20230719153233192.png](https://s2.loli.net/2023/07/25/35jOCBYAGLvbMSr.png)

​	react的defaultProps

![image-20230719153301244.png](https://s2.loli.net/2023/07/25/7Mbkav82tCNFzhP.png)



## props类型限定

​	使用ts

​	使用第三方proptypes



## 组件纯函数

​	只负责自己的任务，不会在更改函数调用前就已存在的对象和变量。（不能修改这个函数组件作用域外的对象和变量）

> ​	**严格模式**检测当前组件是否为纯函数，对这函数调用两次，检测值是否变化

​	输入相同，则输出相同。纯函数总是返回相同的结果。

> ​	不管调用多次，函数都输出同一个值，对于**测试**更方便。**增强健壮性**







## 组件的状态

瞬间变化的数据被称为状态（state），状态可以**进行数据驱动**



useState hooks 提供 <u>状态</u> 和 <u>修改状态</u>的方法



​	普通变量 无法重新渲染JSX

​	state状态，重新触发函数组件，并且具备**组件的<font color=red>记忆</font>**。

> ​	普通纯函数函数， 多次调用执行结果



### 状态是如何改变视图的

渲染与提交的过程 

1️⃣触发一次渲染

​	💡组件的初次渲染，createRoot.render

​	🔦内部状态更新，触发渲染送入队列

2️⃣渲染您的组件

​	💡在进行初次渲染，react调用根组件`<App/>`

​	🔦内部状态更新，会渲染对应的函数组件 

3️⃣提交到DOM上

​	💡初次渲染，appendChild DOM API

​	🔦内部状态更新，更新差异的DOM节点



### 多状态如何正确记忆？

同一个组件的每次渲染中，useState都依托于一个稳定的调用顺序

在react内部，每个组件保存了一个数组， 按照索引记忆usestate位置。`[{索引，useState对}]`

> 所以不要将useState写到一些分支逻辑中，会打乱useState顺序

配合eslint检查，语法是否合规



### 状态的快照

本次作用域中的状态不会改变 

![image-20230719163835286.png](https://s2.loli.net/2023/07/25/uh6UXZsLEilwmjY.png)

点击后，触发三次setcount，但是当前作用域的count都为0



快照的陷阱，异步的时候造成错觉，其实异步逻辑中的状态还是依赖于这次函数作用域

![image-20230719164801330.png](https://s2.loli.net/2023/07/25/65ayv4H7KrhLoli.png)



![image-20230719165254304.png](https://s2.loli.net/2023/07/25/9IhUgbs7ZVlW3Hz.png)



> 词法作用域，只看定义，不看调用

### 状态队列与自动批处理

自动批处理

​	等事件处理函数中的所有代码都运行完毕再处理你的state更新

​	队列都执行完毕后，在进行ui更新

更新函数的写法

![image-20230719170017902.png](https://s2.loli.net/2023/07/25/iCsuQxMSc71oygb.png)

形参c来自于react内部，所以这里并不是保存的count快照，连续调用三次后，得到3



> 更新函数在内部还是以回调的形式，但没使用形参



### 严格遵守状态不可变

**默认**情况下，修改的状态跟上一次相同的情况下，是不会重新触发渲染



### 引用数据类型

拷贝 

​	数组，使用相关方法

​	扩展运算符

​	深克隆（将没改变的数据也是克隆了一份）

​	immer useimmer 可以直接对数据进行修改



### 惰性初始化状态的值

当状态的值组要通过复杂计算才能得到的话，可以对其进行惰性初始化

> 只在初始化的时候执行一次





### 状态提升解决共享问题

父组件管理状态，子组件props



### 状态的重置处理问题

当组件被销毁会时，所对应的状态也会被重置

当**组件位置**没有发生变化，状态会被保留

![状态重置1.gif](https://s2.loli.net/2023/07/25/hZIvLofTXFxWDYz.gif)



> 组件渲染位置发生变化，状态不被保留
>
> ![状态-不同位置.gif](https://s2.loli.net/2023/07/25/7rkxUmafhFbcqGS.gif)
>
> 



不同的结构体，给组件添加**key属性**

> <font color=red>diff算法，同层级的元素是否发生改变</font>





### 利用状态产生计算变量

根据**状态会重新渲染组件**的特性，利用当前**状态快照**生成对应的计算变量

![chrome-capture-2023-6-20.gif](https://s2.loli.net/2023/07/25/BQFsjgh3nrIdA6k.gif)





## 受控组件与非受控组件

通过props控制的组件成为受控，通过state控制的组件称为非受控组件

react表单内置了受控组件的行为 

​	value + onChange

​	checked + onChange



input标签并不是原始的标签

```tsx
const App = () => {
  const [value, setValue] = useState("");
  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={(e) => {
          console.log(e.target.value);
          setValue(e.target.value)
        }}
      />
    </div>
  );
};
```





# Hooks

react中以use开头的函数被称为Hook钩子，Hooks被称为use函数的集合，也就是钩子的集合

Hooks就是一堆功能函数，（像插件

分为： 内置、自定义、第三方



## useRef

使用ref引用一个值，具备记忆功能

改变ref不会触发重新渲染组件

![image-20230720100818798.png](https://s2.loli.net/2023/07/25/iyCeFSGRxWDZXlr.png)

![image-20230720101032139.png](https://s2.loli.net/2023/07/25/XSUFWpV4CJlBMT3.png)



定时器案例：timer用ref存储，始终引用的同一个值



### useref进行dom操作

​	如：让元素获取焦点，滚动到他或测量尺寸位置

### 回调写法，在逻辑中操作dom

![image-20230720102321632.png](https://s2.loli.net/2023/07/25/vtEOLCYA8DgBTfR.png)



### forwardRef

给**组件**设置ref需要forwardRef**转发**

forwardRef让您的组件通过ref向父组件公开DOM节点



直接转发ref给子组件报错

![image-20230724150124791.png](https://s2.loli.net/2023/07/25/kVQnM81xyLu7oDC.png)

![image-20230724151308635.png](https://s2.loli.net/2023/07/25/B5Kh6kOrXaLWQdm.png)



### useImperativeHandle

自定义由ref暴露出来的句柄

![image-20230724153237639.png](https://s2.loli.net/2023/07/25/HSitNEdGyln4KzI.png)

![image-20230724153456747.png](https://s2.loli.net/2023/07/25/7h8YMuCK4WTOwyz.png)



## 纯函数处理useEffect

纯函数：

副作用：

​	函数在执行过程中对外部造成的影响成为副作用，如ajax、dom操作、与外部系统同步

​	事件可以处理副作用，onclick事件等

​	处理副作用，借助useEffect钩子



useEffect触发时机，**jsx渲染后触发**



依赖项，内部通过Object.is() 方法判定

​	当依赖项为空，指挥初始渲染

​	ESLint会检查依赖项是否正确，包括props state 计算变量



### 函数写在useEffect中

useCallback

函数始终是同一个

每次重新渲染函数，函数指向地址发生改变 





### useEffect清理工作

严格模式，检测有无做清理工作

初始化数据时，注意清理操作，

​	更简洁的方法是使用第三方，ahooks的useRequest



## useEffectEvent

![image-20230724162726502.png](https://s2.loli.net/2023/07/25/uKlfrvej4UA2OqJ.png)





## useLayoutEffect 同步执行状态更新

useEffect是在渲染之后，更新之前

如过需要在useEffect中处理DOM，并且改变页面的样式，就需要使用useLayoutEffect，





## useInsertionEffect DOM更新前触发

此时拿不到dom元素

在css-in-js库中使用的多





## useReducer 

局部的状态管理，抽离逻辑

## useImmerReducer

immer类型数据

## Context向组件深层次传递数据



> reducer配合context实现状态数据共享
>
> 更复杂的情况使用第三方库,redux mobx,zuster



## memo在props不变时跳过重新渲染

达到一些性能优化  

> props改变的情况下，会重新渲染组件



没有props

![memo-props1.gif](https://s2.loli.net/2023/07/25/Prz2fHX5ksuL71b.gif)

![memo-props2.gif](https://s2.loli.net/2023/07/25/bUIa1KeAVdiRzNg.gif)



## useMemo对计算结果进行缓存

虽然使用memo，并且props **数组list****值**未变化，但是list的引用地址发生变化，底层使用`Object.is([], []) ==>flase` 比较，则**props值**改变

![usememo1.gif](https://s2.loli.net/2023/07/25/G6FMPfQ1NcULiWZ.gif)

使用useMemo对计算数据进行缓存（类似useCallback缓存函数）  



## useCallback对函数进行缓存

useMemo的一种特殊写法

```react
const fn = useMemo(()=> () => { console.log(1) })
const fn = useCallback(() => { console.log(1) })
```





## StartTrasition 方法及并发模式

​	react18开始，渲染是一个单一的、不间断的、同步的事务，一旦渲染开始，就不能被中断

 	react18引入并发模式，允许标记更新作为一个transition，这回告诉react它们可以被中断执行。这样可以把紧急的任务先更新，不紧急的任务后更新

`任务切换`





## useTrasition hook

提供一个pending状态的布尔值，（配合实现一个loading效果），还有一个startTrasition方法

![useTransition.gif](https://s2.loli.net/2023/07/25/8l6byIz1rWvnd39.gif)



## useDeferredValue 延迟

获取该值的延迟版本

![useDeferredValue.gif](https://s2.loli.net/2023/07/25/UoeJhxVuCiRt2DB.gif)





## useId 产生唯一标识

id 局部前缀

​	xxx + id (字符串拼接)

id 全局前缀

​	createRoot 第二参

```tsx
ReactDOM.createRoot(document.getElementById('root')!, {
  identifierPrefix: 'xxx'
})
```



## useDebugValue 

在自定义hook中打印信息，devtool



## useSyncExternalStore

操作状态

订阅外部 store 的 React Hook。








