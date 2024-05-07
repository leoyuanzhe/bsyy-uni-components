<template>
	<view class="circular-progress-bar">
		<canvas canvas-id="progress_bg" class="progress_bg"></canvas>
		<canvas canvas-id="progress_bar" class="progress_bar"></canvas>
	</view>
</template>

<script>
import { querySelector } from '@/tools/querySelector.ts';
export default {
	props: {
		totalAngle: { type: Number, default: 360 },
		value: { type: Number, default: 0 },
		bg: {
			type: Object,
			default: {
				lineWidth: 10,
				strokeStyle: '#000',
				radius: undefined
			}
		},
		bar: {
			type: Object,
			default: {
				lineWidth: 10,
				strokeStyle: '#fff',
				radius: undefined
			}
		},
		indicate: {
			type: Object,
			default: {
				strokeStyle: '#ff0000',
				radius: 20
			}
		}
	},
	data() {
		return {
			myBgRadius: 0,
			myBarRadius: 0,
			progress: 0,
			point: { x: 0, y: 0 }
		};
	},
	async mounted() {
		await this.init();
		this.drawProgressBg();
		this.drawProgressBar();
	},
	watch: {
		value(newV) {
			this.drawProgressBar();
		},
		totalAngle() {
			this.drawProgressBg();
			this.drawProgressBar();
		},
		bg: {
			handler(newV) {
				this.drawProgressBg();
			},
			deep: true
		},
		bar: {
			handler(newV) {
				this.drawProgressBar();
			},
			deep: true
		},
		indicate: {
			handler(newV) {
				this.drawProgressBar();
			},
			deep: true
		}
	},
	methods: {
		async init() {
			this.progress = (Math.PI * (this.totalAngle / 180)) / 100;
			const oCircularProgressBar = await querySelector('.circular-progress-bar', this);
			this.point.x = oCircularProgressBar.width / 2;
			this.point.y = oCircularProgressBar.height / 2;
			this.myBgRadius = this.bg.radius != undefined ? this.bg.radius : this.point.x - this.bg.lineWidth / 2;
			this.myBarRadius = this.bar.radius != undefined ? this.bar.radius : this.point.x - this.bar.lineWidth / 2;
		},
		// 画进度条背景
		drawProgressBg() {
			const ctx = uni.createCanvasContext('progress_bg', this);
			ctx.save();
			ctx.beginPath();
			ctx.setLineWidth(this.bg.lineWidth);
			ctx.setStrokeStyle(this.bg.strokeStyle);
			ctx.setLineCap('round');
			ctx.arc(this.point.x, this.point.y, this.myBgRadius, -Math.PI / 2, -Math.PI / 2 + this.progress * 100, false);
			ctx.stroke();
			ctx.draw();
			ctx.closePath();
			ctx.restore();
		},
		// 画进图条
		drawProgressBar() {
			const ctx = uni.createCanvasContext('progress_bar', this);
			// 画进度
			ctx.save();
			ctx.beginPath();
			ctx.setLineWidth(this.bar.lineWidth);
			ctx.setStrokeStyle(this.bar.strokeStyle);
			ctx.setLineCap('round');
			ctx.arc(this.point.x, this.point.y, this.myBarRadius, -Math.PI / 2, -Math.PI / 2 + this.value * this.progress, false);
			ctx.stroke();
			ctx.closePath();
			ctx.restore();
			// 画点
			ctx.save();
			ctx.beginPath();
			ctx.setFillStyle('#ff0000');
			ctx.setStrokeStyle('transparent'); // 线透明
			ctx.arc(20, 20, 20, 0, Math.PI * 2, false);
			ctx.fill();
			ctx.stroke();
			ctx.draw();
			ctx.closePath();
			ctx.restore();
		}
	}
};
</script>

<style lang="scss" scoped>
.circular-progress-bar {
	position: relative;
	width: 100%;
	height: 100%;
	.progress_bg,
	.progress_bar {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}
}
</style>
