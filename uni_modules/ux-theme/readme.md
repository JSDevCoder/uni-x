# ux-theme 主题

### 关于本插件

ux-theme计划提供基于uts和scss两种方式的主题，目前实现了uts模式，后续会增加scss模式的

### 方法详细

> new UxTheme()

|属性/方法   |类型 | 参数  |返回值|备注       |
|:---:    |:---:   |:---:  |:---: |:---:     |
|colors   |ColorSchema |  | |static属性  |
|textStyles   |TextStyleSchema|   | |static属性 |
|switchTheme| method | colors: ColorSchema, textStyles: TextStyleSchema| void |切换主题方法|
|$c| method | color: string|string|获取color|
|$ts|method|textStyles: string|TextStyle|获取textStyles|

### Uts模式使用方式  

> 将ux-theme以uni_modules的方式引入项目，在项目根目录下新建theme目录，新建index.uts 

```
import UxTheme from '@/uni_modules/ux-theme/js_sdk/ux-theme.uts'
import { TextStyle } from '@/uni_modules/ux-theme/js_sdk/text_style/type.uts'

const uxThemeIns = new UxTheme()

export { UxTheme, uxThemeIns, TextStyle }
```

> uxTheme默认提供了一些默认主题颜色、样式，可使用如下方式使用

```
//在需要使用的地方引入theme/index.uts
import { UxTheme, uxThemeIns, TextStyle } from '@/common/theme/index.uts'


// 在computed中使用
computed: {
	theme: ():UxTheme => uxThemeIns
}

// 直接在template中使用
<view :style="{
	backgroundColor: theme.$c('darkPrimary')
}">
	测试theme的使用
</view>
```

### Uts模式扩展

UxTheme类提供了switchTheme()方法，用来切换当前主题  

> 自定义主题  
 ```
 // theme/index.uts
 import { ColorSchema } from '@/uni_modules/ux-theme/js_sdk/color/type.uts'
 import { TextStyleSchema, TextStyle } from '@/uni_modules/ux-theme/js_sdk/text_style/type.uts'
 import UxTheme from '@/uni_modules/ux-theme/js_sdk/ux-theme.uts'
 
 const uxThemeIns = new UxTheme()
 
 // 扩展主题，需要同时实现类型ColorSchema和TextStyleSchema
 const customColors = {
	 // ...
 } as ColorSchema
 
 const customTextStyles = {
	 // ...
 } as TextStyleSchema
 
 uxThemeIns.switchTheme(customColors, customTextStyles)
 
 export { UxTheme, uxThemeIns, TextStyle, ColorSchema, TextStyleSchema }
 ```