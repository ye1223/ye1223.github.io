---
title: vue3数据绑定之-sync
categories:
  - []
tags:
  - vue3
  - vue
wordCount: 15
charCount: 536
imgCount: 7
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 1 minute
date: 2023-08-03 22:19:09
featured_image:
---



~~**<font color=red>啊啊啊啊学了这么久vue3还不知道`sync`已经在vue3中被剔除了！！！</font>**~~



在vue2中我们利用v-bind值给子组件，子组件props接受父组件传来的值

![image-20230804094605352.png](https://s2.loli.net/2023/08/04/seqYNjUDltL2Avd.png)

![](https://s2.loli.net/2023/08/04/Quritsa3AmvLZH1.png)

子组件想直接通过props修该父组件传来的值，是不被允许的，这违背了**单项数据流**的原则

![](https://s2.loli.net/2023/08/04/bFmpQ6fGHYwAIrn.png)





那要子组件想要修改父组件的值，需要通过**自定义事件**

由父组件定义回调，子组件`emit`触发回调函数通知父组件修改改，（这里就不展开述说了）

-----

在vue2中提供了一个`v-bind`修饰符`.sync`，旨在传递一个值给子组件，而子组件可以更改这个值

![](https://s2.loli.net/2023/08/04/SQyuDWsB5ZbaLP6.png)

![](https://s2.loli.net/2023/08/04/P4WCG5B71k9q6cR.gif)



而在vue3中这个sync被v-model替代了

> 传递给子组件的，vmodel绑定的这个值一定得是响应式的

![](https://s2.loli.net/2023/08/04/tegdIJlGUPpxYmV.png)

![](https://s2.loli.net/2023/08/04/yoMnjfHrslQcXFv.gif)

