# ux-theme 主题插件

### 关于

uxTheme是一个可以自定义主题的插件   
主题包含两个部分：ColorScheme自定义颜色和TextStyleSchema自定义文本样式  
目前可以实现项目中的各种颜色和文本样式的主题切换  
支持三端android、ios、web  

### 使用方式

1. 在项目common目录下，创建theme/index.uts文件  

```
import UxTheme from '@/uni_modules/ux-theme/js_sdk/ux-theme.uts'

const theme = UxTheme.getInstance()

// 初始化主题
theme.init()

export default theme
```

2. 如果想要在项目页面中使用，有两种方式：globalData全局定义和在页面中直接使用 

a. 在app.uvue中使用globalData全局定义  
```
import theme fromm '@/common/theme/index.uts'
export default {
    globalData: {
		theme: theme
    },
	
	onLaunch(){....}
}
```
```
<template>
	<ux-top-bar :title="currTitle" :bg-color="C('primary')" color="#fff"></ux-top-bar>
</template>

<script>

	export default {
		data() {
			return {
				primary: ''
			}
		},
		
		methods: {
			C(color: string): string {
				return getApp().globalData.theme.C(color)
			}
		}
	}
</script>
```
b. 在页面中直接调用    
```
<template>
	<ux-top-bar :title="currTitle" :bg-color="C('primary')" color="#fff"></ux-top-bar>
</template>

<script>
	import theme fromm '@/common/theme/index.uts'
	export default {
		data() {
			return {
				theme: theme
			}
		},
		
		methods: {
			C(color: string): string {
				return theme.C(color)
			}
		}
	}
</script>
```
3. 插件除了默认的主题，还提供了自定义扩展主题的方法: makeTheme(theme: string, colors: ColorSchema, textStyles: TextStyleSchema): void，可以在/common/theme/index.uts中调用此方法  

```
import { customColors } from './customColors.uts'
import { customTextStyles } from './customTextStyles.uts'
import UxTheme from '@/uni_modules/ux-theme/js_sdk/ux-theme.uts'

const theme = UxTheme.getInstance()

// 初始化主题
theme.init()

// 新增主题
theme.makeTheme('customTheme', customColors, customTextStyles)

export default theme
```

注意：新增主题，需要自定义实现ColorSchema和TextStyleSchema，以下是默认的ColorSchema和TextStyleSchema  

```
export type ColorSchema = {
	primary : string
	darkPrimary : string
	lightPrimary : string
	disabledPrimary : string
	error : string
	darkError : string
	lightError : string
	disabledError : string
	warning : string
	darkWarning : string
	lightWarning : string
	disabledWarning : string
	success : string
	darkSuccess : string
	lightSuccess : string
	disabledSuccess : string
	info : string
	darkInfo : string
	lightInfo : string
	disabledInfo : string
	fontMainColor : string
	fontContentColor : string
	fontTipsColor : string
	fontLightColor : string
	fontDisabledColor : string
	bgColor : string
	borderColor : string
}
```


```
export type TitleStyle = {
	fontSize: string
	fontWeight: number
}

export type TextStyleSchema = {
	largeTitle: TitleStyle
	baseTitle: TitleStyle
	smallTitle: TitleStyle
}

// 后续会新增更多文本样式
// ......
```

4. 当有多个主题时，插件切换主题的方法：switchThemme(theme: string): boolean  

```
<template>
	<ux-top-bar title="主题管理" :bg-color="C('primary')" color="#fff" :back="true"></ux-top-bar>
	<view class="container">
		<view class="title">切换主题</view>
		<view class="themes">
			<view class="theme" :style="{backgroundColor: idx == index ? '#fcbd71' : '#fff'}"
				v-for="(item, index) in themes" :key="item.theme" @tap="switchTheme(item.theme, index)">
				<view class="color" :style="{backgroundColor: item.colors.primary}"></view>
				<view class="color" :style="{backgroundColor: item.colors.darkPrimary}"></view>
				<view class="color" :style="{backgroundColor: item.colors.lightPrimary}"></view>
				<view class="color" :style="{backgroundColor: item.colors.disabledPrimary}"></view>
				<text class="text">{{item.theme}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	import theme from '@/common/theme/index.uts'
	import { Theme } from '@/uni_modules/ux-theme/js_sdk/types.uts';
	export default {
		data() {
			return {
				idx: 0,
				theme: theme,
				themes: [] as Theme[]
			};
		},
		
		created() {
			this.themes = theme.uxThemeAttr.themes
			this.idx = this.themes.map((v: Theme): string => v.theme).indexOf(theme.uxThemeAttr.currTheme)
		},

		methods: {
			// 获取主题色
			C(color: string): string{
				return theme.C(color)
			},
			
			// 切换主题
			switchTheme(currTheme: string, index: number) {
				this.idx = index
				theme.switchTheme(currTheme)
			}
		}
	}
</script>
```

### API解析

##### Theme类型说明

Theme，每一个主题的类型  

|属性名       |类型             | 默认值        |备注               |
|:---:        |:---:            |:---:        |:---:               |
|colors       | ColorSchema     |             |主题colors集合       |
|textStyles   | TextStyleSchema |             |主题textStyles集合   |
|theme        | string          |  default     |主题名              |

ColorSchema，主题的颜色  

`请自行查看：位于uni_modules/js_sdk/color`

TextStyleSchema，主题的文本样式  

`请自行查看：位于uni_modules/js_sdk/text_style`

#### UxTheme可使用的方法和属性

1. 方法  

|方法名       |类型             | 默认值        |备注          |
|:---:         |:---:            |:---:        |:---:        |
|init          | () => void | | 初始化主题| 
|hasTheme      | (theme: string) => boolean | |判断主题是否存在  |
|makeTheme     | (theme: string, colors: ColorSchema, textStyles: TextStyleSchema) => void | |拓展或更新主题  |
|switchTheme      | (theme: string) => boolean | |切换主题，返回是否切换成功  |
|C     | (color: string) => string | |获取一个颜色  |
|T     | (style: string) => UTSJSONObject | |获取一个文本样式  |

2. 属性 UxThemeAttr 

|属性名       |类型             | 默认值        |备注          |
|:---:        |:---:            |:---:        |:---:        |
|colors       | ColorSchema     |             | 当前主题colors集合   |
|textStyles   | TextStyleSchema |             | 当前主题textStyles集合    |
|themes       | Theme[]         |             | 所有主题  |
|currTheme    | string          | default     | 当前主题名     |
