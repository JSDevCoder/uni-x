# ux-request
基于uni.request封装的带拦截器请求  
gitee地址： [uni-x/uni-request](https://gitee.com/hexinkuo/uni-x)  

```
import { UxRequest } from './request.uts'

/**
 * 初始化时，参数配置
 */
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