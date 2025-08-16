---
title: RENEW-vue2
categories:
  - []
wordCount: 1133
charCount: 29702
imgCount: 0
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 5 minutes
date: 2024-04-12 17:24:48
tags:
featured_image:
---

### 1. 关于生命周期

##### 	1.1 生命周期有哪些？发送请求在created还是mounted？

```
请求接口测试：https://fcm.52kfw.cn/index.php?_mall_id=1&r=api/default/district
```

Vue2.x系统自带有8个

```
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated // DOM 已经更新
beforeDestroy // 实例还在，进行清理任务，比如清除计时器、解绑全局事件监听器
destroyed 
```

发送请求在created还是mounted？

```
这个问题具体要看项目和业务的情况了，因为组件的加载顺序是，父组件引入了子组件，那么先执行父的前3个生命周期，再执行子的前4个生命周期，那么如果我们的业务是父组件引入子组件，并且==优先加载子组件==的数据，那么在父组件中当前的请求要放=mounted=中，如果当前组件没有依赖关系那么放在哪个生命周期中请求都是可以的。
```

> ==生命周期：先执行 父前三，在执行子元素前四==
>
> ==如果优先加载子组件的数据，就放在mounted中发送请求== ，==确保子组件优先挂载==

##### 	1.2 为什么发送请求不在beforeCreate里？beforeCreate和created有什么区别？

为什么发送请求不在beforeCreate里？

> ==拿不到methods方法（如果请求封装在methods中）==

```
因为：如果请求是在methods封装好了，在beforeCreate调用的时候，beforeCreate阶段是拿不到methods里面的方法的（会报错了）。
```

beforeCreate和created有什么区别？

> ==拿不拿得到$data与methods==

```
beforeCreate没有$data
created中有$data

created是可以拿到methods的方法的
beforeCreate拿不到methods的方法
```

##### 	1.3 在created中如何获取dom

> ==异步获取==

```
1. 只要写异步代码，获取dom是在异步中获取的，就可以了。
	例如：setTimeout、请求、Promise.xxx()等等...
2. 使用vue系统内置的this.$nextTick
```

##### 	1.4 一旦<font color=red>进入组件</font>会执行哪些生命周期？

> 进入组件，组件已经**挂载**

```
beforeCreate
created
beforeMount
mounted
```

##### 	1.5 第二次或者第N次进去组件会执行哪些生命周期？

> ==看是否缓存了组件==

如果当前组件加入了**keep-alive**，只会执行一个生命周期

```
activated
```

如果没有加入keep-alive

```
beforeCreate
created
beforeMount
mounted
```

##### 	1.6 父组件引入子组件，那么生命周期执行的顺序是？

> ==父三 子四==

```
父：beforeCreate、created、beforeMount
子：beforeCreate、created、beforeMount、mounted
...
父：mounted
```

##### 	1.7 加入keep-alive会执行哪些生命周期？

如果使用了keep-alive组件，当前的组件会额外增加2个生命周期（系统8 + 2 ）

```
activated
deactivated
```

如果当前组件加入了keep-alive第一次进入这个组件会执行5个生命周期

```
beforeCreate
created
beforeMount
mounted
activated
```

##### 1.8 你在什么情况下用过哪些生命周期？说一说生命周期使用场景

> activated 中重新刷新数据 ， 因为使用了缓存组件，后续进入该组件只会运行这一个生命周期
>
> deacticated 清理定时器相关的

```
created    ===> 单组件请求
mounted    ===> 同步可以获取dom，如果先子组件请求后父组件请求
activated  ===> 判断id是否相等，如果不相同发起请求
destroyed  ===> 关闭页面记录视频播放的时间,初始化的时候从上一次的历史开始播放
```

### 2. 关于组件

> 父 -> 子 ==props、this.$parent.xxx(子元素可以直接修改父元素数据)、依赖注入（在配置主题颜色 react中的context上下文 provider）==
>
> 子 -> 父 ==自定义事件emit、$refs==
>
> 兄弟元素： ==eventbus （发布订阅模式）==
>
> 集中状态管理 vuex pinia

##### 	2.1 组件传值（通信）的方式

```
父传后代 ( 后代拿到了父的数据 )
1. 父组件引入子组件，绑定数据
	 <List :str1='str1'></List>
	子组件通过props来接收
	props:{
		str1:{
			type:String,
			default:''
		}
	}
	***这种方式父传子很方便，但是父传给孙子辈分的组件就很麻烦（父=》子=》孙）
	这种方式：子不能直接修改父组件的数据

2. 子组件直接使用父组件的数据
	子组件通过：this.$parent.xxx使用父组件的数据
	这种方式：子可以直接修改父组件的数据
	
3. 依赖注入
	优势：父组件可以直接向某个后代组件传值(不让一级一级的传递)
```

```
后代传父 （父拿到了后代的数据）
1. 子组件传值给父组件
	子组件定义自定义事件 this.$emit
2. 父组件直接拿到子组件的数据
	<List ref='child'></List>
	this.$refs.child
```

