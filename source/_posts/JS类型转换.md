---
title: JS类型转换
categories:
  - []
tags:
  - JavaScript
  - Preparation
wordCount: 19
charCount: 210
imgCount: 0
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 10 seconds
date: 2023-09-18 16:48:56
featured_image:
---




JS是动态类型语言，我们在定义一个变量其实并没有指定这个变量到底属于那种类型，只有到程序执行阶段才确定当前数据类型。

而<font color=red>各种**运算符**对数据类型是有要求的</font>，所以就会触发类型转换机制（no matter 人为 or 隐式触发）



## 显示转换：

​	通过JS内置的函数**明确转换的数据类型**

* Number

* parseInt(string, ?进制) 比Number宽松，一位一位解析遇到不能解析的停止 
* String
* Boolean

## 隐式转换：

​	运算操作符两边数据类型不一致（比较运算符、算术运算符）

* 转为Boolean (需要布尔值的地方，借助的`Boolean()`函数)
  * falsely变量：`undefined`、`null`、`false`、`+/-0`、`NAN`、`""`

* 转为String （复合类型--->原始类型---->字符串）
  * 常发生在，如 `"5" + xxx` 的加法运算中
* 转为Number
  * 除了加法运算符号，其他都有可能



## other

> `=== `在不进行类型转换情况下，双方的类型与值都相等