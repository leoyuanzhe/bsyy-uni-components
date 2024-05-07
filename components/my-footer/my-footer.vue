<template>
	<view class="my-footer" :style="{ zIndex, paddingBottom: paddingBottom + 'px', height, borderTop, backgroundColor, boxShadow }"><slot></slot></view>
</template>

<script>
export default {
	props: {
		zIndex: { type: Number, default: 9 }, // 层级
		height: { type: String, default: '0px' }, // 高度（不包含其他距离）
		borderTop: { type: String, default: 'none' }, // 上边框
		backgroundColor: { type: String, default: '#fff' }, // 背景色
		boxShadow: { type: String, default: 'none' } // 阴影
	},
	data() {
		return {
			paddingBottom: 0
		};
	},
	created() {
		this.init();
	},
	methods: {
		init() {
			uni.getSystemInfo({
				success: ({ safeAreaInsets }) => {
					this.paddingBottom = safeAreaInsets.bottom;
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.my-footer {
	position: fixed;
	left: 0;
	bottom: 0;
	width: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>
