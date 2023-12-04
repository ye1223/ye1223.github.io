---
title: 表格导出为excel
categories:
  - []
tags:
  - enhanced
wordCount: 204
charCount: 4322
imgCount: 0
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 59 seconds
date: 2023-08-10 10:58:18
featured_image:
---

<strong>配合xlsx模块 + fileSaver模块</strong>

## ⚙️ 下载

`yarn add xlsx file-saver`

## 🔦 使用

> ​	tableData格式  [ { 	xxx: yyy 	},  ....]

```ts
import * as XLSX from 'xlsx'
import FileSaver from 'file-saver'

const excelTable = tableData.map(item => {
 	 //格式化一下表格数据
		return {
			'系统编号': item.id,
			'账单时间': item.billTime,
			'类型': item.natureName=== '收入'?'收入':'支出',
			'金额': item.natureName=== '收入'?item.amountMoney:-item.amountMoney,
			'备注': item.remarks
		}
	})
	const worksheet = XLSX.utils.json_to_sheet(excelTable)
	const workbook = XLSX.utils.book_new()
	XLSX.utils.book_append_sheet(workbook, worksheet, 'Sheet1')
	const excelBuffer = XLSX.write(workbook, {
		bookType: 'xlsx',
		type: 'array'
	})
	const blob = new Blob([excelBuffer], { type: 'application/vnd.ms-excel' })
	setTimeout(() => {
		FileSaver.saveAs(blob, '账单') // 下载文件 文件名
	}, 500)
```

> 本来是想利用WebWorker在另一线程中处理数据，~~在vite中还没配置好，on the way~~~