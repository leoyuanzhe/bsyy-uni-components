<template>
	<view class="my-animation" :animation="animation" :style="{ display }"><slot></slot></view>
</template>

<script>
let animation = null;
export default {
	props: {
		show: { type: Boolean, default: true },
		type: { type: String, default: 'fade' }, // 动画类型
		duration: { type: Number, default: 200 }, // 动画时长
		timingFunction: { type: String, default: 'ease-in-out' }, // 动画时机
		delay: { type: Number, default: 0 } // 延迟
	},
	data() {
		return {
			animation: null,
			display: 'none'
		};
	},
	watch: {
		show(newV) {
			this.show ? this.showAnimation(true) : this.hideAnimation(true);
		}
	},
	created() {
		this.init();
	},
	methods: {
		init() {
			animation = uni.createAnimation({
				duration: this.duration,
				timingFunction: this.timingFunction,
				delay: this.delay
			});
			this.show ? this.showAnimation() : this.hideAnimation();
		},
		showAnimation() {
			if (this.type == 'fade') {
				this.display = 'block';
				setTimeout(() => {
					animation.opacity(1).step();
					this.animation = animation.export();
				}, 17);
			}
		},
		hideAnimation() {
			if (this.type == 'fade') {
				animation.opacity(0).step();
				this.animation = animation.export();
				setTimeout(() => {
					this.display = 'none';
				}, this.duration);
			}
		}
	}
};
</script>
