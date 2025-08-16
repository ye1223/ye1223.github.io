---
title: redux-toolkit学习
tags: react
# categories: gallery
featured_image: https://redux-toolkit.js.org/img/redux.svg
wordCount: 592
charCount: 12752
imgCount: 2
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 3 minutes
date: 2023-07-20 16:28:42
---

# Redux-Toolkit

> 摘自官网
>
> **[Redux Toolkit](https://redux-toolkit.js.org/)** 是 Redux 官方强烈推荐，开箱即用的一个高效的 Redux 开发工具集。
>
> 简化最常见场景下的 Redux 开发，
>
> ​	包括配置 store、定义 reducer，不可变的更新逻辑
>
> ​	可以立即创建整个状态的 “切片 slice”，而**无需手动编写任何 action creator 或者 action type**
>
> 自带了一些最常用的 Redux 插件，
>
> ​	例如用于异步逻辑 Redux Thunk，
>
> ​	用于编写选择器 selector 的函数 Reselect （可缓存select数据）



在 Redux 中,**切片(Slice)**指的是使用 **createSlice** API 创建的 reducer 和 action 的组合。它是 Redux Toolkit 中的一个核心概念。

createSlice 接收一个配置对象参数,里面包含:

> - 初始 state
> - reducers:包含不同 reducer 的对象
> - extraReducers:处理 action 的 reducer 函数



- 切片让我们可以**把 reducer 与 action 打包在一起**
- 创建**简化**了编写 **reducer** 的流程
- **自动生成 action** 类型



⚙️ CRA新建一个react项目

`create-react-app myapp --template typescript`

⚙️ 安装react-redux react-toolkit

`npm install @reduxjs/toolkit react-redux `



|                         项目基本结构                         |
| :----------------------------------------------------------: |
| <img src="https://s2.loli.net/2023/07/14/v4muItCeOgTFfaV.png" alt="image-20230714095149784.png" style="zoom:67%;" /> |



* 新建状态管理主文件

```ts
// store.ts
import { configureStore } from "@reduxjs/toolkit"
import counterReducer from "./slice/counterSlice" // 由切片导入的reducer

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
})

// 根据store本身推断出推断 `RootState` 和 `AppDispatch`类型
export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch
```



* 在项目入口文件，给App组件**注入store**

```tsx
// index.tsx
//....
import { store } from './redux/store'
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
    <Provider store={store}>
      <App />
    </Provider>
);
```

* 创建切片

> 之前利用 都要分别写各自actionCreator和各自的reducer， 现在只用一个切片即可管理

 😇 React Toolkit 创建的 Slice 状态state本身是 `immutable` 的，所以可以放心的直接加工使用

```tsx
// src/redux/slice/counterSlice.ts
import { createSlice } from "@reduxjs/toolkit"
import { PayloadAction } from "@reduxjs/toolkit"

interface ICounterState {
    value: number
}
const initialState: ICounterState = {
    value: 1
}

// 创建切片
export const counterSlice = createSlice({
    name: 'counter',
    initialState,
    reducers: {
        increment: (state) => {
            state.value += 1
        },
        decrement: (state) => {
            state.value -= 1
        },
        incrementByAmount: (state, action: PayloadAction<number>) => {
            state.value += action.payload
        },
    }
})

// Action creators 会在各自的reducer函数中自动创建， 这里直接导出
export const { increment, decrement, incrementByAmount } = counterSlice.actions

// 💡注意> 默认导出的是reducer
export default counterSlice.reducer
```



* 尝试在App.tsx组件中使用

```tsx
import React from 'react'
import { useDispatch, useSelector } from 'react-redux'
import { AppDispatch, RootState } from './redux/store'
import { decrement, increment, incrementByAmount } from './redux/slice/counterSlice'

const App: React.FC = () => {
  const count = useSelector((state: RootState) => state.counter.value)
  const disptach = useDispatch<AppDispatch>()
  return (
    <div className="App">
      {count}
      <button onClick={() => disptach(increment())}>点击我+1</button>
      <button onClick={() => disptach(decrement())}>点击我-1</button>
      <button onClick={() => disptach(incrementByAmount(100))}>点击我+100</button>

    </div>
  );
}

export default App;

```



✅成功

![屏幕录制2023-07-14-上午10.12.15](https://s2.loli.net/2023/07/14/QKWlncjEVHUgGRN.gif)


