<template>
	<view class="content">
		<easy-hover iconUrl="http://image.xichengame.net/xichengame/1603085706758505.gif" :circle="false" :height="100"
		 :initMarginTop="50" :width="100" @taped="triged" :stickSide="true" initSide="right"></easy-hover>
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
	</view>
</template>

<script>
	import easyHover from '../../components/easy-hover.vue'
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {
			triged() {
				console.log('组件内点击了')
			}

		},
		components: {
			easyHover
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color: #333333;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
