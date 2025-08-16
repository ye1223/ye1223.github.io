---
title: 模块化 + webpack打包
categories:
  - []
wordCount: 3661
charCount: 83750
imgCount: 23
vidCount: 0
wsCount: 0
cbCount: 4
readTime: About 20 minutes
date: 2024-04-22 17:12:18
tags:
featured_image:
---

「模块化」是思想



![内容概要](https://s2.loli.net/2023/05/08/cqUElmXNyaRPnsG.png)




## ==模块化的演变过程==

> 都是依靠script标签加载模块，不能代码控制加载

1. #### 文件划分方式（全靠约定）

   每一个文件是一个独立模块，存入的状态数据和功能函数 

   script src引用

   使用函数和全局变量

缺点： 污染全局变量、命名冲突、管理模块之前依赖关系

2. #### 命名空间方式

   将每个模块封装到一个<u>对象</u>中，依靠name属性命名模块

缺点：没有私有空间，可以在外被修改、依赖关系

3. #### IIFE

   将模块用<u>IIFE封装</u>，将所需要暴露的数据挂载在<u>全局对象window..</u>上

​	  通过闭包的方式，私有变量、参数传递，依赖关系（Jquery $）





## ==模块化规范==

### 模块化规范的出现

#### CommonJS

> node提出
>
> 同步加载

* 一个文件就是一个模块
* 每个模块都是有一个单独的作用域
* 通过module.exports导出模块、require函数载入模块



#### AMD(Asynchronous Module Definition) 异步模块定义规范

> 社区提出
>
> Require.js实现了AMD规范
>
> 模块加载器

```js
/* 定义一个模块 */
// 参数 定义模块名、引用依赖文件、函数（依赖文件导出的成员）
define('muduleName',['jquery', './moduleA'],function($, moduleA){
  // 导出成员
  return {
    $('body').animate({margin:'20px'})
    moduleA()
  }
})

/* 载入一个模块 */
require(['moduleName'],function(module){
  module.start()
})
//自动创建script标签
```

> 淘宝推出Sea.js 实现的CMD模块，旨在用CommonJS的方式写AMD，后续被require.js兼容 



### 模块化标准规范

浏览器：ES Module、node：CommonJS

#### ES Module 特性（2014年）

通过给script标签添加`type="module"`，以ESM标准执行JS代码

* 自动开启**严格模式** 
* 每个module单独私有作用域 
* 通过CORS方式请求外部的JS模块（外部必须支持CORS，要加入CORS响应头）
* 延迟执行脚本 `defer`

#### ES Module 导入和导出

 导出导入都可 as 重命名，

**`export`**

```js
export { a, b } //这个{} 不是对象，而是固定写法
export default xxx //xxx可以是一个变量或者值，
```

**`import`**

* 引用文件路径
  * 不能省略`.js`文件后缀、也不能直接省略访问`index.js`，后续通过**打包工具**可以这样做 
  * 不能直接载入文件名（`from 'moduleA.js'`），因为这样会被识别成第三方库
  * 可以载入相对路径(不能省略`./`)、绝对路径、完整的url路径（引用cdn模块）

* 只导入不取值

  * ```js
    import {} form './moduleA.js'
    import './moduleA.js' //简写方式
    ```

* 全部导入

  * ```js
    import * as mod from 'moduleA.js'
    ```

* 动态加载模块（import函数）

  * 普通`import from` 只能作用于最顶层作用域

    ````js
    import('./moduleA.js') 
    ````

* 同时默认和分别导出时

  * ```js
    //导出
    export {a, b}
    export default '1234'
    
    //导入
    import {a, b, default as str} from './moduleA.js'
    import str, {a, b} from './moduleA.js'  //简写 
    ```

**`export import 结合`**

* 导入的成员，直接export导出出去（模块中间桥梁）

```js
//index.js文件
/* import {button} from './button.js'
import {avatar} from './avatar.js'
export {button, avatar} */

export {button} from './button.js'
export {avatar} from './avatar.js'
//export {default as aaa} form './a.js' //默认导出要重命名

//后面只需要一个import index.js就可导入button.js和avatar.js 
```



#### ES Module 浏览器环境兼容

> 这种方式只适合<font color=red>开发阶段</font>，生产阶段不用。动态解析脚本，性能差
>
> 最好的方式还是在执行前，就将代码编译好

有些浏览器不支持直接解析es6 

[es-module-loader](https://github.com/ModuleLoader/browser-es-module-loader)代码读出来，交给babel转换

不支持promise，用`promise-ployfill`

借助`script`标签的<font color=red>`nomodule`</font>属性，只在不支持的浏览器运行转换脚本

```html
<script nomodule src="https://unpkg.com/browser-es-module-loader@0.4.1/dist/babel-browser-build.js"></script>
<script nomodule src="https://unpkg.com/browser-es-module-loader@0.4.1/dist/browser-es-module-loader.js"></script>
    
<script type="module">
	console.log('我是一个模块')
</script>
```



#### ES Module 在Node中使用

在node环境下，esm文件后缀为`.mjs` ,不太推荐使用



<u>官方内置模块</u>做了兼容，可以默认全部导入，也可以分别导入

第三方库一般都是默认导出，所以不能import {xx} from '.module.js'

 

#### ES Module 在Node中与 CommonJS模块交互

<font color=red> esm可以载入cjs模块</font>

```js
// esm.ejs
// 只能默认导入
import mod from './cjs.js'
console.log(mod)

// ********************* //

//cjs.js
/* module.exports = {
    foo: 'cjs中的foo'
} */
exports.foo = 'cjs中的foo'
```

cjs不能载入esm模块（在原生node环境中）



nvm use 版本号



#### ES Module在Node新版本中的支持

`package.json` 新增`"type":"module"` 字段，node中`.js`文件模块就设定为ESM

此时还需用CommonJS模块，后缀名为`.cjs`



#### ES Module 在Node中 Babel兼容方案

Babel js编译器，使用新特性代码 =(编译成)=》当前环境支持的代码



##### 用preset插件集合

下载所需模块yarn add `@babel/node` `@babel/core` `@babel/preset-env ` --dev(作为开发依赖)

运行 `yarn babel-node xxx.js --presets=preset-env`  (还有其他preset)



* 如果不想每次运行都添加--presets=xxx。那么在文件下新建`.babelrc`文件，中添加

  * ```json
    {
      "presets":["@babel/preset-env"]
    }
    ```

    



> babel基于<font color=red>插件机制</font>实现，核心模块@babel/core并不会转换代码
>
> Babel一个插件转换一个特性
>
> ![](https://s2.loli.net/2023/05/08/2D7s3uIPNTkp8aK.png)
>
> 
>
> preset-env 是插件集合
>
> ![preset插件集合](https://s2.loli.net/2023/05/08/xiucOT21A4azENP.png)



##### 用单独的转换插件！！！

但是帮我们转换的是一个单独的插件而不是preset插件集合

所以我们用单独的插件plugin，转换

移除preset `yarn remove @babel/preset-env`

添加转换插件 `yarn add @babel/plugin-transform-modules-commonjs --dev`

* .babelrc文件

  * ```json
    {
      "plugin":[
        "@babel/plugin-transform-modules-commonjs"
      ]
    }
    ```

运行 `yarn babel-node xx.js`





## ==模块化打包工具==

> ESM 存在环境兼容问题
>
> 模块文件过多，网络请求频繁，影响工作效率
>
> 所有前端资源(html, css...)都需要模块化

|                        新特性代码编译                        |                         模块化JS打包                         |                    支持不同类型的资源模块                    |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![image-20230504103526814](https://s2.loli.net/2023/05/08/xNJsOmDZSQ24Cka.png) | ![image-20230504103539317](https://s2.loli.net/2023/05/08/Yoq8ByXEDLQFJw1.png) | ![image-20230504103918557](https://s2.loli.net/2023/05/08/o5xhIYfqEnHteFs.png) |


模块打包工具（Module Bundler）

`webpack` `parcel` `rollup`

打包工具的模块化是指对<font color=red>前端整体的模块化</font>，并不单指JS的模块化



### `webpack`

模块加载器loader （代码转换）

代码拆分 code splitting 

资源模块 assets module，以模块化的方式引入任何资源文件



`yarn init --yes`

`yarn add webpack webpack-cli --dev`

`yarn webpack` (默认 打包src下的index.js文件)

 

#### 配置文件

默认打包 `src/index.js` 



`webpack.config.js` 运行在node环境中的CommonJS模块

```js
const path = require('path')
module.exports = {
    entry: './src/main.js', //指定入口文件
    output: {
        filename: 'bundle.js', //指定打包后的文件名
        path: path.join(__dirname, 'output') //必须为绝对路径
    }
}
```



#### 工作模式

正常运行webpack出现警告⚠️

> WARNING in configuration
> The 'mode' option has not been set, webpack will fallback to 'production' for this value.
> **Set 'mode' option to 'development' or 'production' to enable defaults for each environment.**
> You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/



##### prodcuction 生产默认模式

yarn webpack

​	自动优化打包结果

##### 开发模式

yarn webpack --mode development 优化打包速度，添加调试过程中辅助

##### none 模式

yarn webpack --mode none 最原始



或者在`webpack.config.js`文件添加`mode: 'development'`属性，就不需要在`yarn webpack --mode 参数`



#### 资源模块加载

webpack默认只处理js文件 

不同类型的资源文件，需要不同类型的加载器loader



##### 加载css资源

`yarn add css-loader ` ***将css文件转为js文件模块***

`yarn add style-loader` 将css-loader转换的css文件以style标签追加到页面上

```js
//webpack.config.js
const path = require('path')
module.exports = {
    mode: 'none',
    entry: './src/main.css',//打包css文件
    output: {
        filename: 'bundle.js',
        path: path.join(__dirname, 'output')
    },
    module: {
        rules: [
            {
                test: /.css$/, //打包文件后缀
                use: [
                    'style-loader',
                    'css-loader'
                ]//use模块是从后往前直行，左移css-loader要卸载后面
            }
        ]
    }
}
```



***loader是webpack的核心特性，通过不同的loader加载不同类型资源***



###### ==导入import资源模块==

打包入口---> 运行入口

js驱动驱动整个前端业务



| js文件作为打包的入口 ，然后在js模块中通过import引入css文件   |
| ------------------------------------------------------------ |
| //main.js <br />`import './xxx.css'` `import './yyy.css'`    |
| <img src="https://s2.loli.net/2023/05/08/8BQZXKWvAbUh6Df.png" alt="image-20230504140630685" style="zoom:100%;" /> |



***根据代码的需要动态导入资源*** 

<font color=red>逻辑合理，js需要资源文件</font>

<font color=red>确保上线资源文件不缺失</font>

> 代码更易维护，减少网络请求
>
> loader 编译转换压缩
>
> Webpack静态资源优化，分析依赖关系是否必须，tree-shaking，优化代码





##### 文件资源加载器 file-loader

`yarn add file-loader` 拷贝物理文件

```js
output: {
        filename: 'bundle.js',
        path: path.join(__dirname, 'output'), //默认是dist
        // publicPath: 'output/'  //默认
    },
module: {
        rules: [
            {
                test: /.jpeg$/,
                use: 'file-loader' //配置file-loader
            }
        ]
    }
```

根据配置文件的匹配到对应的文件加载器

文件加载器将导入的文件**拷贝到输出的目录**，

将这个拷贝的**文件路径**作为这个**模块的返回值**返回，文件就被发布出来，

可以通过模块的导出成员拿到这个资源的访问路径

![image-20230504143659882](https://s2.loli.net/2023/05/08/7BHleROkNcDWFvd.png)





##### url-loader

DataUrls 这个就已经代表了文件

将小型文件（如图片、音频等）直接嵌入到 HTML、CSS 或 JavaScript 文件中，而不必再通过网络请求获取这些文件

| ![image-20230504144417780](https://s2.loli.net/2023/05/08/FOMloWnvTq9Hc4Z.png) |
| ------------------------------------------------------------ |
| ![image-20230504144448616](https://s2.loli.net/2023/05/08/KoEDmiLzx87gRp4.png) |
| ![image-20230504144533002](https://s2.loli.net/2023/05/08/A2gP89vCDUpJBRM.png) |
| ![image-20230505105909806](https://s2.loli.net/2023/05/08/cUkDwi7jH8e9XSC.png) |

通过url-loader实现DataUrls

`yarn add url-loader`

```js
//webpack.config.js
module: {
        rules: [
            {
                test: /.jpeg$/,
                //use: 'url-loader',
              	use: {
                    loader: 'url-loader',
                    options:{
                        limit: 10 * 1024 //10KB,只处理<=10KB，的文件,超过就单独提取存放
                    }
                }
            }
        ]
    }
```

url-loader不会像file-loader在输出文件夹，而是生成导出文件（在打包文件中）



<p style="color:red;">小文件使用DataUrls的方式，减少请求次数
  <br>大文件单独提取存放，提高加载速度</p>需要同时安装file-loader和url-loader模块



#### 常用加载器分类

| 编译转换类                                                   | 文件操作类                                                   | 代码检查类                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              | 统一代码风格，提高代码质量                                   |
| ![image-20230504150809959](https://s2.loli.net/2023/05/08/J49vgDNeiHhGoy7.png) | ![image-20230504150857447](https://s2.loli.net/2023/05/08/NXHYKzByxDGSv2A.png) | ![image-20230504150924725](https://s2.loli.net/2023/05/08/8puCoUVwRJmiWgh.png) |



#### webpack与ES2015 babel-loader

因为打包需要，处理export和import

利用babel中的预设插件编译转换代码

`yarn add babel-loader @babel/core @babel/preset-env `

```js
//webpack.config.js
module: {
        rules: [
            {
                test:/.js$/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            },
          ]
		}
```



#### 模块加载方式

> 非必要，不要混用标准

遵循ESM的import声明

遵循CJS的require函数，（CJS require ESM的默认导出模块，需要`require().default`）

遵循AMD的define和require函数

Loader加载的非JS也会触发资源加载（处理的结果打包到输出目录）

​	样式代码的@import指令 url函数

​		`@import url(reset.css);`

​		`background-image: url(background.jpeg);`

​	html中的src属性

  > Footer.html
  >
  > ```html
  > <footer>
  >     <!-- <img src="footer.jpeg"> -->
  >     <a href="footer.jpeg">download</a>
  > </footer>
  > ```
  >
  > 
  >
  > html文件默认导出是字符串，需要接收
  >
  > ```js
  > import footerHtml from './footer.html'
  > document.write(footer.html)
  > ```
  >
  > 再配合html-loader（==默认只能加载img src==）
  >
  > ```js
  > //webpack.config.js
  > module: {
  >     rules: [
  >         {
  >             test: /.html$/,
  >             use: 'html-loader'
  >         },
  >         options: {
  >             // attrs: ['img:src', 'a:href'] //已经被废弃
  >             sources: {
  >                 list: [
  >                     "...",// 所有默认支持的标签和属性，这个一定要加上，不然就只会检测a标签了
  >                     {
  >                         tag: "a",
  >                         attribute: 'href',
  >                         type: 'src'
  >                     }
  >                 ]
  >             }
  >         }
  >     }
  > ```
  >

---

#### Loader核心工作原理

入口文件（js文件 ）

通过require import 推断解析文件所依赖的模块，分别解析每个模块

生成依赖树，递归依赖树，找到结点的依赖文件，根据配置文件rules属性，找到对应loader，最后打包在bundle.js文件

***Loader机制是webpack核心***

---

>  Loader本质上是一个导出函数的Javascript模块，Webpack调用这个函数，将文件内容传递给它，返回转换后的结果。这个函数需要返回一个Javascript模块，Webpack会将它打包到最终的输出中。

***管道（Pipeline）机制***



#### 开发一个Loader

loader负责资源文件从输入到输出的转换

webpack中对于同一个资源可一次使用多个loader，但是最后loader处理的result必须是一段js代码

<img src="https://s2.loli.net/2023/05/08/lvFeHpb1SPcY3rG.png" alt="image-20230504174619249" style="zoom: 25%;" />

打包过后，webpack直接将loader return的js代码拼接在打包后的js文件中



##### 简单实现一个 显示markdown loader

```markdown
//aboutme.md
# 一个markdown文件
 xxxxx
```

```js
//main.js 打包入口文件

//导入md文件
import aboutMd from './aboutme.md'  //一般md文件导入会是一段html字符串
console.log(aboutMd)
```
loader return输出必须是一段js代码，其实就是将return的结果直接拼接在打包完成的js文件中

```js
//markdown-loader.js
module.exports = source =>{
  const marked = require('marked') //借助marked
	const html = marked.parse(source)
  
 	//return `module.exports = ${JSON.stringify(html)}` //return输出必须是一段js代码
  // return `export default ${JSON.stringify(html)}` //esm导出
  
  //!!!!借助html-loader 解析html
  return html
}
```

```js
//webpack.config.js
module: {
    rules: [
        {
            test: /.md$/,
            use: [//use数组，从后往前执行
                'html-loader',//!!!!借助html-loader 解析html
                './markdown-loader'
            ]
        }
    ]
}
```



#### 插件Plugin机制

增强webpack自动化能力

处理<u>除了loader处理的资源加载</u>以外自动化工作

e.g. 清除上次打包的dist文件夹

​		将拷贝的静态文件输出至目录

​		压缩输出代码

​		...

实现大多数前端工程化



##### `clean-webpack-plugin` 

清除输出目录

新打包只会覆盖重名文件，而其他文件则会一直积累在输出目录

`yarn add clean-webpack-plugin --dev` 第三方库

```js
//webpack.config.js
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
module.exports = {
  plugins:[
    new CleanWebpackPlugin()
  ]
}
```



##### `html-webpack-plugin`

**webpack输出HTML文件**

>  `html-webpack-plugin`是一个webpack插件，用于生成HTML文件。它可以根据你的配置自动生成一个HTML文件，并将打包生成的js、css等文件**自动引入**到HTML文件中。

自动使用bundle.js的html

最后只用发布dist目录，不需要另外再同时发布一个html文件

`yarn add html-webpack-plugin --dev`

```js
//webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  plugins:[
    //new HtmlWebpackPlugin() //在输出目录生成index.html
    new HtmlWebpackPlugin({
      title: 'webpack plugin sample',
      meta: {
        viewport: 'width=device-width'
      },
      template:'./src/index.html' //模板文件
    })
  ]
}
```

```html
<!-- 模板文件 ejs模板 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= htmlWebpackPlugin.options.title %></title>
</head>
<body>
    <div class="container">
        <h1><%= htmlWebpackPlugin.options.title %></h1>
    </div>
</body>
</html>
```

**输出多个html页面，直接多`new HtmlWebpackPlugin({filename:'hello'})`**



##### `copy-webpack-plugin` 静态资源

将静态文件打包copy到输出目录 ,[一般不在开发环境中使用](#jump)

`yarn add copy-webpack-plugin --dev`

```js
//webpack.config.js
const CopyWebpackPlugin = require('copy-webpack-plugin')
module.exports = {
  plugins:[
    new CopyWebpackPlugin({
      patterns: [
        {from: 'public', to: ''} //从那个文件到输出文件夹
      ]
    })
  ]
}
```



##### 自定义插件

Plugin通过钩子机制实现

一个具有 [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法的 JavaScript 对象

通过在生命周期的钩子挂载函数实现扩展

```js
//webpack.config.js

// 用于处理删除打包bundle.js 的前面的注释
class MyPlugin(){
  apply(compliler){
    compiler.hooks.emit.tap('MyPlugin', conpilation => {
      // compilation 此次打包的上下文
      for(const file in compilation.assets){
        //console.log(filename) //打包目录的某个文件
        if(file.endsWith('.js')){
          const content = compilation.assets[file].source() //某个文件内容
          const withoutComments = contents.replace(/\/\*\*+\*\//g, '')
          compliation.assets[file] = { //覆盖一下里面的方法
            source: ()=> withoutComments,
         		size: ()=> withoutComments.length
         }
        }
      }
    })
  }
}
```





#### 增强开发体验

手动启动服务，过于原始

<img src="https://s2.loli.net/2023/05/08/NvyFanxBXjJAkKE.png" alt="image-20230504205435077" style="zoom:35%;" />



> **需求**
>
> http server启动（接近）
>
> 自动编译+自动刷新
>
> source map支持



##### 自动编译+刷新

* <u>`watch`工作模式</u> 监听文件变化，自动重新打包

​	专注编码

​	`yarn webpack --watch`

* 编译后自动刷新浏览器

​	[`BrowserSync`](https://browsersync.bootcss.com/#install)

```
npm install -g browser-sync
browser-sync start --server --files "css/*.css" //一个基本用途是，监听某个 css 文件
//"**/*" //监听所有文件的变化
```

**`browser-sync 文件夹名 --files "**/*" `**

> ***watch 与 browser-sync结合麻烦，效率低***



##### ==`webpack-dev-server`==

==对开发者提供一个良好的服务器==

自动编译+刷新

`yarn add webpack-dev-server --dev`

`yarn webpack-dev-server` 启动

添加 `--open` 自动打开浏览器窗口

*将打包结果写入内存，并没有生成dist目录，不需要**读写磁盘**，提高效率*



##### `webpack-dev-server`静态资源访问

Dev-server默认只会serve打包输出文件

只要是通过webpack打包输出的文件都能被访问到

<span id='jump' style="visibility:none;"></span>

> 之前通过clean-webpack-plugin插件实现将静态资源打包到输出文件夹，
>
> 但是在开发环境一般不要使用插件，
>
> 因为我们频繁修改代码，拷贝静态文件，将影响开发
>
> 所以一般是在上线的阶段前，使用一次这个插件

[webpack.devServer.static](https://www.webpackjs.com/configuration/dev-server/#devserverstatic) 

```js
//webpack.config.js
module.exports = {
  devServer: {
    //contentBase: './public', //wepack5已经移除
    static: {
            directory: path.join(__dirname, 'public') //指定当前项目静态资源文件夹
        }//可以托管多个静态文件夹
  }
}
```





##### `webpack-dev-server `代理API服务

> CORS 跨域资源共享，服务端开启
>
> 并不是任何情况服务端都必须支持CORS，
>
> 前后端同源部署，没必要开启CORS

|                         代理API服务                          |
| :----------------------------------------------------------: |
| <img src="https://s2.loli.net/2023/05/08/yVghdvRLlaZ39ES.png" alt="image-20230505135331738 " style="zoom:33%;" /> |

**代理 https://api.github.com**

```js
//webpack.config.js
module.exports = {
  devServer: {
    static: {
            directory: path.join(__dirname, 'public') //指定当前项目静态资源文件夹
        }
  	},
  	// !!!代理
  	proxy: {
      '/api': {
                // 对当前`/api/users`的请求 --会代理到--> `https://api.github.com/api/users`
                target: 'https://api.github.com',
                // 如果不希望传递/api，则需要重写路径 
                pathRewrite: { '^/api': '' }, //`/api/users`的请求 --会代理到--> `https://api.github.com/users`
                // 不能使用localhost:8080 作为请求GitHub的主机名
                /* 
                    服务器有多个网站，服务器通过主机名判断该请求那个网站 
                */
                changeOrigin: true
            }
    }
  
  }
}
```
使用

```js
// src/main.js
// 代理请求`https://api.github.com/users`
//http://localhost:8080/api/users 同源请求，不用担心跨域问题
fetch('/api/users').then(res=>res.json()).then(res=>{
  console.log(res)
})
```



##### sourceMap

运行代码与源代码不同

需要调试应用，定位错误

**调试和报错都是基于转换后的代码运行的**

|            sourceMap映射源代码与转换后代码的关系             |
| :----------------------------------------------------------: |
| <img src="https://s2.loli.net/2023/05/08/MOTNu8KoedPZj7v.png" alt="image-20230505151251462" style="zoom:25%;" /> |
| <img src="https://s2.loli.net/2023/05/08/nb23756hNwTodgZ.png" alt="image-20230505151353861" style="zoom:24%;" /> |

*sourceMap解决了源代码和运行代码不一致所产生的问题*

> 拿jquery距举例
>
> ```html
> <body>
> <!-- 引入的是压缩后的jq包 -->
> <script src='jquery.min.js'></script>
> <script>
>  // 使用
> 	var $body = $(document.body)
> 	console.log($body)
> </script>
> </body>
> ```
> ```js
> //jquery.min.js
> xxxxxxxxxxxxxxxxxxxxxx
> 
> //# sourceMappingURL=jquery.min.map 
> //浏览器会自动识别，用于在浏览器开发工具中调试和定位代码时提供源文件的映射关系
> ```
>
> ```js
> //jquery.js //js没被压缩的源文件
> xxxxxxxxxxx
> ```
>
> ```json
> //jquery.min.map //sourceMap文件
> xxxxxxxx
> ```
>
> 之后在浏览器调试，虽然文件引入使用的是压缩后的体积，但是因为有sourcemap文件就能直接定位到源文件





##### webpack配置sourceMap

###### `source-map`模式

```js
//webpack.config.js
module.exports = {
  devtool: 'source-map'
}
```

`yarn webpack` 在dist目录生成sourcemap文件



webpack支持多种soucemap方式，每种方式的效率和效果都不相同

###### `eval`模式

* 映射到转换后的代码
* 速度fastest

将模块打包后的代码放入eval函数执行，配合 `#sourceURL=路径`方式说明对应文件路径，浏览器执行就到源代码是哪个文件，从而定位文件

并没有生成sourceMap文件

> <font color=red>打包过后的模块代码</font>
>
> 定位文件信息，而不知道行列（）



###### 模式对比

`eval` 使用eval函数+sourceURL执行模块，只有行信息

`cheap` 只有行信息，没有具体列信息

带有`module`，没有经过loader es转换（例如babel-loader）的信息

![image-20230505165912024](https://s2.loli.net/2023/05/08/DC6gvoAi9TqN2mn.png)

`Inline` sourceMap以dataURl模式嵌入到转换后的模块中

`hidden` 生成了sourcemap文件，但是并没有注释引入到文件中，（第三方包，）

`nosources` 调试工具看不到源代码，但是提供了行列信息。生产环境保护源代码，不会被暴露



##### 选择sourcemap

开发环境 `cheap-module-eval-source-map`

<img src="https://s2.loli.net/2023/05/08/jLxybh6wuodRqc8.png" alt="image-20230505170647622" style="zoom:25%;" />

生产环境 `none`

<img src="https://s2.loli.net/2023/05/08/YoQROVGhfbacKMp.png" alt="image-20230505170741601" style="zoom: 25%;" />

***调试是开发阶段的问题***，没有信息选择nosources-source-map





#### HMR (hot module replacement)模块热更新

> 自动刷新页面任何操作状态都会丢失
>
> 1. 提前写死编辑器内容
> 2. 额外代码实现刷新前保存，刷新后读取
>
>  **最好：页面不刷新，模块可以更新代码模块**

应用执行过程中，实时替换某个模块，应用运行状态不改变

最强大的功能之一，极大提高了工作效率

集成在webpack-dev-server

##### 开启hmr `hot`

`yarn webpack-dev-server --hot`

或者

```js
//webpack.config.js
const webpack = require('webpack')
module.exports = {
  devServer: {
    hot: true
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ]
}
```





> <font color=red>webpack的HMR并不能开箱即用</font>
>
> 样式文件可以的原因是，经过loader处理，style-loader处理了样式文件热更新
>
> 使用框架开发vue react，每种文件有规律，有通用的替换办法，脚手架创建的项目都继承了HMR方案
>
> <font color=red>但是webpack的HMR需要手动处理模块热替换逻辑 </font>



*更新被我们**手动处理**了，就**不会触发自动刷新**，反之就会刷新页面*



##### js模块hmr

不同的js模块有不同的逻辑，所以需要自定义

```js
//main.js 打包入口js文件
import CreateEditor from './editor.js' //为一个创建编辑器的模块
const editor = createEditor()
document.body.appendChild(editor)

//##########处理hmr，与上面业务代码无关，并且下面代码并不会被打包############
if(module.hot){ //module.hot 由插件webpack.HotModuleReplacementPlugin()带来，原生module并不存在
 		 /**
     * //注册某个模块更新过后的处理函数
     * @param {string} './editor.js' - 监视模块依赖路径
     * @param {callback}  - 处理更新的逻辑回调函数
     */
  let lastEditor = editor
  module.hot.accept('./editor.js', () => {
    let value = lastEditor.innerHTML
    
    const newEditor = createEditor()
    newEditor.innerHTML = value
    document.body.appendChild(newEditor)
    
    lastEditor = newEditor
  })
}
```



##### 图片hmr

直接在`module.hot.accept`更新图片路径就行



##### `hotonly`

假如说hmr处理逻辑有误，就会导致页面自动刷新，看不到报错信息

那就最好使用`hotonly`，维持原状，不会自动刷新页面

`yarn webpack-dev-server --hot only`

或者

```js
//webpack.config.js
module.exports = {
  devServer: {
    //hotOnly: true, webpack5废弃
    hot: 'only'
  }，
  plugins:[
  	new webpack.hmrp()
  ]
}
```





`module.hot` 是`webpack.HotModuleReplacementPlugin()` 插件带来的，

<font color=red>处理热更新的逻辑也不会被打包</font>



#### 生产环境优化

生产环境跟开发环境差异大，<u>生产环境注重运行效率，</u>开发环境注重开发效率

不同的`mode` 配置：none, production, development

为不同的环境创建不同的配置



#### 不同的环境的配置

1. 配置文件根据环境不同导出不同配置
2. 一个环境对应一个配置文件



##### 环境变量参数配置

`webpack-config.js` 导出模块`module.exports`可以导出一个函数

> 参数 env 代表cli传递的环境变量
>
> 参数 argv 代表cli传递的所有参数
>
> ​	比如：`yarn webpack-dev-server --env production` 此时env.prodution === true
>
> 返回 一个配置

```js
//webpack.config.js
module.exports = (env, argv) => {
  const config = {
    mode: 'none',
    devtool: 'cheap-module-eval-source-map'
    entry: './src/main.js',
    output: {
      filename: 'bundle.js',
      path: path.join(__dirname, 'dist')
    }
    // ...等其他开发阶段的配置
  }
  
  // 此时环境为生产环境
  if(env.production) {
    config.mode = 'production',
    config.devtool = 'nosources-source-map',
    config.plugins = [
      ...config.plugins,
    	new CleanWebpackPlugin(),
      new CopyWebpackPlugin({
      patterns: [
									{ from: "public", to: "" }
              ],
    })
    ]
  }
return config
}
```



##### 不同环境对应不同配置

三个配置（开发、生产、公共）

```js
//webpack.common.js
module.exports = {//xxx
}
```

`yarn add webpack-merge --dev`  专门用于合并导出配置项的函数

```js
//webpack.prod.js 生产环境的webapck配置
const common = require('./webpack.common.js')
const { merge } = require('webpack-merge') // 合并配置项函数
module.exports = merge(common, {
  mode:'production',
  // xxxx
})
```

```js
//webpack.dev.js 开发环境的webpack配置 （与prod差不多）
```



执行不同开发环境的webpack配置，加上`--config`参数

`yarn webpack --config webpack.prod.js` 执行**开发环境**的webpack配置

或

在`package.json`，写入npm 脚本

```json
{
  "script":{
    "start" : "webpack --config webpack.dev.js",
    "build" : "webpack --config webpack.prod.js"
  }
}
```





#### webpack自带的优化

##### DefinePlugin

为代码注入全局成员

默认启用，<font color=gray>代码注入一个`process.env.NODE_ENV`常量，判断当前运行环境</font>

```js
//webpack.config.js
const webpack = require('webpack')
module.exports = {
  plugins:[
    new webpack.DefinePlugin({
      /*注入一个API_BASE_URL常量*/
      // API_BASE_URL: '"http://example.com"' //js代码片段
      API_BASE_URL: JSON.stringify('http://example.com') //或者用这个方式
    })
  ]
}
```

```js
//main.js
console.log(API_BASE_URL) //直接调用
```



##### Tree Shaking

shake掉代码中未引用的部分，『 未引用代码dead-code 』

**生产模式**自动开启 `yarn webpack--mode production`



> treeshaking 并不是webpack某一个配置选项
>
> 是一组功能搭配使用的效果 

```js
//webpack-cofig.js
module.exports = {
  optimization: {
    usedExports: true, //只导出使用了的模块
    minimize: true //压缩代码
  }
}
```



##### 合并模块函数

也是`optimization`的一个属性

**`concatenateModules`**，将所有的模块合并输出到一个<font color=red>函数</font>中（<u>*scope hoisting 作用域提升*</u>）

> 默认在生产环境启用，其他环境禁用





treeshaking 前提是使用ESM组织代码，由webpack打包的代码使用ESM

为了转换代码新特性，使用babel-loader，<u>**有可能**会将ESM->CJS（取决于是否使用了esm转换插件）</u>



@babel/preset-env 插件集合带有了esm转换插件

**但是**在最新的babel-loader中，使用preset-env插件会自动`auto`识别当前环境的模块化方式，不会强制转换esm



想要强制转换为commojs

```js
module.exports = {
  module: {
        rules: [
            {
                test: /.js$/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            ['@babel/preset-env', { modules: 'commonjs' }]
                     				//['@babel/preset-env', { modules: false}] //不会转换模块
                        ]
                    }
                }
            }
        ]
    }
}
```



##### sideEffects 副作用

通过配置表示代码是否有副作用，更好的帮助treeshaking

模块执行的时候，除了导出成员，做出其他的事情

> 一般用于NPM模块包标记是否有副作用
>
> 这个包是否会对包以外的对象产生影响，比如是否修改了 window 上的属性，是否复写了原生对象 Array, Object 方法。



<strong>使用前提：确保代码没有副作用</strong>

开启sideEffects

```js
//package.json
{
  "sideEffects" : false //标记表示这个包没有副作用
}

{
  "sideEffects" : [
    "./src/components/extends.js", //标记哪个文件有副作用
    "*.css"  //css模块也有副作用
  ]
}

//webpack.config.js
module.exports = {
  optimization: {
    sideEffects: true  //开启sideEffects优化功能
  }
}
```





##### 分包/代码分割Coding Splitting

打包到同一个bundle文件太大了，并不是每个模块都是在启动时必要的

分包，按需加载

> HTTP1.1 并不能对同一个域名下发起很多并行请求
>
> 每次请求有延迟
>
> 请求Header浪费带宽流量



* ==多打包入口 multi entry==

  * 多页应用程序
    * 一个页面对应一个打包入口，公共部分单独提取

  ```js
  //webpack.config.js
  module.exports = {
    entry: { //两个打包入口，对象形式
      home: './src/homepage/home.js',
      info: './src/info/info.js'
    },
    output:{
      filename: '[name].bundle.js'  //[name]占位符，匹配entry的键，进而有几个entry就有几个output
    },
    optimization: {
      splitChunks: {
        chunks: 'all' //将所有模块的公共部分再提取出来
      }
    },
    plugins: [
      new HtmlWebpackPlugin({
        title: 'homepage',
        filename: 'home.html',
        template: './src/homepage/home.html',
        chunks: ['home'] //指定哪个打包的模块才能插入到这个html页面中
      }),
      new HtmlWebpackPlugin({
        title: 'info',
        filename: 'info.html',
        template: './src/info/info.html',
        chunks: ['info'] //指定哪个打包的模块才能插入到这个html页面中
      }),
    ]
  }
  ```

  <font color=gray>HtmlWebpackPlugin默认将所有的打包文件都插入到html使用，要使用chunks配置项制定单独的js打包模块</font>

  

* ###### ==动态导入 Dynamic Import==

  * 所有动态导入的模块会<strong style="color:red;">自动分包</strong>

    * ```js
      import('./xx.js').then(module=>{
        console.log(module)
      })
      import('./yy.js').then(module=>{
        console.log(module)
      })
      
      //自动分成两个包
      ```

  * **魔法注释 Magic Comment**

    * 给动态引入文件的打包分包 **命名**

    * ```js
      import(/* webpackChunkName: "xxx" */'./xx.js').then(...)
      //生成的打包文件 //xxx.bundle.js
      ```

      ```js
      //webpack.config.js
      filename:'[name].bundle.js' //记得占位
      ```
      
      



##### 提取css文件

之前都是直接将css文件打包在同一个bundle中，

通过插件`MiniCssExtractPlugin` ,提取css文件，实现按需加载

> 之前是通过`css-loader`解析模块化css文件，再使用`style-loader`通过style标签注入到页面中
>
> 现在通过`MiniCssExtactPlugin.loader`配合`css-loader`将css文件**<font color=red>`link`</font>**引入

`yarn add mini-css-webpack-plugin --dev`

```js
const MiniCssExtractPlugin = require('mini-css-webpack-plugin')
module.exports = {
  module: {
    rules: [
      {
        test: /.css$/,
        use: {
          MiniCssExtractPlugin.loader,
          'css-loader'
        }
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin()
  ]
}
```



> 超过150KB的css文件就最好不要提取出来，
>
> 避免过多的请求





##### 压缩css文件

> 在开启生产环境模式时，webpack会自动开启代码压缩，
>
> 而webpack本身只能压缩js文件

使用**`OptimizeCssAssetsWebpackPlugin`**压缩css文件

`yarn add optimize-css-assets-webpack-plugin --dev`

> 如果将这个插件配置在plugins属性中，那么在任何情况都会工作
>
> 配置在`minimizer`配置，那么只在压缩的过程中才启用这个插件，
>
> 在生产环境，这时候需要配置额外的`terser-webpack-plugin` 压缩打包js代码

*webpack希望我们将压缩相关插件写在optimization.minimizer属性中*

```js
//webpack.config.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin') //提取css代码
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-plugin')
const TerserWebpackPlugin = require('terser-webpack-plugin')

module.exports = {
  optimiztion: {
    minimizer: [
      new OptimizeCssWebpackPlugin(),//压缩css代码
      new TerserWebpackPlugin() //压缩js代码
    ]
  },
  plugins: [
    new MiniCssExtractPlugin()//提取css文件
  ]
  
}
```



#### 输出文件名Hash

Substitutions

> 在部署前端资源文件是，启用服务器的静态资源缓存
>
> 对于用户的浏览器而言，可以缓存住应用当中的静态文件
>
> 后续就不足要请求服务器获取资源



> 假如说将文件过期时间设置较长，在过程中应用发生了更新，重新部署后，没有及时更新客户端

生产环境下，文件名用添加Hash值

资源文件发生改变，文件名也跟着改变，

对于客户端，全新的文件名就是全新的请求，没有缓存的问题



`filename: '[name].[hash].bundle.css'`

三种模式：

hash 项目级别，所有的打包文件共用一个hash。一个文件修改，所有文件的hash都变

trunkhash 一个entry对应一个hash

contenthash 单文件级别的hash



> 所依赖的文件改变，文件也会改变相对应hash改变。

