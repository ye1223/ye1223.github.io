---
title: npm
categories:
  - []
tags:
  - nodejs
wordCount: 157
charCount: 2234
imgCount: 4
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 1 minute
date: 2023-09-02 22:44:06
featured_image:
---


`node package manager`  基于命令行的工具 

安装、卸载、更新、发布

**version** **版本号x,y,z**

主 次 补丁

 ^ 锁定从左边数第一个非0的

~ 锁定主版本or次版本

latest 最新版本



# 依赖

dependecies

devDependencies 开发依赖

peerDependencies 对等依赖，（插件不能凭空运行，需要依赖某一模块）



# 一些常用命令

* `npm config list` 查看npm配置信息


<img src="https://s2.loli.net/2023/09/02/cZmUGLrlnh74v9Y.png" alt="image-20230902172055264" style="width:50%;" />

* `npm get registry`  查看镜像源
* `npm set registry xxxx`设置镜像源
* `npm ls (-g)` 当前（全局）安装的可执行文件

 

# npm install 原理

安装依赖存放于根目录`node_modules`文件夹

排序: .bin @系列 abcd排序

广度优先，扁平化（理想情况下）处理下载

> 非理想： node_modules下a b 模块依赖不同版本的c模块，会给b模块下再建立一个node_modules文件夹

![img](https://s2.loli.net/2023/09/02/ybEQgwSzVoL2kA3.png)

> pakage-lock.json
>
> 锁定版本号
>
> 缓存： intergrity + verion + name 生成唯一的key，这个key在缓存文件中,
>
> `npm config list` 查看中的cache
>
> 	index-v5 索引目录，记录content-v2的索引位置，对应的 intergrity + verion + name由算法生成的哈希值
>
> 对应得上，就将这个加密的包解压到node_modules中

| pakage-lock.json pakage.json |                                              |
| ---------------------------- | -------------------------------------------- |
| 不一致                       | 按照pakage中的版本下载，并更新lock中的版本号 |
| 一直                         | 先找缓存，没有再下载资源                     |



# npm run

可执行命令都存在node_modules下的`.bin`文件夹中

1. 当前项目node_modules/.bin 中查找

2. 全局的node_modules查找

3. 环境变量中查找

4. 报错 



也是有生命周期的

```json
{
  "predev": "node 1.js",
  "dev": "node 2.js",
  "postdev": "node 3.js"
}
```

运行 `npm run dev`, 自动执行了 1.js 2.js 3.js

> 比如一个打包命令，之前可以做一个清理的任务，最后做一个发布的工作，比如写一个CI脚本顺便把代码提交了
>
> Vue-cli也是用了生命周期pretest: 'yarn clean'



# npx

npm高版本自带的命令行工具

**运行node_modules/.bin 下的可执行文件**

> 之前通过package.json 中的scripts 配合npm run实现，现在可通过npx运行不需要安装这个依赖包运行命令

* 避免全局安装
  * 当前项目node_modules/.bin ---> 全局modules/.bin ---> 官网下载，用完删除（需要联网），减少内存占用

* 总是使用最新版本

* 执行任意npm包

> 在一个项目中，`npm i vite`	
>
> 没有全局安装，直接运行vite肯定是不行的，
>
> 此时配合npx， `npx vite` 在项目中的node_modules/.bin中查找运行

## 与npm区别

npx侧重执行命令

npm侧重安装或卸载某个模块，不具备执行某个模块的功能



# 发布npm包

* `npm addUser`（创建npm账号）
* `npm login` 登录( 使用**官方的源**，而不是第三方镜像源)
* `npm publish`  发布（同版本号不能重新发布，包名唯一）



# 构建npm私服

* 部署到内网集群，离线使用
* 安全性
* 提高包下载速度

利用`verdaccio`库

<img src="https://s2.loli.net/2023/09/02/PZQCziu4qrKAsRk.png" alt="image-20230902172055264" style="width:50%;" />

* `npm i verdaccio -g`
* `verdaccio --listen 5000` 开启服务
* 发包流程与之前的差不多
  * <img src="https://s2.loli.net/2023/09/02/zYhy9muZv8cWD3H.png" alt="image-20230902172055264" style="width:50%;" />