# ux-request 封装请求

基于uni.request封装的带拦截器的请求

### 使用方式  

在项目根目录下创建request/index.uts，内容如下：  

```
// 当前request/index.uts

import { UxRequest } from '@/uni_modules/js_sdk/request.uts'

// 初始化时，参数配置
const reqParams = new Map<string, any>()
reqParams.set('baseURL', 'https://www.fastmock.site/mock/f3db82b0eeba8d6ec09d1947627bb194/uni-x1')
reqParams.set('header', {
	'Content-Type': 'application/json'
} as UTSJSONObject)

// 请求实例
const reqIns = new UxRequest(reqParams)

// 请求前拦截
reqIns.requestInterceptor.requestUse((config: Map<string, any>): void => {
	const data = config.get('data') as UTSJSONObject
	// 修改data参数
	config.set('data', data)
})

// 响应拦截
reqIns.responseInterceptor.responseUse((res: RequestSuccess<UTSJSONObject>): void => {
	// 走到这里的都是200 500 404 403之类的
	// 在这里只需要判断逻辑出错即可，并给于提示
	if(res.statusCode == 200) {
		const data = res.data
		if(data?.get('code') != '1000') {
			let msg = data?.getString('msg')
			uni.showToast({
				title: msg != null ? msg : '请求出错',
				icon: 'none'
			})
		}
	} else {
		uni.showToast({
			title: '请求的接口出错或资源不存在，请重试',
			icon: 'none'
		})
	}
}, (error: string) => {
	console.log('error', error)
	// 走到这里的，说明访问的ip地址有问题，访问不了，请求未通
	uni.showToast({
		title: error,
		icon: 'none'
	})
})

export default reqIns
```

api中使用  

```
// 当前request/apis/app.uts

/**
 * 发送验证码
 * @param mobile {String} 手机号
 */
export function sendSms(mobile : string) : Promise<UTSJSONObject> {
	return ins.post('/api/app/send_sms', { mobile } as UTSJSONObject, null)
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
			if (res.getString('code') == '1000') {
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