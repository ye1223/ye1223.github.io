---
title: FUNNY_ERROR
categories:
  - []
wordCount: 108
charCount: 2284
imgCount: 0
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 33 seconds
date: 2024-03-23 16:01:41
tags:
featured_image:
---

## 1、immediate 写错位置 哭晕
```ts
const reCompolist = ref([])
watch(
  () => props.list,
  () => {
    ;(reCompolist.value as any) = props.list.map(
      (row) => {
        const newRow: Record<string, any> = {}
        props.labels.forEach(([key, _]) => {
          newRow[key] = row[key]
        })
        return newRow
      },
      {
        immediate: true,
      }
    )
  }
)
```
