---
title: Create-React-App项目Template
categories:
  - []
wordCount: 253
charCount: 5512
imgCount: 0
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 1 minute
date: 2023-12-04 17:34:08
tags:
featured_image:
---

[💡项目地址](https://github.com/ye1223/CRA-template.git)
<br />
## 

```
├── .husky
├── .vscode
├── public
├── src
│   ├── App.tsx
│   ├── index.tsx
│   └── react-app-env.d.ts
├── prettier.config.js
├── tsconfig.json
├── package.json
└── yarn.lock
├── README.md
```

<br/>

### 初始化项目

​	`yarn add create-react-app [app-name] --template typescript`

### prettier

​	`yarn add -D prettier`

```js
//prettier.config.js
module.exports = {
   semi: false, // 在每条语句的末尾不使用分号
   trailingComma: 'es5', // 允许在ES5中有效的尾随逗号
   singleQuote: true, // 使用单引号而不是双引号
   printWidth: 80, // 指定打印行的最大长度
   tabWidth: 3, // 设置每个缩进级别的空格数
   useTabs: false, // 使用空格而不是制表符进行缩进
   bracketSpacing: true, // 在对象字面量中添加空格
   jsxBracketSameLine: false, // 在JSX中把'>'放在最后一行的末尾
   arrowParens: 'avoid', // 当箭头函数只有一个参数时不使用圆括号
   endOfLine: 'lf', // 行尾序列使用LF（\n）
}
```

### **husky**

​	`yarn add -D husky`

```json
//package.json
"scripts": {
  "prettier": "prettier --write \"src/**/*.{js,jsx,ts,tsx,json,css,scss,md}\""
}
```

​	**启用 Git 钩子**： `npx husky install`

​	**添加 pre-commit 钩子**：`npx husky add .husky/pre-commit "yarn prettier"`

### 路径别名

```json
"compilerOptions": {
  // ...其他配置...
  "baseUrl": "src",
  "paths": {
    "@/*": ["*"]
  }
}
```


<br />
<strong> 静候补充... </strong>