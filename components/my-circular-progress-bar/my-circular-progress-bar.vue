<template>
	<view class="my-circular-progress-bar" :style="{ width, height }">
		<canvas canvas-id="progress_bg" class="progress_bg"></canvas>
		<canvas canvas-id="progress_bar" class="progress_bar"></canvas>
	</view>
</template>

<script>
let myBgRadius = 0; // 背景圈的半径
let myBarRadius = 0; // 进度条的半径
let progress = 0; // 1%的进度值
const point = { x: 0, y: 0 }; // 坐标原点
export default {
	props: {
		width: { type: String, default: '100px' },
		height: { type: String, default: '100px' },
		totalAngle: { type: Number, default: 360 }, // 总角度
		value: { type: Number, default: 0 }, // 当前进度
		// 背景进度参数
		bg: {
			type: Object,
			default: {
				show: true, // 是否显示
				radius: undefined, // 半径
				fillStyle: 'transparent', // 填充色
				lineWidth: 10, // 线宽
				strokeStyle: '#ccc' // 线的颜色
			}
		},
		// 当前进度参数
		bar: {
			type: Object,
			default: {
				show: true,
				radius: undefined,
				fillStyle: 'transparent',
				lineWidth: 10,
				strokeStyle: '#000'
			}
		},
		// 指示点参数
		indicate: {
			type: Object,
			default: {
				show: false,
				radius: 20,
				fillStyle: '#ff0000',
				lineWidth: 0,
				strokeStyle: 'transparent'
			}
		}
	},
	watch: {
		value(newV) {
			this.drawProgressBar();
		},
		totalAngle(newV) {
			this.drawProgressBg();
			this.drawProgressBar();
		},
		bg(newV) {
			this.drawProgressBg();
		},
		bar(newV) {
			this.drawProgressBar();
		},
		indicate(newV) {
			this.drawProgressBar();
		}
	},
	mounted() {
		this.init();
	},
	methods: {
		// 初始化
		async init() {
			progress = (Math.PI * (this.totalAngle / 180)) / 100;
			optionInit.call(this);
			const oCircularProgressBar = await new Promise((resolve, reject) => {
				uni.createSelectorQuery()
					.in(this)
					.select('.my-circular-progress-bar')
					.boundingClientRect(data => {
						data ? resolve(data) : reject(data);
					})
					.exec();
			});
			point.x = oCircularProgressBar.width / 2;
			point.y = oCircularProgressBar.height / 2;
			myBgRadius = this.bg.radius != undefined ? this.bg.radius : point.x - this.bg.lineWidth / 2;
			myBarRadius = this.bar.radius != undefined ? this.bar.radius : point.x - this.bar.lineWidth / 2;
			this.drawProgressBg();
			this.drawProgressBar();
			// 初始化配置项
			function optionInit() {
				!this.bg.show && (this.bg.show = true);
				!this.bg.radius && (this.bg.radius = undefined);
				!this.bg.fillStyle && (this.bg.fillStyle = 'transparent');
				!this.bg.lineWidth && (this.bg.lineWidth = 10);
				!this.bg.strokeStyle && (this.bg.strokeStyle = '#ccc');
				!this.bar.show && (this.bar.show = true);
				!this.bar.radius && (this.bar.radius = undefined);
				!this.bar.fillStyle && (this.bar.fillStyle = 'transparent');
				!this.bar.lineWidth && (this.bar.lineWidth = 10);
				!this.bar.strokeStyle && (this.bar.strokeStyle = '#000');
				!this.indicate.show && (this.indicate.show = false);
				!this.indicate.radius && (this.indicate.radius = 20);
				!this.indicate.fillStyle && (this.indicate.fillStyle = '#ff0000');
				!this.indicate.lineWidth && (this.indicate.lineWidth = 0);
				!this.indicate.strokeStyle && (this.indicate.strokeStyle = 'transparent');
			}
		},
		drawProgressBg() {
			if (this.bg.show) {
				const ctx = uni.createCanvasContext('progress_bg', this);
				ctx.save();
				ctx.beginPath();
				ctx.setFillStyle(this.bg.fillStyle);
				ctx.setLineWidth(this.bg.lineWidth);
				ctx.setStrokeStyle(this.bg.strokeStyle);
				ctx.setLineCap('round');
				ctx.arc(point.x, point.y, myBgRadius, -Math.PI / 2, -Math.PI / 2 + progress * 100, false);
				ctx.fill();
				ctx.stroke();
				ctx.closePath();
				ctx.restore();
				ctx.draw();
			}
		},
		// 画进图条
		drawProgressBar() {
			const ctx = uni.createCanvasContext('progress_bar', this);
			// 画进度
			if (this.bar.show) {
				ctx.save();
				ctx.beginPath();
				ctx.setFillStyle(this.bar.fillStyle);
				ctx.setLineWidth(this.bar.lineWidth);
				ctx.setStrokeStyle(this.bar.strokeStyle);
				ctx.setLineCap('round');
				ctx.arc(point.x, point.y, myBarRadius, -Math.PI / 2, -Math.PI / 2 + this.value * progress, false);
				ctx.fill();
				ctx.stroke();
				ctx.closePath();
				ctx.restore();
			}
			// 画指示点
			if (this.indicate.show) {
				console.log(this.indicate.show);
				let { x, y } = countXY(this.totalAngle * (this.value / 100), myBarRadius);
				ctx.save();
				ctx.beginPath();
				ctx.setFillStyle(this.indicate.fillStyle);
				ctx.setStrokeStyle(this.indicate.strokeStyle); // 线透明
				ctx.setLineWidth(this.indicate.lineWidth);
				ctx.arc(x + point.x - myBarRadius, y + point.y - myBarRadius, this.indicate.radius / 2, 0, Math.PI * 2, false);
				ctx.fill();
				ctx.stroke();
				ctx.closePath();
				ctx.restore();
			}
			ctx.draw();
			// 计算指示点的xy坐标
			function countXY(deg, r) {
				let x = 0;
				let y = 0;
				if (deg <= 90) {
					let hudu = degToHudu(deg);
					x = r + r * Math.sin(hudu);
					y = r - r * Math.cos(hudu);
				} else if (deg <= 180) {
					let hudu = degToHudu(deg - 90);
					x = r + r * Math.cos(hudu);
					y = r + r * Math.sin(hudu);
				} else if (deg <= 270) {
					let hudu = degToHudu(deg - 180);
					x = r - r * Math.sin(hudu);
					y = r + r * Math.cos(hudu);
				} else {
					let hudu = degToHudu(deg - 270);
					x = r - r * Math.cos(hudu);
					y = r - r * Math.sin(hudu);
				}
				return { x, y };
				function degToHudu(deg) {
					return deg * (Math.PI / 180);
				}
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.my-circular-progress-bar {
	position: relative;
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