```
平辈之间的传值 ( 兄弟可以拿到数据 ) 

通过新建bus.js文件来做
```

##### 	2.2 父组件如何直接修改子组件的值

```
<List ref='child'></List>
this.$refs.child.xxx = 'yyyy';
```

##### 	2.3 子组件如何直接修改父组件的值

```
子组件中可以使用：this.$parent.xxx去修改
```

##### 	2.4 如何找到父组件

```
this.$parent
```

##### 	2.5 如何找到根组件

```
this.$root
```

##### 	2.6 keep-alive

```
keep-alive是什么  ： 缓存当前组件
```

##### 	2.7 slot/插槽

```
匿名插槽 ：插槽没有名字
```

```
具名插槽 ：插槽有名字
```

```
作用域插槽 ： 传值
```

##### 2.8 provide/inject

```
provide/inject ===> 依赖注入
```

##### 	2.9 如何封装组件

```
组件一定要难点，涉及的知识点：slot、组件通信...
```

### 3. 关于Vuex

##### 	3.1 Vuex有哪些属性

```
state ==> 全局共享属性
getters ==> 针对于state数据进行二次计算
mutatioins ==> 存放同步方法的
actions    ==> 存放异步方法的，并且是来提交mutations
modules    ==> 把vuex再次进行模块之间的划分
```

##### 	3.2 Vuex使用state值

```
this.$store.state.xxx
```

```
辅助函数：mapState
```

以上俩种方式都可以拿到state的值，那么区别是什么？

```
使用this.$store.state.xxx是可以直接修改vuex的state数据的

使用辅助函数的形式，是不可以修改的
```

##### 	3.3 Vuex的getters值修改

面试官可能会这样问：组件使用了getters中的内容，组件使用采用v-model的形式会发生什么？

```
getters是不可以修改的
```

##### 	3.4 Vuex的mutations和actions区别

```
相同点：mutations和actions都是来存放全局方法的，这个全局方法return的值拿不到
```

```
区别：
	mutations ==》 同步
	actions   ==》 返回的是一个Promise对象，他可以执行相关异步操作
	
mutations是来修改state的值的，actions的作用是来提交mutations
```

##### 	3.5 Vuex持久化存储  ：在页面使用了state值：1，然后把1修改成2，然后刷新页面又回到了1为什么？ 

> ==我用的pinia==
>
> ==state在LS，读取时getItem，修改时setItem==

```
Vuex本身不是持久化存储的数据。Vuex是一个状态管理仓库（state：全局属性）==》就是存放全局属性的地方。
```

```
实现持久化存储：1. 自己写localStorage  2. 使用vuex-persistedstate插件
```

```
插件使用方式：https://www.xuexiluxian.cn/blog/detail/dae4073b07144d3c9abb3e2cc8495922
```

### 4. 关于路由

##### 	4.1 路由的模式和区别

> history 使用HTML5 的history API, ==**`pushState(state, title, url)`**== **`popstate`** replacestate
>
> hash检测浏览器url hash值 , ==浏览器并不会因为hash值的改变而作响应向服务器发送请求，而是有vuerouter控制客户端作相应的响应，利用onhashchange（window.location.hash）==   同一页面内的不同位置标记

> <font color=red>hash</font>
>
> ==服务器只需要返回入口页面（通常是 `index.html`）==, 而后面一切都是由vuerouter在客户端渲染完成的
>
> <font color=red>history</font>
>
> 需要在后端额外配置请求的uri地址

```
路由的模式：history、hash
```

```
区别：
1. 关于找不到当前页面发送请求的问题
	history会给后端发送一次请求而hash不会
2. 关于项目打包前端自测问题
	hash是可以看到内容的
	history默认情况是看不到内容的
3. 关于表象不同
	hash:#
	history:/
```

##### 	4.2 子路由和动态路由

> 动态路由 /user/:id 占位

##### 	4.3 路由传值

> query 和 params 两种

##### 	4.4 导航故障

> 可以catch捕获，isNavigationFailure 给出相关提示

```
官网说明：https://v3.router.vuejs.org/zh/guide/advanced/navigation-failures.html#%E6%A3%80%E6%B5%8B%E5%AF%BC%E8%88%AA%E6%95%85%E9%9A%9C
```

解决：

> $router.push跳转到一个相同的路由报错
>
> 重写push方法

```
import VueRouter from 'vue-router'
const routerPush = VueRouter.prototype.push
VueRouter.prototype.push = function (location) {
  return routerPush.call(this, location).catch(error => error)
}
```

##### 	4.5 $router和$route区别

```
$router不仅包含当前路由还包含 ==整个路由的属性和方法==

$route包含==当前==路由对象
```

##### 	4.6 导航守卫

> beforeResolve
>
> 所有组件内守卫和异步路由组件被解析之后，而在导航被最终确认之前。
>
> 提供了一个==最后的机会==来取消或修改即将发生的路由跳转。 （权限

```
1. 全局守卫

	beforeEach 路由进入之前
	afterEach 路由进入之后

2. 路由独享守卫
	
	beforeEnter 路由进入之前

3. 组件内守卫

	beforeRouteEnter 路由进入之前
	beforeRouteUpdate 路由更新之前
	beforeRouteLeave 路由离开之前
```

