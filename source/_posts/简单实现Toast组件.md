---
title: 简单实现Toast组件
categories:
  - []
tags:
  - enhanced
wordCount: 850
charCount: 17685
imgCount: 2
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 4 minutes
date: 2023-08-17 16:26:12
featured_image:
---

> * 一个模版文件Toast.vue
> * 一个渲染挂载导出方法文件index.ts
>   * 通过Toast.success('xxxx') 调用

这里UI使用`Flowbite`的[Toast](https://flowbite.com/docs/components/toast/#colors)组件（需要配合tailwindcss）

![image-20230817102433189.png](https://s2.loli.net/2023/08/17/hrfHPm5M8LAZcv4.png)



## 供给渲染的模板

```vue
<script lang="ts" setup>
import { ref } from 'vue'
interface Props {
	visible: boolean
	type: string
	message: string
}
const props = withDefaults(defineProps<Props>(), {
	visible: false,
	type: '',
	message: ''
})

const status = props.type === 'success' ? 'success' : 'danger'
const toast = ref<HTMLDivElement>()
const handleClose = () => {
	toast.value!.style.display = 'none'
}
</script>

<template>
	<Transition
		name="toast"
		enter-active-class="transition ease-out duration-300"
		leave-active-class="transition ease-in duration-300"
	>
		<div class="fixed inset-0 h-0" v-if="props.visible">
			<div
				:id="`toast-${status}`"
				class="absolute top-5 left-1/2 -translate-x-1/2 flex items-center w-full max-w-xs p-4 mb-4 text-gray-500 bg-white rounded-lg shadow dark:text-gray-400 dark:bg-gray-800"
				role="alert"
				ref="toast"
			>
				<div
					v-if="props.type === 'success'"
					class="inline-flex items-center justify-center flex-shrink-0 w-8 h-8 text-green-500 bg-green-100 rounded-lg dark:bg-green-800 dark:text-green-200"
				>
					<svg
						class="w-5 h-5"
						aria-hidden="true"
						xmlns="http://www.w3.org/2000/svg"
						fill="currentColor"
						viewBox="0 0 20 20"
					>
						<path
							d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5Zm3.707 8.207-4 4a1 1 0 0 1-1.414 0l-2-2a1 1 0 0 1 1.414-1.414L9 10.586l3.293-3.293a1 1 0 0 1 1.414 1.414Z"
						/>
					</svg>
					<span class="sr-only"> Check icon </span>
				</div>

				<div
					v-else-if="props.type === 'error'"
					class="inline-flex items-center justify-center flex-shrink-0 w-8 h-8 text-red-500 bg-red-100 rounded-lg dark:bg-red-800 dark:text-red-200"
				>
					<svg
						class="w-5 h-5"
						aria-hidden="true"
						xmlns="http://www.w3.org/2000/svg"
						fill="currentColor"
						viewBox="0 0 20 20"
					>
						<path
							d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5Zm3.707 11.793a1 1 0 1 1-1.414 1.414L10 11.414l-2.293 2.293a1 1 0 0 1-1.414-1.414L8.586 10 6.293 7.707a1 1 0 0 1 1.414-1.414L10 8.586l2.293-2.293a1 1 0 0 1 1.414 1.414L11.414 10l2.293 2.293Z"
						/>
					</svg>
					<span class="sr-only"> Error icon </span>
				</div>
				<div class="ml-3 text-sm font-normal">
					<!-- {/* 不同 */}  -->
					{{ props.message }}
				</div>
				<button
					type="button"
					class="ml-auto -mx-1.5 -my-1.5 bg-white text-gray-400 hover:text-gray-900 rounded-lg focus:ring-2 focus:ring-gray-300 p-1.5 hover:bg-gray-100 inline-flex items-center justify-center h-8 w-8 dark:text-gray-500 dark:hover:text-white dark:bg-gray-800 dark:hover:bg-gray-700"
					data-dismiss-target="#toast-success"
					aria-label="Close"
					@click="handleClose"
				>
					<span class="sr-only">Close</span>

					<svg
						class="w-3 h-3"
						aria-hidden="true"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 14 14"
					>
						<path
							stroke="currentColor"
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"
						/>
					</svg>
				</button>
			</div>
		</div>
	</Transition>
</template>

<style scoped>
.toast-enter-from,
.toast-leave-to {
	opacity: 0;
	transform: translateY(-30px);
}
</style>
```



## 导出toast方法，提供给Vue进行挂载

index.ts

```ts
//@ts-nocheck
import { createApp, h } from 'vue'
import ToastComponent from './template/Toast.vue'

interface ToastProps {
	visible: boolean
	type: 'success' | 'error'
	message: string
}

const ToastContainer = createApp({
	render() {
		return h(ToastComponent, this.toastProps)
	},
	data() {
		return {
			toastProps: {
				visible: false,
				type: '',
				message: '' 
			} as unknown as ToastProps
		}
	}
})

const toast = ToastContainer.mount(document.createElement('div'))

document.body.appendChild(toast.$el)

const Toast = {
	success(message: string) {
		toast.toastProps = {
			visible: true,
			type: 'success',
			message
		}
		setTimeout(() => {
			toast.toastProps.visible = false
		}, 3000)
	},
	error(message: string) {
		toast.toastProps = {
			visible: true,
			type: 'error',
			message
		}
		setTimeout(() => {
			toast.toastProps.visible = false
		}, 3000)
	}
}

export default Toast
```

## 👨‍💻使用

```ts
Toast.success('hello')
Toast.error('world')
```



![toast.gif](https://s2.loli.net/2023/08/17/ewqEhulLyJmUCKH.gif)

