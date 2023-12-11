# ux-date 日期格式化SDK

### format 方法

`format: (time: string, format: string) => string`  

##### 参数

|参数名     |类型        | 默认值        |备注       |
|:---      |:---:       |:---:           |:---:       |
|datetime       | string     |               | 时间日期字符串、时间戳字符串     |
|format     | string     |                | 'YYYY-MM-DD hh:mm:ss WW'自由组合   |


### timeAgo 方法

`timeAgo: (time: number) => string`  

##### 参数

|参数名     |类型        | 默认值        |备注       |
|:---      |:---:       |:---:           |:---:       |
|time       | number     |               | 数字类型的时间戳    |

### 使用示例

```
import useDate from '@/uni_modules/ux-date/js_sdk/use-date.uts'

const { format, timeAgo } = useDate()

// format
console.log(format(new Date(), 'YYYY-MM-DD WW'))

//timeAgo
console.log(timeAgo(new Date().getTime() - 2 * 60 * 60 * 1000))
```


