# ux-bottom-bar 底部带凸起按钮导航栏

### 使用方式

以uni_modules方式导入该组件, 本组件支持三端（android/ios/web） 

#### 在需要的地方使用该组件  

```vue
<ux-bottom-bar :tabs="customTabs" @on-tab-active="onTabActive"></ux-bottom-bar>
```

#### 组件参数解析  

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| color | String | #333 | 否 | 默认颜色 |
| selectedColor | String | ##007aff | 否 | 选中颜色 |
| bgColor | String | #fff | 否 | 背景颜色 |
| iconSize | Number | 14 | 否 | 图标默认大小 |
| fontSize | Number | 14 | 否 | 文本默认字体大小 |
| scale | Number | 1 | 否 | 凸起按钮大小调控参数 |
| curr | Number | 0 | 否 | 默认选择选项 |
| tabs | UxBottomItem[] | [] | 必传 | 详细参数如下表格所示 |

#### UxBottomItem类型参数解析  

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| imagePath | String&null | null | 否 | 默认图片路径 |
| selectedImagePath | String&null | null | 否 | 选中图片路径 |
| text | String&null | null | 否 | 默认文本 |
| middle | Boolean | false | 是 | 设置某一项是否凸起按钮 |
| icon | string&null | null | 否 | 默认使用icon图标 |
| selectedIcon | string&null | null | 否 | 选中时使用icon图标 |

> 备注： 

> 本组件中的图标支持iconfont图标和image图片，二选一即可  

> icon：使用的[ux-icons图标组件](https://ext.dcloud.net.cn/plugin?id=15699)，只需要将图标名传给icon参数，则默认启用图标  

> imagePath: 可使用自定义图片  

> middle参数：如果该参数为true，默认屏蔽text参数  

> 如果所有选项的middle都设置为false，则为普通的tab切换  

> 如果要设置凸起按钮，ux-bottom-bar的选项数量最少是3个 ，最中间的项，设置middle：true即可  

> 除了middle必填外，其他参数都是可选项  

#### 事件  

| 事件名    | 类型    | 参数    | 参数类型    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| @on-tab-active | tap | index | Number | 点击选项时触发 |

#### 使用示例 

1. 在/pages/index/index页面中直接使用组件  

```
<ux-top-bar :title="currTitle"></ux-top-bar>

<!-- #ifdef APP -->
<scroll-view style="flex: 1">
<!-- #endif -->
	<component :is="currPage"></component>
<!-- #ifdef APP -->
</scroll-view>
<!-- #endif -->


<ux-bottom-bar :tabs="tabs" @on-tab-active="onTabActive"></ux-bottom-bar>
```

2. 配置参数

```
import { UxBottomItem } from '@/uni_modules/ux-bottom-bar/components/ux-bottom-bar/types/index.uts';
// tab对应的页面级组件
import home from './components/home.uvue'
import comps from './components/comps.uvue'
import apis from './components/apis.uvue'
import mine from './components/mine.uvue'

export default {
	components: {
		home,
		comps,
		apis,
		mine
	},
	data() {
		return {
			// 当前点击的tab下标
			idx: 0,
			// 当前页面
			currPage: 'home',
			// 当前页面标题
			currTitle: '首页',
			// tab对应的所有页面
			pages: ['home', 'comps', 'apis', 'mine'],
			// tab对应的所有页面的标题
			pageTitles: ['首页', '组件', 'API', '我的'],
			// tabs数据
			tabs: [{
				text: '首页',
				icon: 'all',
				selectIcon: 'link',
				middle: false
			}, {
				text: '组件',
				icon: 'all',
				selectIcon: 'link',
				middle: false
			}, {
				text: 'API',
				icon: 'discount',
				selectIcon: 'discount',
				middle: false
			}, {
				text: '我的',
				icon: 'bussiness-man',
				selectIcon: 'bussiness-man',
				middle: false
			}] as UxBottomItem[]
		}
	},
	
	methods: {
		onTabActive(index: number) {
			console.log(index)
			this.idx = index
			this.currPage = this.pages[index]
			this.currTitle = this.pageTitles[index]
		}
	}
}
```

3. 一些说明  

如果后续小程序平台不支持component组件，则可以使用v-if来替代  

```
<home v-if="idx == 0"></home>
<comps v-if="idx == 1"></comps>
<apis v-if="idx == 2"></apis>
<mine v-if="idx == 3"></mine>
```