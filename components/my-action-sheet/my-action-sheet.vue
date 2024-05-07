<template>
	<view class="my-action-sheet" :style="{ zIndex, display }">
		<view
			class="my-action-sheet-mark"
			:animation="markAnimation"
			@click.stop="markClick()"
			:style="{ backgroundColor: markBackgroundColor, backdropFilter: markBackdropFilter }"
		></view>
		<view
			class="my-action-sheet-inner"
			:animation="innerAnimation"
			:style="{
				paddingTop: innerPaddingTop,
				paddingLeft: innerPaddingLeft,
				paddingRight: innerPaddingRight,
				paddingBottom: innerPaddingBottom,
				width,
				height
			}"
		>
			<slot></slot>
		</view>
	</view>
</template>

<script>
let markAnimation = null;
let innerAnimation = null;
let screenWidth = 0;
let screenHeight = 0;
export default {
	props: {
		show: { type: Boolean }, // 是否显示动态面板
		direction: { type: String, default: 'bottom' }, // 动态面板弹出方向
		zIndex: { type: Number, default: 9 }, // 层级
		markClose: { type: Boolean, default: true }, // 是否点击遮罩关闭
		markBackgroundColor: { type: String, default: 'rgba(0, 0, 0, 0.5)' }, // 遮罩背景色
		markBackdropFilter: { type: String, default: 'blur(5px) brightness(100%)' }, // 遮罩背景模糊
		topFit: { type: Boolean, default: false }, // 是否适配顶部安全距离
		leftFit: { type: Boolean, default: false },
		rightFit: { type: Boolean, default: false },
		bottomFit: { type: Boolean, default: false }
	},
	data() {
		return {
			width: 'auto',
			height: 'auto',
			display: 'none',
			markAnimation: null,
			innerAnimation: null,
			innerPaddingTop: 0,
			innerPaddingLeft: 0,
			innerPaddingRight: 0,
			innerPaddingBottom: 0
		};
	},
	watch: {
		show(newV) {
			if (newV) {
				this.showAnimation();
			} else {
				this.hideAnimation();
			}
		}
	},
	created() {
		this.init();
	},
	methods: {
		// 遮罩点击事件
		markClick() {
			if (this.markClose) {
				this.$emit('update:show', false);
				this.$emit('close');
			}
		},
		// 初始化
		init() {
			this.direction == 'bottom' || this.direction == 'top' ? (this.width = '100vw') : (this.height = '100vh');
			markAnimation = uni.createAnimation({
				duration: 200,
				timingFunction: 'ease-in-out',
				delay: 0
			});
			innerAnimation = uni.createAnimation({
				duration: 200,
				timingFunction: 'ease-in-out',
				delay: 0
			});
			uni.getSystemInfo({
				success: res => {
					screenWidth = res.screenWidth;
					screenHeight = res.screenHeight;
					this.topFit && (this.innerPaddingTop = res.safeAreaInsets.top + 'px');
					this.leftFit && (this.innerPaddingLeft = res.safeAreaInsets.left + 'px');
					this.rightFit && (this.innerPaddingRight = res.safeAreaInsets.right + 'px');
					this.bottomFit && (this.innerPaddingBottom = res.safeAreaInsets.bottom + 'px');
				}
			});
			this.hideAnimation();
			setTimeout(() => this.show && this.showAnimation(), 200);
		},
		// 显示动画
		showAnimation() {
			// #ifdef H5
			document.body.style.overflow = 'hidden';
			// #endif
			this.display = 'block';
			setTimeout(() => {
				markAnimation.opacity(1).step();
				this.markAnimation = markAnimation.export();
				switch (this.direction) {
					case 'bottom':
						innerAnimation.bottom(0).step();
						break;
					case 'top':
						innerAnimation.top(0).step();
						break;
					case 'left':
						innerAnimation.left(0).step();
						break;
					case 'right':
						innerAnimation.right(0).step();
						break;
				}
				this.innerAnimation = innerAnimation.export();
			}, 17);
		},
		// 隐藏动画
		hideAnimation() {
			// #ifdef H5
			document.body.style.overflow = 'visible';
			// #endif
			markAnimation.opacity(0).step();
			this.markAnimation = markAnimation.export();
			switch (this.direction) {
				case 'bottom':
					innerAnimation.bottom(-screenHeight).step();
					break;
				case 'top':
					innerAnimation.top(-screenHeight).step();
					break;
				case 'left':
					innerAnimation.left(-screenWidth).step();
					break;
				case 'right':
					innerAnimation.right(-screenWidth).step();
					break;
			}
			this.innerAnimation = innerAnimation.export();
			setTimeout(() => (this.display = 'none'), 200);
		}
	}
};
</script>

<style lang="scss" scoped>
.my-action-sheet {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	overflow: hidden;
	.my-action-sheet-mark {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}
	.my-action-sheet-inner {
		max-width: 100vw;
		max-height: 100vh;
		position: absolute;
		overflow: auto;
	}
}
</style>
