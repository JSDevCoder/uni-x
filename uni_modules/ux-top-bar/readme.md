# ux-navbar

顶部导航栏组件

### 使用方式

```
<ux-top-bar title="自定义导航栏"></ux-top-bar>
```

### 参数

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---    | :---:   | :---:     | :---:      |:---:      |
| back | boolean | false | 否 | 是否有返回按钮 |
| color | string | #000 | 否 | 标题文本颜色 |
| frontColor | string | #000000 | 否 | 状态栏前景色，只支持#ffffff和#000000 |
| bgColor | string | #fff | 否 | 导航栏背景色 |
| position | boolean | false | 否 | 是否开启定位 |
| title | string | '' | 否 | 导航栏标题文本 |
| leftMenus | Menu[] | [] | 否 | 左侧按钮 |
| rightMenus | Menu[] | [] | 否 | 右侧按钮 |

### 事件

| 事件名    | 参数   | 备注 |
| :---    | :---:   | :---:       |
| @on-left-menus | index: number |  左侧按钮点击事件，index为按钮下标 |
| @on-right-menus | index: number |  右侧按钮点击事件，index为按钮下标 |

### Slots

| slot       | 备注 |
| :---    | :---:       |
| @slot left-menus |  可自定义左侧按钮 |
| @slot title |  自定义标题文本 |
| @slot right-menus |  自定义右侧按钮 |

### Menu类型

| type       |  备注 |
| :---    | :---:       |:---:       |
| icon: string |  按钮图标 |
| text: string |  按钮文本 |
| color: string |  按钮颜色 |
| size: number |  按钮图标大小 |

> 注意：icon默认使用的[ux-icons图标](https://ext.dcloud.net.cn/plugin?id=15699)  

> 注意：本组件需要关联[ux-statusbar组件](https://ext.dcloud.net.cn/plugin?id=15760)  
