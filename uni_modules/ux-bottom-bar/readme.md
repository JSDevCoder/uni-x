# ux-bottom-bar 底部切换导航栏

### 使用方式
以uni_modules方式导入该组件  

> 1. 在需要的地方使用该组件  
```vue
<ux-bottom-bar :tabs="customTabs" @on-tab-active="onTabActive"></ux-bottom-bar>
```

> 2. 参数解析  

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| color | String | #333 | 否 | 默认颜色 |
| selectedColor | String | ##007aff | 否 | 选中颜色 |
| bgColor | String | #fff | 否 | 背景颜色 |
| iconSize | Number | 14 | 否 | 图标默认大小 |
| fontSize | Number | 14 | 否 | 文本默认字体大小 |
| scale | Number | 1 | 否 | 凸起按钮大小调控参数 |
| tabs | Icon[] | [] | 必传 | 详细参数如下表格所示 |

> 3. Icon类型参数解析  

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| iconPath | String |  | 是 | 默认图标路径 |
| selectedIconPath | String |  | 是 | 选中图标路径 |
| pagePath | String |  | 是 | 切换时组件路径 |
| text | String |  | 是 | 默认文本 |
| middle | Boolean | false | 是 | 是否凸起按钮 |
| icon | string |  | 是 | 是否使用icon图标 |

> 备注：   
> icon参数：使用的[ux-icons图标组件](https://ext.dcloud.net.cn/plugin?id=15699)，只需要将图标名传给icon参数，则默认启用图标，如果icon为空字符串，则默认使用iconPath和selectedIconPath  
> middle参数：如果该参数为true，默认屏蔽text参数

> 4. 事件

| 事件名    | 类型    | 参数    | 参数类型    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| @on-tab-active | tap | index | Number | 点击选项时触发 |

> 5. tabs参数示例

```
//type Icon[]的使用示例
customTabs: [
	{
		iconPath: "/static/images/tabbar/index.png",
		selectedIconPath: "/static/images/tabbar/index-active.png",
		pagePath: "components/home",
		text: "首页",
		middle: false,
		icon: 'all'
	},
	{
		iconPath: "/static/images/tabbar/middle.png",
		selectedIconPath: "/static/images/tabbar/middle-active.png",
		pagePath: "/pages/index/index",
		text: "",
		middle: true,
		icon: 'atm'
	},
	{
		iconPath: "/static/images/tabbar/mine.png",
		selectedIconPath: "/static/images/tabbar/mine-active.png",
		pagePath: "components/mine",
		text: "我的",
		middle: false,
		icon: 'account'
	},
] as BottomBarItem[]
```