# 直接拨打电话 wa-call

uts插件，仅支持Android，主要为解决uni-app x还不支持`uni.makePhoneCall`的替代插件。

未在Android5上测试。

因需要电话权限，在HBuilderX 3.97以前，需要打包自定义基座。3.97起的标准基座已包含该权限。

本插件只有一个API，调用非常简单。工程下引入插件，在页面里的调用方式如下：
```vue
<template>
	<view style="align-items: center;">
		<input class="input" type="tel" @input="bindInput" placeholder="请输入电话号码" />
		<button @tap="makePhoneCall" :disabled="disabled">直接拨打</button>
	</view>
</template>

<script>
	import {makePhoneCall} from "@/uni_modules/wa-call";
	export default {
		data() {
			return {
				disabled: true,
				inputValue: ""
			}
		},
		methods: {
			bindInput: function (e : InputEvent) {
				this.inputValue = e.detail.value
				if (this.inputValue.length > 2) {
					this.disabled = false
				} else {
					this.disabled = true
				}
			},
			makePhoneCall: function () {
				makePhoneCall(this.inputValue)
			}
		}
	}
</script>

<style>
	.input {
		height: 44px;
		border-bottom: 1rpx solid #E2E2E2;
		text-align: center;
		margin: 4px;
	}
</style>
```