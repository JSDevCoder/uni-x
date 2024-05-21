# ux-validate 数据验证插件

本插件数据验证插件，可以与官方提供的Form组件无缝衔接，也可以用在自己的自定义表单组件中，另外提供一些常用验证函数，方便使用  

### 使用方式

从插件市场以uni_modules方式引入本插件  

##### 验证函数

本插件提供了以下验证函数，可以直接调用

isEmpty : (value : string) => boolean  
isMobile : (value : string) => boolean  
isPwd : (value : string) => boolean  
isEmail : (value : string) => boolean  
isMin : (value : string, len : number) => boolean  
isMax : (value : string, len : number) => boolean  
isBetween : (value : string, maxLen : number, minLen : number) => boolean  
isEqual : (val1 : string, val2 : string) => boolean  
isIp : (value : string) => boolean  
isUrl : (value : string) => boolean  
isIdCard : (value : string) => boolean  

下面两个为表单验证函数  
checkValid : (formData : Map<string, string>, rules : Map<string, Valid[]>) => boolean  
getError : () => string[]  

### checkValid 函数

|参数名     |类型                   | 默认值        |备注       |
|:---:      |:---:                 |:---:        |:---:       |
|formData   | Map<string, string>   |            | 要验证的表单数据     |
|rules      | Map<string, Valid[]>  |             | 验证规则   |

### Valid 类型

|参数名     |类型       | 是否为null        |备注       |
|:---:      |:---:      |:---:        |:---:       |
|require    | boolean  |       null    | 必填     |
|mobile     | boolean  |       null    | 手机号   |
|password   | boolean  |       null    | 密码   |
|min        | number   |       null    | 最小位数   |
|max        | number   |       null    | 最大位数   |
|equal      | boolean  |       null    | 两个值是否相等   |
|ip         | boolean  |       null    | ip地址   |
|url        | boolean  |       null    | url地址   |
|idcard     | boolean  |       null    | 身份证号   |
|msg        | boolean  |               | 验证提示信息   |

> formData使用示例  

```
// formData诗句使用Map<string, string>定义
const formData = new Map<string, string>()
formData.set('username', username)
formData.set('userpwd', userpwd)
```

> rules使用示例  

```
// formData诗句使用Map<string, Valid[]>定义
const rules = new Map<string, Valid[]>()
rules.set('username', [{
	require: true,
	msg: '用户名必填'
}] as Valid[])
rules.set('userpwd', [{
	password: true,
	msg: '密码格式不正确'
}] as Valid[])
```

需要注意的是，这里的密码验证模式为：  

```
// 最少包含小写字母、大写字母、数字、特殊符号，以及不能小于8位
const pwdRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/i
```

### 完整使用示例

```
import useValidate, { Valid } from '@/uni_modules/ux-validate/js_sdk/use-validate.uts'

/**
 * 验证用户登录表单
 * @param username {String} 用户名
 * @param userpwd {String} 用户密码
 * @returns { Array<string> | null} 成功返回null, 失败返回错误信息数组
 */
export function useLoginValid(username : string, userpwd : string) : Array<string> | null {
	// 表单数据
	const formData = new Map<string, string>()
	formData.set('username', username)
	formData.set('userpwd', userpwd)

	// 验证规则
	const rules = new Map<string, Array<Valid>>()
	rules.set('username', [{
		require: true,
		msg: '用户名必填'
	}] as Valid[])
	rules.set('userpwd', [{
		password: true,
		msg: '密码必填'
	}] as Valid[])
	
	const { checkValid, getError } = useValidate()
	const valid = checkValid(formData, rules);
	if (!valid) {
		return getError()
	}
	return null
}

```