### 5. 关于API

##### 	5.1 $set

> 给响应式对象添加新属性
>
> 响应式数组对象**不能**直接通过**修改索引**的方式修改数据 ， 利相关方法push pop shift unshift splice sort reverse，更改length

```
面试官：你有没有碰到过，数据更新视图没有更新的问题==》$set
```

```
this.$set(target,key,修改后的值)
```

##### 	5.2 $nextTick

> ==在写单元测试的时候使用过，因为要获取当前最新的DOM，返回promise==
>
>  数据更新导致视图变化，（给这个DOM添加动画
>
> DOM是异步更新的，确保获取最新的DOM借助nexttick

```
$nextTick返回的参数[函数]，是一个异步的。功能：==获取更新后的dom==

源码|原理：
$nextTick( callback ){
		return Promise.resolve().then(()=>{
			callback();
		})
}
```

##### 	5.3 $refs

```
来获取dom的
```

##### 	5.4 $el

```
$el 获取当前组件的根节点
```

##### 	5.5 $data

```
$data 获取当前组件data数据的
```

##### 	5.6 $children

```
$children 获取到当前组件的所有子组件的
```

##### 	5.7 $parent

```
找到当前组件的父组件，如果找不到返回自身
```

##### 	5.8 $root

```
找到根组件
```

##### 	5.9 data定义数据

```
数据定义在data的return内和return外的区别：

1. return外：单纯修改这个数据是不可以修改的，因为没有被get/set
2. reutnr内：是可以修改的
```

##### 	5.10 computed计算属性

> getter setter写法

```
computed计算属性的结果值，可以修改吗？可以的，需要通过get/set写法

当前组件v-model绑定的值是computed来的，那么可以修改吗？可以的，需要通过get/set写法
```

##### 	5.11 watch

> ==可以在其中执行**异步**方法，但是要注意trycatch捕获处理异常==
>
> ==在组件销毁时清理watch，返回一个销毁函数==

```
watch:{
  obj:{
    handler(newVal,oldVal){
      console.log( 'obj',newVal , oldVal )
    },
    immediate:true,
    deep:true
  },
}
```

##### 	5.12 methods和computed区别

```
computed是有缓存机制的，methods是没有缓存机制的（调用几次执行几次）
```

### 6. 关于指令

##### 	6.1 如何自定义指令

> ==组件权限管理==

```
全局：
Vue.directive('demo', {
  inserted: function (a,b,c) {
    console.log( a,b,c );
  }
})
```

```
局部：
<script>
export default {
  directives: {
    demo: {
      bind: function (el) {
        console.log( 1 )
      }
    }
  }
}
</script>
```

##### 	6.2 vue单项绑定

```
双向绑定：v-model
```

```
单项绑定：v-bind
```

##### 	6.3 v-if和v-for优先级

> ==同时作用在一个元素上，浪费性能，我们其实只想渲染出list其中符合条件的item，事实上我们却把每条item都遍历了一遍==
>
> ==所以我们应该在vfor之前过滤filter==

> Vue2 
>
> ​	利用computed重新生成数组
>
> Vue3 虽说可以同时使用，但是，vif优先级高，可能不能及时获取到当前的item导致错误
>
> ​	借助vfor template标签
>
> ```vue
>  <template v-for="(item, index) in list" :key="index">
>           <li v-if="item.show">
>             {{ item.name }}
>           </li>
> 	</template>
> ```
>
> 

```
vue2中：v-for > v-if
```

```
vue3中：v-if > v-for
```

### 7. 关于原理

##### 	7.1 $nextTick原理

```
$nextTick功能：获取更新后的dom
```

```
$nextTick( callback ){

		return Promise.resolve().then(()=>{
			callback();
		})
		
}
```

##### 	7.2 双向绑定原理

> 双向绑定就是 数据与视图

UI 控件的变化能自动更新数据模型

vmodel 语法糖，vbind绑定在监听@input or @change事件做出改变



### 响应式

数据劫持： 当对数据进行取值操作的时候，就会对这个数据进行依赖收集，当这个数据发生改变的时候，就会触发视图更新，刷新页面。

在get中进行`depend`依赖收集，在set中进行`notify`通知依赖的watcher去重新渲染（视图更新）

`Object.defineProperty`这个api存在着一些问题，比如必须要**深层次递归监听一个对象内所有的属性**，性能并不太好，并且不能监听数组的`length`改变



数据代理 （复制一份）

​	Vue 在组件实例上通过 `this` 提供对组件数据的直接访问。这是通过将 `data` 对象中的属性代理到 Vue 实例上实现的。这样，你可以通过 `this.propertyName` 直接访问数据，而不是 `this.data.propertyName ` 

```js
Object.keys(vm.$data).forEach(key=>{
  Object.defineProperty(vm, key, {
      get() {
        return vm.$data[key];
      },
      set(newVal) {
        vm.$data[key] = newVal;
      }
    })
})
```

