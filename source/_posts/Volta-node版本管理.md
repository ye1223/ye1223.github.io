---
title: Volta node版本管理
author: Levy Liu
wordCount: 30
charCount: 582
imgCount: 3
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 46 seconds
date: 2024-11-13 21:35:18
tags:
---
## volta

> 试了多种node版本工具`nvm` `fnm`，都没有成功下在离线状态下托管本地node包

快速安装管理和切换不同版本的node、npm、yarn等的工具

### 离线管理不同版本的node（Windows）

#### 1. 下载安装volta 

* ` https://github.com/volta-cli/volta/releases/download/v2.0.1/volta-2.0.1-windows-x86_64.msi`

#### 2. 导入本地node预构二进制文件压缩包

  * 将Windows的 node zip包放在`C:\Users\{用户}\AppData\Local\Volta\tools\inventory\node`文件夹中

  * ![windows-volta-node.png](https://s2.loli.net/2024/11/13/UHpEI4ekiMvmdLR.png)

> ==到这一步volta还没有托管手动导入的node包，需要手动挂载到全局或者项目中，才能正常使用==

  


#### 3. 托管本地node包（切换node版本也是这流程）

  * 全局级别

    * `volta install node@<version>`

  * 项目级别

    * 进入**项目根目录**终端，`volta pin node@<version>`
    * ![volta-.png](https://s2.loli.net/2024/11/13/l2Rfm5NTXLGsnA1.png)

  > volta将托管导出进来的node包，并在文件夹下生成一个`node-v版本号-npm`文件



#### 查看当前托管的node版本

* `volta list node` 
* ![volta-node.png](https://s2.loli.net/2024/11/13/kB2q8bl9DU4AxuF.png)

