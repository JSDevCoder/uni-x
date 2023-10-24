<template>
	<view>
		<view class="ux-popup" v-if="isShow">
			<view class="ux-popup-content" :class="[type]">
				<!-- 内容区域 -->
				<slot></slot>
			</view>
			<!-- 遮罩层 -->
			<view class="ux-popup-mask" @tap="isCloseMask">zhe zhao</view>
		</view>
	</view>
</template>
<script>
	export default {
		name: 'ux-popup',
		data() {
			return {
				isShow: false
			}
		},
		props: {
			type: {
				type: String,
				default: 'bottom'
			},
			closeMask: {
				type: Boolean,
				default: true
			}
		},
		computed: {

		},
		methods: {
			isCloseMask() {
				if (!this.closeMask) return
				this.isShow = false
			},
			open() {
				this.isShow = true
			}
		}
	}
</script>
<style>
	.ux-popup {
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		z-index: 9999;
		background-color: rgba(0, 0, 0, 0);
	}

	.ux-popup-content {
		position: absolute;
		left: 0;
		right: 0;
		z-index: 99;
	}

	.ux-popup-content.bottom {
		bottom: 0;
	}

	.ux-popup-mask {
		position: absolute;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		z-index: 88;
		background-color: rgba(0, 0, 0, 0.3);
	}
</style>