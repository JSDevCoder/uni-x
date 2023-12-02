# ux-bottom-bar

### 使用方式
以uni_modules方式导入该组件  

1. 在需要的地方使用该组件  
```vue
<ux-bottom-bar :tabs="customTabs" @on-tab-active="onTabActive"></ux-bottom-bar>
// customTabs示例 ==>
customTabs: [
	{
		iconPath: "/static/images/tabbar/index.png",
		selectedIconPath: "/static/images/tabbar/index-active.png",
		pagePath: "components/home",
		text: "首页",
		middle: false
	},
	{
		iconPath: "/static/images/tabbar/middle.png",
		selectedIconPath: "/static/images/tabbar/middle.png",
		pagePath: "/pages/index/index",
		text: "按钮",
		middle: true
	},
	{
		iconPath: "/static/images/tabbar/mine.png",
		selectedIconPath: "/static/images/tabbar/mine-active.png",
		pagePath: "components/mine",
		text: "我的",
		middle: false
	},
] as BottomBarItem[]
```

2. 传递参数  
```uts
import type { PropType } from "vue"
import { BottomBarItem } from '../types/index.uts'
props: {
	tabs: {
		type: Array as PropType<BottomBarItem[]>,
		default() : BottomBarItem[] {
			return [] as BottomBarItem[]
		}
	}
},
```

3. BottomBarItem类型解释
```uts
export type BottomBarItem = {
	// 图片路径
	iconPath : string
	
	// 文本
	text : string
	
	// 选中项图片路径
	selectedIconPath : string
	
	// 对应的组件地址，以components开头，比如components/home
	pagePath : string
	
	// 是否中间凸起按钮，true-是|false-否
	middle : boolean
}
```

4. 点击选项事件
```uts
// 该组件对外暴露@on-tab-active事件
// @on-tab-active="onTabActive"
onTabActive(index: number) {
	this.idx = index
	const title = ['首页', '自定义', '我的']
	this.pageTitle = title[index]
}
```