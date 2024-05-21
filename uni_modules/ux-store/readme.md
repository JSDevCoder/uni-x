# ux-store 状态管理工具

借鉴Flux规范实现的简易状态管理工具  


### 使用方式

1. 在项目根目录下，创建stores目录  
2. 约定使用文件夹划分模块，以counter模块为例  
3. 在stores目录下，创建counter目录，作为counter模块  
4. 在counter目录下新建index.uts作为模块入口，创建state/reducer/action  
5. 创建counter模块的store实例，至此，一个完整的store创建完成

```
// 导入UxCreateStore
import UxCreateStore from "@/uni_modules/ux-store/js_sdk/store"
import { Action } from '@/uni_modules/ux-store/js_sdk/types'

// state类型
export type CounterState = {
	index : number
}

// state
export const counterInitialState : CounterState = { index: 0 }

// action
export const INCREMENT = 'INCREMENT'
export const DECREMENT = 'DECREMENT'

export function increment(num: number): Action {
	return { type: INCREMENT, payload: num } as Action
}

export function decrement(): Action {
	return { type: DECREMENT } as Action
}

// reducer
export const counterReducer = (state : CounterState, action : Action) : void => {
	switch (action.type) {
		case INCREMENT:
			state.index ++
			break
		case DECREMENT:
			state.index--
			break
		default:
			console.error('无效的action')
	}
}

// 创建store实例
export default new UxCreateStore<CounterState>(counterInitialState, counterReducer)

```
接下来，我们使用创建好的store  
  
```
<text>{{index}}</text>
<view @tap="add">点击添加</view>
```

```
import counterStore, { CounterState, increment } from '@/stores/counter/index.uts';

let index = ref(0)

function add() {
	console.log(counterStore)
	counterStore.subscribe((state) => {
		console.log('监听state变化', state)
	})
	counterStore.dispatch(increment(1))
	index.value = counterStore.useState((state : CounterState) : number => state.index) as number
}
```

我们可能会需要监听state的变化，以用来本地持久化或者其他操作
这个操作，我们可以放在stores/index.uts中完成，也可以放到页面逻辑中  

```
import counterStore from './counter/index.uts'

counterStore.subscribe((state) => {
	console.log('监听state变化', state)
})

```

### API文档

#### new UxCreateStore(initialState : S, reducer : (state : S, action : Action) => void)

该API用来创建Store

| 参数名    |详情 |备注 |
| :---:    |:---:  |:---:    |
| initialState | 提供一个初始化的state|state的类型由用户自己定义 |
| reducer | 提供一个用来操作antion的reducer| |
