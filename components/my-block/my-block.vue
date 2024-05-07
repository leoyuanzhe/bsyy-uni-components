<template>
	<view class="my-block" :style="{ paddingTop: paddingTop + 'px', paddingBottom: paddingBottom + 'px', height, backgroundColor }"><slot></slot></view>
</template>

<script>
export default {
	props: {
		height: { type: String, default: "0rpx" },
		backgroundColor: { type: String, default: "transparent" },
		topFit: { type: Boolean, default: false },
		bottomFit: { type: Boolean, default: false },
	},
	data() {
		return {
			paddingTop: 0,
			paddingBottom: 0,
		};
	},
	created() {
		this.init();
	},
	methods: {
		init() {
			if (this.topFit || this.bottomFit) {
				uni.getSystemInfo({
					success: ({ safeAreaInsets }) => {
						this.topFit && (this.paddingTop = safeAreaInsets.top);
						this.bottomFit && (this.paddingBottom = safeAreaInsets.bottom);
					},
				});
			}
		},
	},
};
</script>

<style scoped>
.my-block {
	flex-shrink: 0;
	box-sizing: content-box;
}
</style>
