```bash
├── public # 打包后生成的静态文件
├── scaffolds # 模板文档
├── source # 新建的帖子存放地
│   ├── _posts
│   ├── categories
│   └── tag
└── themes # 主题
    └── frame # 这里使用的Frame.主题
        ├── languages # 多语言配置
        ├── layout # 布局文件夹。用于存放主题的模板文件，当前为ejs模版
        │   ├── pages # 页面相关的partial
        │   ├── partials # partial局部模板让您在不同模板之间共享相同的组件
        │   └── post # 帖子相关的partial
        ├── lib
        ├── scripts # 启动时，Hexo会自动载入此文件夹内的JavaScript文件
        └── source # 资源文件夹，除了 模板 以外的 Asset，例如 CSS、JavaScript 文件等，都应该放在这个文件夹中
            ├── css
            ├── fonts
            ├── icon
            └── js
```





生成英文post

```
hexo new post --lang en 'your-filename'
```

