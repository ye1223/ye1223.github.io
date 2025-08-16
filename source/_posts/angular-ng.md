---
title: angular-ng
categories:
  - []
wordCount: 538
charCount: 11379
imgCount: 1
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 3 minutes
date: 2024-06-06 00:20:02
tags:
featured_image:
---

### Angualr

组件模式 AMD

scope

### 00

> ng-app 指令表明这个 html 文件为 angular 模板文件解析

使用 `ngApp` 指令自动引导 AngularJS 应用程序非常简单，并且适合大多数情况。在高级情况下，例如使用脚本加载器时，您可以使用命令/手动方式来引导应用程序。

1. The [injector](https://docs.angularjs.org/api/auto/service/$injector) that will be used for dependency injection is created.
   创建将用于依赖注入的注入器。
2. The injector will then create the [root scope](https://docs.angularjs.org/api/ng/service/$rootScope) that will become the context for the model of our application.
   然后，注入器将创建根范围，该范围将成为我们应用程序模型的上下文。
3. AngularJS will then "compile" the DOM starting at the `ngApp` root element, processing any directives and bindings found along the way.
   然后，AngularJS 将从 `ngApp` 根元素开始“编译”DOM，处理沿途发现的任何指令和绑定。

- Added phone images to `app/img/phones/`.
  将手机图像添加到 `app/img/phones/` 。
- Added phone data files (JSON) to `app/phones/`.
  将电话数据文件 (JSON) 添加到 `app/phones/` 。

### 01 控制器

简单理解就是在控制器中写页面处理逻辑

### 02 组件

​ 由于这种组合（模板 + 控制器）是一种常见且重复出现的模式，因此 AngularJS 提供了一种简单而简洁的方法将它们组合成可重用且独立的实体（称为组件）。此外，AngularJS 将为我们组件的每个实例创建一个所谓的隔离范围，这意味着没有原型继承，并且我们的组件没有影响应用程序其他部分的风险，反之亦然。

​ 事实上，人们可以将组件视为其更复杂、更冗长（但功能更强大）的同级指令（指令）的固执己见和精简版本，指令是 AngularJS 教授 HTML 新技巧的方式。您可以在开发人员指南的指令部分阅读有关它们的所有内容。

> 海云项目还没用到 component，

### 04 分文件编写

加载顺序

![image-20240514172110622](https://s2.loli.net/2024/05/17/OlKUGzXMjqu8Wb6.png)

> 在分文件编写的时候发现模块加载顺序很重要

### 05 filter

```html
<div class="container-fluid">
  <div class="row">
    <div class="col-md-2">
      <!--Sidebar content-->

      Search: <input ng-model="$ctrl.query" />
    </div>
    <div class="col-md-10">
      <!--Body content-->

      <ul class="phones">
        <li ng-repeat="phone in $ctrl.phones | filter:$ctrl.query">
          <span>{{phone.name}}</span>
          <p>{{phone.snippet}}</p>
        </li>
      </ul>
    </div>
  </div>
</div>
```

### 06 双向数据绑定

vue 的 v-model

```html
<input type="text" ng-model="$ctrl.test" /> <strong>{{$ctrl.test}}</strong>
```

### 07 依赖注入

DI subsystem

在控制器中使用“服务”

服务以`$` 开头，`$$`被认为是私有的

给某个 controller 注入内置`$http`服务

```js
function PhoneListController($http) {...}
PhoneListController.$inject = ['$http'];
...
.component('phoneList', {..., controller: PhoneListController});
```

内联写法

```js
function PhoneListController($http) {...}
...
.component('phoneList', {..., controller: ['$http', PhoneListController]});
```

### 08 templating and links

`ngSrc` 指令可防止浏览器向无效位置发出 HTTP 请求

### 09 Routing and Mutiple Views

Angular-route : `npm i angular-route`

注入$routeProvider 提供路由服务

```js
angular.module("phonecatApp").config([
  "$routeProvider",
  function config($routeProvider) {
    $routeProvider
      .when("/phones", {
        template: "<phone-list></phone-list>",
      })
      .when("/phones/:phoneId", {
        template: "<phone-detail></phone-detail>",
      })
      .otherwise("/phones")
  },
])
```

配合 ng-view 指令 （类似 vuerouter 的`<router-view/>`）

AngularJS（默认情况下）使用 URL 的哈希部分（即哈希 ( `#` ) 符号后面的内容）来确定当前路由。除此之外，你还可以指定一个散列前缀（默认为 `!` ），它需要出现在散列符号之后，以便 AngularJS 将该值视为“AngularJS 路径”并处理它（例如例如，尝试将其与路线匹配）[Using $location](https://docs.angularjs.org/guide/$location)

`phoneDetail` 模块依赖于 `ngRoute` 模块来提供 `$routeParams` 对象，该对象在 `phoneDetail` 组件的控制器中使用。由于 `ngRoute` 也是主 `phonecatApp` 模块的依赖项，因此其服务和指令已在应用程序中的任何位置可用（包括 `phoneDetail` 组件）。

==始终明确子模块的依赖关系。不要依赖从父模块继承的依赖项（因为有一天该父模块可能不存在）。==

### 11 自定义过滤器

由于此过滤器是通用的（即它不特定于任何视图或组件），因此我们将其注册到 `core` 模块中，该模块包含“应用程序范围”功

```
'use strict';

// Define the `core` module
angular.module('core', []);

```

```angular
angular. module('core'). filter('checkmark', function() { return function(input)
{ return input ? '\u2713' : '\u2718'; }; });
```

模版中直接使用 `{{  true | checkmark }}`

### 13 rest 和 定制服务

使用 $resource 工厂来定义一个资源服务，这个服务可以用来与后端进行 RESTful 交互

### angular.directive 指令

在 AngularJS 中，restrict 属性用于指定指令可以在模板中以哪种方式使用。restrict 属性可以接受以下参数：

1. **'E' (Element)**：

指令作为**元素**使用。

```
  <my-directive></my-directive>
```

2. **'A' (Attribute)**：

指令作为属性使用。

```
 <div my-directive></div>
```

3. **'C' (Class)**：

指令作为类使用。

```
   <div class="my-directive"></div>
```

4. **'M' (Comment)**：

指令作为注释使用。

```
  <!-- directive: my-directive -->
```

### 组合使用

可以组合使用多个参数，例如 'EA' 表示指令可以作为元素和属性使用。

```
.directive('myDirective', function() {
  return {
​    restrict: 'EA',
​   // 其他配置
  };

});
```

### angular.factory

`.factory` 是一个常用的方式来创建并返回一个服务的单例。`.factory` 方法用于封装业务逻辑或 API 服务，并在整个应用中共享。
