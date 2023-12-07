# ux-request 封装请求

基于uni.request封装的带拦截器的请求  

### new UxRequest时可配置的一些参数

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| baseURL | string |  | 是 | 请求域名 |
| api | string |  | 是 | 请求的api接口 |
| method | RequestMethod | GET | 否 | 请求方法 |
| data | UTSJSONObject | {} | 否 | 请求的数据 |
| header | UTSJSONObject | {} | 否 | 请求的headers |

### 需要说明的一些问题

一、关于请求成功之后返回的业务逻辑数据：   

```
{
    "cookies": [],
    "data": {
        "code": 1000,
        "data": "123456",
        "msg": "找回密码成功"
    },
    "header": {
        "access-control-allow-credentials": "true",
        "connection": "keep-alive",
        "content-length": "58",
        "content-type": "application/json; charset=utf-8",
        "date": "Thu, 07 Dec 2023 03:47:35 GMT",
        "etag": "W/\"3a-EuzQv5dmM0vEsEM+q0p1aaqnS6k\"",
        "server": "nginx/1.17.8",
        "set-cookie": "connect.sid=s%3Ae5n7Pr_MPPN-FsRPTRDnz9pzPwnK5gxi.wwne4O8XMwbh19LiMT4qqPJ4Zt5FN%2BNbdCZ7VUWlgfc; Path=/; Expires=Thu, 07 Dec 2023 15:47:35 GMT; HttpOnly",
        "vary": "Accept, Origin, Accept-Encoding",
        "x-powered-by": "Express"
    },
    "statusCode": 200
}
```

这里需要注意的是，第一层返回的data的类型，官方提供的是T，需要使用者自行定义类型，本插件默认定义如下  

| 参数名    | 类型    | 默认值    | 是否必传    | 备注 |
| :---:    | :---:   | :---:     | :---:      |:---:      |
| code | number |  | 是 | 请求的业务逻辑code |
| data | any |  | 是 | 请求的业务逻辑数据  |
| msg | string | 是 | 否 | 请求返回的提示信息 |

如果有用户跟本插件定义的类型不一致，如果在不能更改的情况下，请打开插件目录下的interceptors.uts文件，修改DataResult类型代码即可  
如非必要，请勿修改！  

二、前置拦截器的拦截逻辑  

拦截器config > request/post/get方法参数 > new Request()参数 > 默认参数  

### 使用方式  

在项目根目录下common/request/index.uts，配置拦截器  

```
// 当前common/request/index.uts

import { UxRequest, DataResult, FailInfo, SuccessInfo } from '@/uni_modules/ux-request/js_sdk/request.uts'

// 初始化时，参数配置
const reqParams = new Map<string, any>()
reqParams.set('baseURL', 'https://www.fastmock.site/mock/f3db82b0eeba8d6ec09d1947627bb194/uni-x')
reqParams.set('header', {
	'Content-Type': 'application/json'
} as UTSJSONObject)

// 请求实例
const reqIns = new UxRequest(reqParams)

// 请求前拦截
reqIns.interceptors.request.use((config : Map<string, any>) : Map<string, any> => {
	console.log('config ======> ', config)
	return config
})

// 响应拦截
reqIns.interceptors.response.use(
	(res : SuccessInfo) : SuccessInfo => {
		// 走到这里的都是200 500 404 403之类的
		// 在这里只需要判断逻辑出错即可，并给于提示
		console.log('res ======> ', res)
		return res
	},
	(err : FailInfo) : FailInfo => {
		// 走到这里的，说明服务端有问题
		console.log('err ======> ', err)
		return err
	}
)

export { reqIns, DataResult }
```

api中使用  

```
// 当前common/request/apis/app.uts

import {reqIns, DataResult} from '../index.uts'

/**
 * 发送验证码
 * @param mobile {String} 手机号
 */
export function sendSms(mobile : string) : Promise<DataResult> {
	return reqIns.post('/api/app/send_sms', { mobile } as UTSJSONObject, null)
}
```

页面中使用  

```
// 当前pages/users/user-register.uvue

import { sendSms } from '@/request/apis/app.uts';

methods: {
	// 发送验证码
	sendSms() {
		if (this.time < 60) return
		const result = useSmsValid(this.mobile)
		if (result != null) {
			uni.showToast({
				title: result[0],
				icon: 'none',
				position: 'top'
			})
			return
		}
		if(this.password != this.repassword) {
			uni.showToast({
				title: '密码和重复密码不一致',
				icon: 'none',
				position: 'top'
			})
			return
		}
		uni.showLoading({
			title: 'loading'
		})
		sendSms(this.mobile).then(res => {
			if (res.code == 1000) {
				this.timer = setInterval(() => {
					if (this.time <= 0) {
						clearInterval(this.timer as number)
						this.time = 60
						this.sendText = '发送验证码'
					} else {
						this.time -= 1
						this.sendText = this.time + 's后重发'
					}
				}, 1000)
				uni.showToast({
					title: '手机验证码已发送，请查收',
					icon: 'none',
					position: 'top'
				})
			}
		}).finally(() => {
			uni.hideLoading()
		})
	},
}
```