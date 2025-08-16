---
title: CSS只显示两行
categories:
  - []
tags:
  - CSS
wordCount: 48
charCount: 1242
imgCount: 1
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 30 seconds
date: 2024-02-01 09:57:25
featured_image:
---

**`-webkit-line-clamp`** CSS 属性可以把[块容器](https://developer.mozilla.org/zh-CN/docs/Glossary/Block)中的内容限制为指定的行数。

它只有在 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 属性设置成 `-webkit-box` 或者 `-webkit-inline-box` 并且 [`box-orient`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-orient) 属性设置成 `vertical`时才有效果。

在大部分情况下，也需要设置 [`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow) 属性为 `hidden`，否则，里面的内容不会被裁减，并且在内容显示为指定行数后还会显示省略号。

```
{
  overflow: hidden;
  display: -webkit-box;
  text-overflow: ellipsis;
  -webkit-line-clamp: 2;  /* 两行 */
  -webkit-box-orient: vertical;
   word-break: break-all;
}
```

效果
![css只展示两行png.png](https://s2.loli.net/2024/02/01/ljaoHIB9rkU4uXn.png)

💡 其中， `word-break` 指定了怎样在单词内断行， 这里 `break-all` 将单词截断换行，由于这里只能显示 2 行，所以出现图中效果
