<template>
	<view
		id="my-preview"
		class="my-preview"
		:animation="animation"
		@touchstart.stop="touchStart"
		@touchmove.stop="touchMove"
		@touchend.stop="touchEnd"
		@click.stop
		:style="{ zIndex, display }"
	>
		<image
			class="my-preview-image"
			:src="srcs[current]"
			mode="aspectFit"
			:style="{
				top: 'calc(50% - ' + offsetTop + 'px)',
				left: 'calc(50% - ' + offsetLeft + 'px)',
				transformOrigin,
				transform: 'translate(-50%, -50%) scale(' + scale + ')',
				transition
			}"
			@click="imageClick"
		/>
		<slot name="page">
			<text class="page" v-show="srcs.length > 1" @click="imageClick">{{ current + 1 }} / {{ srcs.length }}</text>
		</slot>
		<slot></slot>
	</view>
</template>

<script>
let offsetWidth = 0; // 预览组件的宽度
let pointX = 0; // 触摸屏幕的X坐标
let pointY = 0; // 触摸屏幕的Y坐标
let screenWidth = 0;
let screenHeight = 0;
let animation = null;
export default {
	props: {
		show: { type: Boolean, default: false },
		current: { type: Number, default: 0 },
		srcs: { type: Array, default: [] },
		zIndex: { type: [String, Number], default: '9' }
	},
	data() {
		return {
			scale: 1, // 缩放值
			offsetTop: 0, // image顶部偏移量
			offsetLeft: 0, // image左侧偏移量
			transition: 'none', // image过渡
			transformOrigin: 'center',
			display: 'none',
			animation: null,
			doubleClickTimer: null,
			closeTimer: null
		};
	},
	watch: {
		show(newV) {
			newV ? this.showAnimation() : this.hideAnimation();
		}
	},
	created() {
		uni.getSystemInfo({
			success: res => {
				screenWidth = res.screenWidth;
				screenHeight = res.screenHeight;
			}
		});
	},
	mounted() {
		this.init();
	},
	methods: {
		init() {
			animation = uni.createAnimation({
				duration: 200,
				timingFunction: 'ease-in-out',
				delay: 0
			});
			if (this.show) {
				this.showAnimation();
			} else {
				this.hideAnimation();
			}
		},
		// 点击图片
		imageClick(e) {
			if (this.doubleClickTimer) {
				this.imageDoubleClick(e);
			} else {
				this.doubleClickTimer = setTimeout(() => {
					this.doubleClickTimer = null;
				}, 180);
				if (this.closeTimer) {
					this.closeTimer = null;
				} else {
					this.closeTimer = setTimeout(() => {
						if (this.closeTimer) {
							this.closeTimer = null;
							this.hideAnimation();
						}
					}, 180);
				}
			}
		},
		// 双击图片
		imageDoubleClick(e) {
			this.closeTimer = null;
			this.transformOrigin = e.detail.x + 'px ' + e.detail.y + 'px';
			this.transition = 'all 0.2s';
			if (this.scale != 1) {
				// 缩小
				this.scale = 1;
			} else {
				this.scale = 2;
			}
			setTimeout(() => {
				this.transition = 'none';
			}, 200);
		},
		touchStart(e) {
			this.transition = 'none';
			this.offsetLeft = 0;
			this.offsetTop = 0;
			pointX = e.changedTouches[0].pageX;
			pointY = e.changedTouches[0].pageY;
		},
		touchMove(e) {
			this.offsetLeft = pointX - e.changedTouches[0].pageX;
			this.offsetTop = pointY - e.changedTouches[0].pageY;
		},
		touchEnd(e) {
			let dX = e.changedTouches[0].pageX - pointX; // 触摸开始与触摸结束后的距离
			let dY = e.changedTouches[0].pageY - pointY;
			if (dX > 200) {
				// 上一页
				if (this.current > 0) {
					this.scale = 1;
					this.offsetLeft = screenWidth;
					this.$emit('change', this.current - 1); // 翻页
					this.$emit('update:current', this.current - 1);
				} else {
					this.$emit('prevFail'); // 上翻失败
				}
			} else if (dX < -200) {
				// 下一页
				if (this.current + 1 < this.srcs.length) {
					this.scale = 1;
					this.offsetLeft = -screenWidth;
					this.$emit('change', this.current + 1); // 翻页
					this.$emit('update:current', this.current + 1);
				} else {
					this.$emit('nextFail'); // 下翻失败
				}
			}
			if (dY > 200) {
				// 下滑关闭
				this.hideAnimation();
			}
			setTimeout(() => {
				// 复位
				this.transition = 'all 0.2s';
				this.offsetLeft = 0;
				this.offsetTop = 0;
				pointX = 0;
				pointY = 0;
			}, 17);
		},
		// 显示动画
		showAnimation() {
			// #ifdef H5
			document.body.style.overflow = 'hidden';
			// #endif
			this.display = 'block';
			setTimeout(() => {
				animation.opacity(1).step();
				this.animation = animation.export();
				this.$emit('open', this.current);
			}, 17);
		},
		// 隐藏动画
		hideAnimation() {
			// #ifdef H5
			document.body.style.overflow = 'visible';
			// #endif
			animation.opacity(0).step();
			this.animation = animation.export();
			setTimeout(() => {
				this.display = 'none';
				this.scale = 1;
				this.$emit('close', this.current);
				this.$emit('update:show', false);
			}, 200);
		}
	}
};
</script>

<style lang="scss" scoped>
.my-preview {
	position: fixed !important;
	top: 0;
	left: 0;
	width: 100vw;
	height: 100vh;
	backdrop-filter: blur(20px) brightness(100%);
	background-color: rgba(0, 0, 0, 0.8);
	cursor: pointer;
	.my-preview-image {
		width: 100%;
		height: 100%;
		position: absolute;
		transform: translate(-50%, -50%);
	}
	.page {
		position: fixed;
		bottom: 50px;
		left: 50%;
		transform: translateX(-50%);
		color: #fff;
		font-size: 20px;
		font-weight: bold;
		text-shadow: 0 0 6rpx #000;
	}
	.close {
		position: fixed;
		top: 30px;
		right: 30px;
		color: #fff;
		font-size: 40px;
		font-weight: bold;
	}
}
</style>
