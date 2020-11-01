<template>
	<view class="hover-wrapper" :style="{'width':width+'rpx','height':height+'rpx','border-radius':circle?'50%':'0','top':top+'px','left':left+'px'}"
	 @touchmove.prevent="touchmove" @touchend="touchend" @tap="doTap">
		<image :src="iconUrl" mode="aspectFill" class="icon" :style="{'width':width+'rpx','height':height+'rpx','border-radius':circle?'50%':'0'}"></image>
	</view>
</template>

<script>
	export default {
		name: 'easy-hover',
		props: {
			/**
			 * 初始化方向
			 */
			initSide: {
				type: String,
				default: 'right'
			},
			/**
			 * 初始化距离上部距离rpx
			 */
			initMarginTop: {
				type: Number,
				default: 100
			},
			/**
			 * 图标地址
			 */
			iconUrl: {
				type: String,
				default: ''
			},
			/**
			 * 宽度
			 */
			width: {
				type: Number,
				default: 200
			},
			/**
			 * 高度
			 */
			height: {
				type: Number,
				default: 200
			},
			/**
			 * 是否是圆形
			 */
			circle: {
				type: Boolean,
				default: false
			},
			/**
			 * 是否贴边
			 */
			stickSide: {
				type: Boolean,
				default: true
			}

		},
		data() {
			return {
				screenWidthMax: 0,
				screenHeightMax: 0,
				widthMiddle: 0,
				xOffset: 0,
				yOffset: 0,
				menuHeight: 0,
				isMove: false,
				top: 0,
				left: 0,
				animation: {},
				animationData: {}
			}
		},
		methods: {
			/**
			 * 初始化参数封装，主要处理界面参数
			 */
			initParams() {
				let phoneParam = uni.getSystemInfoSync()
				this.screenWidthMax = phoneParam.screenWidth
				this.screenHeightMax = phoneParam.screenHeight
				this.widthMiddle = phoneParam.windowWidth / 2
				this.menuHeight = phoneParam.windowTop
				//计算偏移量
				this.xOffset = this.width * 375 / 750
				this.yOffset = this.height * 375 / 750
				//计算top和left
				this.top = this.menuHeight + (this.initMarginTop * 375 / 750)
				this.left = this.initSide === 'left' ? (this.xOffset / 2) : this.initSide === 'right' ? (this.screenWidthMax - this
					.xOffset / 2) : 0
			},
			/**
			 * 长按拖动
			 */
			touchmove(e) {
				this.isMove = true;
				let touch = e.touches[0] || e.changedTouches[0];
				this.left = touch.clientX;
				this.top = touch.clientY;
			},
			/**
			 * 长按结束
			 * 计算贴什么边,如果开启贴边则计算，否则不计算
			 * 计算时注意下边和右边要减去一半的偏移量
			 * 贴边计算时因为质心为中心，需要加上偏移量
			 */
			touchend(e) {
				//超过边界放置于边界，不属于贴边，属于通用
				let touch = e.touches[0] || e.changedTouches[0];
				//开启贴边，贴边原则，只要一遍碰到边就，只要过中线也贴
				if (this.stickSide) {
					//x方向小于贴边
					if (touch.clientX < this.xOffset / 2) {
						this.left = this.xOffset / 2
					}
					//x方向大于贴边
					if (touch.clientX < this.screenWidthMax - this.xOffset / 2) {
						this.left = this.screenWidthMax - this.xOffset / 2
					}
					//x中线贴边
					if (touch.clientX < this.widthMiddle) {
						this.left = this.xOffset / 2
					}
					if (touch.clientX > this.widthMiddle) {
						this.left = this.screenWidthMax - this.xOffset / 2
					}
					//y方向小于贴边
					if (touch.clientY < this.yOffset) {
						this.top = this.menuHeight
					}
					//y方向大于贴边
					if (touch.clientY > this.screenHeightMax - this.yOffset) {
						//需要计算偏移量
						this.top = this.screenHeightMax - this.yOffset
					}

				} else {
					if (touch.clientX < 0) {
						this.left = this.xOffset / 2
					}
					if (touch.clientX > this.screenWidthMax) {
						this.left = this.screenWidthMax - this.xOffset / 2
					}
					if (touch.clientY < 0) {
						this.top = this.menuHeight
					}
					if (touch.clientY > this.screenHeightMax) {
						//需要计算偏移量
						this.top = this.screenHeightMax - this.yOffset
					}
				}

			},
			/**
			 * 显示动画
			 */
			doAnimation(param, derection) {},
			/**
			 * 被点击
			 */
			doTap() {
				this.$emit('taped')
			}
		},
		created() {
			this.initParams()
		}
	}
</script>

<style lang="scss" scoped>
	.hover-wrapper {
		z-index: 1000;
		position: fixed;
		overflow: hidden;
		display: flex;
		transform: translate(-50%, 0);
		justify-content: center;
		align-items: center;
	}
</style>
