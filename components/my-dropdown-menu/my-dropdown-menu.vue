<template>
	<view class="my-dropdown-menu" :style="{ zIndex }">
		<view
			class="mark"
			:animation="markAnimation"
			:style="{ display, backgroundColor: markBackgroundColor, backdropFilter: markBackdropFilter }"
			@click.stop="markClick()"
		></view>
		<view
			v-if="direction == 'top' || direction == 'left'"
			class="content"
			:style="{
				position: contentAbsolute ? 'absolute' : 'reactive',
				top: contentTop,
				left: contentLeft,
				right: contentRight,
				bottom: contentBottom,
				width: contentWidth,
				height: contentHeight
			}"
		>
			<view class="inner" :animation="innerAnimation" :style="{ display }"><slot name="content"></slot></view>
		</view>
		<view class="menu"><slot name="menu"></slot></view>
		<view
			v-if="direction == 'bottom' || direction == 'right'"
			class="content"
			:style="{
				position: contentAbsolute ? 'absolute' : 'reactive',
				top: contentTop,
				left: contentLeft,
				right: contentRight,
				bottom: contentBottom,
				width: contentWidth,
				height: contentHeight
			}"
		>
			<view class="inner" :animation="innerAnimation" :style="{ display }"><slot name="content"></slot></view>
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
		show: { type: Boolean, default: false }, // 是否拉起下拉菜单
		zIndex: { type: [Number, String], default: 9 }, // 下拉菜单的层级
		direction: { type: String, default: 'bottom' }, // 菜单拉出的方向
		markClose: { type: Boolean, default: true }, // 是否点击遮罩关闭
		markBackgroundColor: { type: String, default: 'rgba(0, 0, 0, 0.5)' }, // 遮罩背景色
		markBackdropFilter: { type: String, default: 'blur(5px) brightness(100%)' }, // 遮罩背景模糊
		menuSelector: { type: String, default: '.my-dropdown-menu .menu' }, // 下拉框的css选择器
		contentAbsolute: { type: Boolean, default: true }, // 下拉框的定位属性
		duration: { type: Number, default: 200 }, // 动画时长
		timingFunction: { type: String, default: 'ease-in-out' }, // 动画时机
		delay: { type: Number, default: 0 } // 延迟
	},
	data() {
		return {
			markAnimation: null,
			innerAnimation: null,
			display: 'none',
			contentTop: 'auto',
			contentLeft: 'auto',
			contentRight: 'auto',
			contentBottom: 'auto',
			contentWidth: 'auto',
			contentHeight: 'auto'
		};
	},
	watch: {
		show(newV) {
			newV ? this.showAnimation() : this.hideAnimation();
		}
	},
	created() {
		this.init();
	},
	methods: {
		init() {
			this.direction == 'top' || this.direction == 'bottom' ? (this.contentWidth = '100%') : (this.contentHeight = '100%');
			uni.getSystemInfo({
				success: res => {
					screenWidth = res.screenWidth;
					screenHeight = res.screenHeight;
				}
			});
			markAnimation = uni.createAnimation({
				duration: this.duration,
				timingFunction: this.timingFunction,
				delay: this.delay
			});
			innerAnimation = uni.createAnimation({
				duration: this.duration,
				timingFunction: this.timingFunction,
				delay: this.delay
			});
			this.show ? this.showAnimation() : this.hideAnimation();
		},
		// 点击遮罩事件
		markClick() {
			if (this.markClose) {
				this.$emit('update:show', false);
				this.$emit('close');
			}
		},
		// 显示动画
		async showAnimation() {
			this.display = 'block';
			setTimeout(async () => {
				let oMenu = await new Promise((resolve, reject) => {
					uni.createSelectorQuery()
						.in(this)
						.select(this.menuSelector)
						.boundingClientRect(data => {
							data ? resolve(data) : reject(data);
						})
						.exec();
				});
				if (this.contentAbsolute && this.direction == 'top') {
					this.contentBottom = oMenu.height + 'px';
				}
				if (this.direction == 'left') {
					this.contentTop = 0;
					this.contentRight = oMenu.width + 'px';
				}
				if (this.direction == 'right') {
					this.contentTop = 0;
					this.contentLeft = oMenu.width + 'px';
				}
				markAnimation.opacity(1).step();
				this.markAnimation = markAnimation.export();
				if (this.direction == 'top') {
					innerAnimation.bottom(0).step();
					this.innerAnimation = innerAnimation.export();
				}
				if (this.direction == 'bottom') {
					innerAnimation.top(0).step();
					this.innerAnimation = innerAnimation.export();
				}
				if (this.direction == 'left') {
					innerAnimation.right(0).step();
					this.innerAnimation = innerAnimation.export();
				}
				if (this.direction == 'right') {
					innerAnimation.left(0).step();
					this.innerAnimation = innerAnimation.export();
				}
			}, 17);
		},
		// 隐藏动画
		async hideAnimation() {
			markAnimation.opacity(0).step();
			this.markAnimation = markAnimation.export();
			if (this.direction == 'top') {
				innerAnimation.bottom(-screenHeight).step();
				this.innerAnimation = innerAnimation.export();
			}
			if (this.direction == 'bottom') {
				innerAnimation.top(-screenHeight).step();
				this.innerAnimation = innerAnimation.export();
			}
			if (this.direction == 'left') {
				innerAnimation.right(-screenWidth).step();
				this.innerAnimation = innerAnimation.export();
			}
			if (this.direction == 'right') {
				innerAnimation.left(-screenWidth).step();
				this.innerAnimation = innerAnimation.export();
			}
			setTimeout(() => {
				this.display = 'none';
			}, this.duration);
		}
	}
};
</script>

<style lang="scss" scoped>
.my-dropdown-menu {
	position: relative;
	.mark {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
	}
	.menu {
		position: relative;
	}
	.content {
		overflow: hidden;
		right: 250px;
		.inner {
			position: relative;
			width: 100%;
			background-color: #fff;
		}
	}
}
</style>
