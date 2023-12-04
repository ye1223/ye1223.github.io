---
title: computed与watch
categories:
  - []
tags:
  - vue
wordCount: 226
charCount: 5130
imgCount: 0
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 1 minute
date: 2023-10-26 10:43:15
featured_image:
---

### **计算属性computed**

​	<u>适合用在模板渲染中，某个值是**依赖了其它的响应式对象**甚至是**计算属性**计算而来</u>

- <font color=red>支持缓存</font>，只有依赖数据发生改变，才会重新进行计算
- ==不支持异步==，当computed内有异步操作时无效，无法监听数据的变化
- computed 属性值会默认走缓存，计算属性是基于它们的响应式依赖进行缓存的，也就是基于data中声明过或者父组件传递的props中的数据通过计算得到的值
- 如果一个属性是由其他属性计算而来的，这个属性依赖其他属性，是一个多对一或者一对一，一般用computed
- 如果computed属性值是函数，那么默认会走get方法；函数的返回值就是属性的属性值；在computed中的，属性都有一个`get`和一个`set`方法，当数据变化时，调用set方法。

```vue
<template>
  <div>
    <p>计算属性示例</p>
    <p>原始数据: {{ originalData }}</p>
    <p>计算属性: {{ computedProperty }}</p>
    <button @click="updateData">更新数据</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      originalData: 5
    };
  },
  computed: {
    computedProperty() {
      // 计算属性，依赖于原始数据
      return this.originalData * 2;
    }
  },
  methods: {
    updateData() {
      // 更新原始数据，会触发计算属性重新计算
      this.originalData += 1;
    }
  }
};
</script>

```



### 侦听属性watch
​	<u>适用于观测某个值的变化去**完成一段复杂的业务逻辑**（例如执行异步或开销较大的操作）</u>

- <font color=red>不支持缓存</font>，数据变，直接会触发相应的操作；
- watch==支持异步==；
- 监听的函数接收两个参数，第一个参数是最新的值；第二个参数是输入之前的值；
- 当一个属性发生变化时，需要执行对应的操作；一对多；
- 监听数据必须是data中声明过或者父组件传递过来的props中的数据，当数据变化时，触发其他操作，函数有两个参数：新值和旧值

```vue
<template>
  <div>
    <p>监听的数据: {{ watchedData }}</p>
    <button @click="changeData">改变数据</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      watchedData: '初始值'
    };
  },
  methods: {
    changeData() {
      this.watchedData = '新值';
    }
  },
  watch: {
    watchedData(newVal, oldVal) {
      // 在数据变化时触发的操作
      console.log('新值：', newVal);
      console.log('旧值：', oldVal);
      // 这里可以执行您想要的其他操作
    }
  }
};
</script>
```

