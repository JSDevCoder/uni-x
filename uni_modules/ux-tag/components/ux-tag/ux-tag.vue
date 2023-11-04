<template>
	<view class="ux-tag" :class="[`ux-tag-${type}`, `ux-tag-${size}`, `ux-tag-${shape}`]">
		<slot>
			<text class="ux-tag-text" :class="[`ux-tag-fs_${size}`]" @tap="onClick">{{text}}</text>
			<ux-icons v-if="closable" class="ux-tag-icon" name="times" color="#fff" @tap="onClose"></ux-icons>
		</slot>
	</view>
</template>
<script>
	export default {
		props: {
			type: {
				type: String,
				default: 'primary'
			},

			// tab 文本
			text: {
				type: String,
				default: '默认文本'
			},

			// tag 大小
			size: {
				type: String,
				default: 'base' // sm|base|large
			},

			// 圆形 tag
			shape: {
				type: String,
				default: 'square'
			},

			// 是否关闭标签
			closable: {
				type: Boolean,
				default: false
			}
		},

		methods: {
			/**
			 * 点击标签
			 */
			onClick() {
				this.$emit('on-click')
			},
			
			/**
			 * 点击关闭标签按钮
			 */
			onClose() {
				this.$emit('on-close')
			}
		}
	}
</script>
<style lang="scss">
	.ux-tag {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
		border-radius: 2px;

		&-text {
			color: #fff;
			flex-grow: 1;
			text-align: center;
		}
	}

	.ux-tag-primary {
		background-color: $ux-color-primary;
	}

	.ux-tag-success {
		background-color: $ux-color-success;
	}

	.ux-tag-error {
		background-color: $ux-color-error;
	}

	.ux-tag-warning {
		background-color: $ux-color-warning;
	}

	.ux-tag-sm {
		padding: 8rpx;
		border-radius: 2px;
	}

	.ux-tag-base {
		padding: 12rpx;
		border-radius: 4px;
	}

	.ux-tag-large {
		padding: 16rpx;
		border-radius: 4px;
	}

	.ux-tag-fs_sm {
		font-size: $ux-font-size-sm;
	}

	.ux-tag-fs_base {
		font-size: $ux-font-size-base;
	}

	.ux-tag-fs_large {
		font-size: $ux-font-size-lg;
	}

	.ux-tag-circle {
		border-radius: 50px;
	}
</style>