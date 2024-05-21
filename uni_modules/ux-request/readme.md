# ux-request 封装请求

1. 基于uts封装uni.request请求  
2. 带拦截器，支持多拦截器  
3. 支持uni-app x  

### 使用注意事项

1. 本插件支持Android、Web、Ios端  
2. ios端未测试，因为作者没有mac  
3. 因为官方的泛型只用于类型推断，无法作为值传递，所以封装本插件时，返回的数据均以UTSJSONObject方式操作，参考[UTSJSONObject文档](https://doc.dcloud.net.cn/uni-app-x/tutorial/request.html)
4. 如果有任何问题，可以在uni-im留言  

### 插件通用配置

| 参数名    | 类型    | 默认值       | 备注 |
| :---:    | :---:   | :---:          |:---:      |
| url | string |   | 请求的完整链接 |
| baseURL | string |   | 请求域名 |
| api | string |   | 请求的api接口 |
| method | RequestMethod | GET  | 请求方法 |
| data | UTSJSONObject|null |  请求的数据 |
| header | UTSJSONObject | {}  | 请求的header |


### 使用方式  

在项目根目录下创建common/request/index.uts配置拦截器，也可根据自己的项目自行配置  
创建拦截器需要注意的是：  
1. 定义的拦截器须继承UxInterceptor基类  
2. 继承之后，须重写beforeRequest/afterResponseSucc/afterResponseFail三个方法  
3. 重写之后，在各个方法里面实现自己的拦截逻辑（如下示例）  

| 方法名    | 备注 |
| :---:    |:---:      |
| beforeRequest | 请求之前拦截 |
| afterResponseSucc | 请求成功拦截 |
| afterResponseFail | 请求失败拦截 |

```
// 当前common/request/index.uts
import UxRequest from '@/uni_modules/ux-request/js_sdk/request.uts'
import UxInterceptor from '@/uni_modules/ux-request/js_sdk/interceptors.uts'
import { UxParam, UxRequestOptions, UxResponseFail } from '@/uni_modules/ux-request/js_sdk/types.uts'

// 请求拦截器示例
class HttpInterceptor extends UxInterceptor {
	constructor() {
		super()
	}
	override beforeRequest(config : UxRequestOptions) : UxRequestOptions {
		config.baseURL = 'http://127.0.0.1:8888' // 请填写自己的域名或者本机ip
		config.url = config.baseURL + config.api
		config.timeout = 50000
		config.header['Content-Type'] = 'application/x-www-form-urlencoded'
		return config
	}
	override afterResponseSucc(response : UTSJSONObject) : UTSJSONObject {
		return response
	}
	override afterResponseFail(error : UxResponseFail) : UxResponseFail {
		uni.showToast({
			title: '错误码: ' + error.errCode + ', 错误信息: ' + error.errMsg,
			icon: 'none'
		})
		return error
	}
}
const ins = UxRequest.create()
ins.addInterceptor(new HttpInterceptor())

export default ins
```
### 多拦截器说明

本插件支持多拦截器，按照ins.addInterceptor添加的顺序执行   
 
```
// 本插件支持多拦截器
import UxRequest from '@/uni_modules/ux-request/js_sdk/request.uts'
import UxInterceptor from '@/uni_modules/ux-request/js_sdk/interceptors.uts'
import { UxRequestOptions, UxResponseFail } from '@/uni_modules/ux-request/js_sdk/types.uts'

// 请求拦截器示例
class HttpInterceptor extends UxInterceptor {
	constructor() {
		super()
	}
	override beforeRequest(config : UxRequestOptions) : UxRequestOptions {
		config.baseURL = 'http://127.0.0.1:8888' // 请填写自己的域名或者本机ip
		config.url = config.baseURL + config.api
		config.timeout = 50000
		config.header['Content-Type'] = 'application/x-www-form-urlencoded'
		return config
	}
	override afterResponseSucc(response : UTSJSONObject) : UTSJSONObject {
		// 根据返回的数，在此处做请求状态处理
		// ...
		return response
	}
	override afterResponseFail(error : UxResponseFail) : UxResponseFail {
		uni.showToast({
			title: '错误码: ' + error.errCode + ', 错误信息: ' + error.errMsg,
			icon: 'none'
		})
		return error
	}
}

// 日志拦截器示例
class LogInterceptor extends UxInterceptor {
	constructor() {
		super()
	}
	override beforeRequest(config : UxRequestOptions) : UxRequestOptions {
		console.log('请求前拦截器日志：', config)
		return config
	}
	override afterResponseFail(error : UxResponseFail) : UxResponseFail {
		console.log('请求失败拦截器日志：', error)
		return error
	}
}
const ins = UxRequest.create()
ins.addInterceptor(new HttpInterceptor())
ins.addInterceptor(new LogInterceptor())

export default ins
```

### 定义sevices

```
// 当前common/request/apis/app.uts

import ins from "../index.uts"

export const getList = (): Promise<UTSJSONObject> => {
	return ins.getData('/project/list', null)
}
```

### 页面中使用

```
import { getList } from '@/common/request/apis/app.uts'

getList().then((res: UTSJSONObject)  => {
	console.log(res) // UTSJSONObject
})
```
