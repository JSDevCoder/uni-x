# ux-theme 主题

### 关于本插件

ux-theme计划提供基于uts和scss两种方式的主题，目前实现了uts模式，后续会增加scss模式的

> useUxTheme()中可用的一些方法和属性

##### 类型说明

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

UxTheme，用来约束useUxTheme方法

|属性/方法名       |类型             | 默认值        |备注          |
|:---:         |:---:            |:---:        |:---:        |
|uxTheme       | UxThemeAttr | | |
|init          | () => void | | 初始化主题| 
|hasTheme      | (theme: string) => boolean | |判断主题是否存在  |
|makeTheme     | (theme: string, colors: ColorSchema, textStyles: TextStyleSchema) => void | |拓展或更新主题  |
|switchTheme      | (theme: string) => boolean | |切换主题，返回是否切换成功  |
|$c     | (color: string) => string | |获取一个颜色  |
|$t     | (style: string) => UTSJSONObject | |获取一个文本样式  |

UxThemeAttr 类型

|属性名       |类型             | 默认值        |备注          |
|:---:        |:---:            |:---:        |:---:        |
|colors       | ColorSchema     |             | 当前主题colors集合   |
|textStyles   | TextStyleSchema |             | 当前主题textStyles集合    |
|themes       | Theme[]         |             | 所有主题  |
|currTheme    | string          | default     | 当前主题名     |

### Uts模式使用方式  

> 将ux-theme以uni_modules的方式引入项目，在项目根目录下新建theme目录，新建index.uts 

```
// 当前common/theme/index.uts
// 引入useUxTheme
import useUxTheme from '@/uni_modules/ux-theme/js_sdk/ux-theme.uts'

const { init, makeTheme, switchTheme, $c, $t } = useUxTheme()

// 初始化主题
init()

// 将常用的一些方法导出，以备后用
export {init, makeTheme, switchTheme, $c, $t}
```

> 在项目中使用

```
// 当前pages/index/components/home.vue

import { $c, $t, switchTheme } from '@/common/theme/index.uts'

// 放在methods中，以备界面使用
methods: {
	// 获取颜色
	c(color: string): string {
		return $c(color)
	},
	
	// 获取文本样式
	t(style: string): UTSJSONObject {
		return $t(style)
	},
	
	// 切换主题
	st(theme : string) {
		switchTheme(theme)
	},
}

// template中使用
<view :style="{
	backgroundColor: c('primary')
}">
	测试theme的使用
	<text :style="t('smallTitle')">这是一个标题</text>

</view>
<view>
	<text>点击切换主题</text>
	<text @tap="st('default')">default</text>
	<text @tap="st('customTheme')">customTheme</text>
</view>
```

### Uts模式扩展新主题

useUxTheme函数提供了makeTheme(theme: string, color: ColorSchema, textStyles: TextStyleSchema)方法，用来扩展新主题  

> 自定义主题  

 ```
 // 当前common/theme/index.uts
 // 引入useUxTheme
 import useUxTheme from '@/uni_modules/ux-theme/js_sdk/ux-theme.uts'
 // 自定义colors
 import { customColors } from './customColors.uts'
 // 自定义textStyles
 import { customTextStyles } from './customTextStyles.uts'
 
 const { init, makeTheme, switchTheme, $c, $t } = useUxTheme()
 
 // 初始化主题
 init()
 
 // 新增主题
 makeTheme('customTheme', customColors, customTextStyles)
 
 // 将常用的一些方法导出，以备后用
 export {init, makeTheme, switchTheme, $c, $t}
 ```