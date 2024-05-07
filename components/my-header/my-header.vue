<template>
	<view class="my-header" :style="{ top, zIndex, paddingTop: paddingTop + 'px', backgroundColor, boxShadow }">
		<slot>
			<view v-if="showBack" class="icon left" :style="{ paddingTop: paddingTop + 'px' }" @click="goBack()">
				<text class="iconfont icon-fanhui1" :style="{ color }"></text>
			</view>
			<view v-if="leftIcon" class="icon left" :style="{ paddingTop: paddingTop + 'px' }" @tap="$emit('left-tap')">
				<text :class="{ iconfont: true, [leftIcon]: true }" :style="{ color }"></text>
			</view>
			<text class="title" :style="{ color }">{{ title }}</text>
			<view v-if="rightIcon" class="icon right" :style="{ paddingTop: paddingTop + 'px' }" @tap="$emit('right-tap')">
				<text :class="{ iconfont: true, [rightIcon]: true }" :style="{ color }"></text>
			</view>
		</slot>
	</view>
</template>

<script>
export default {
	props: {
		zIndex: { type: Number, default: 999 },
		top: { type: String, default: "0px" },
		height: { type: String, default: "44px" },
		color: { type: String, default: "var(--fore1)" },
		backgroundColor: { type: String, default: "var(--rgba2)" },
		boxShadow: { type: String, default: "0 0 6px 3px var(--back4)" },
		title: { type: String, default: "" },
		showBack: { type: Boolean, default: false },
	},
	data() {
		return {
			paddingTop: 0,
		};
	},
	created() {
		this.init();
	},
	methods: {
		goBack() {
			uni.navigateBack().catch(() => uni.switchTab({ url: "/pages/index/index" }));
		},
		init() {
			uni.getSystemInfo({
				success: ({ safeArea }) => {
					this.paddingTop = safeArea.top;
				},
			});
		},
	},
};
</script>

<style lang="scss" scoped>
.my-header {
	box-sizing: content-box;
	position: fixed;
	left: 0;
	width: 100%;
	height: 44px;
	display: flex;
	justify-content: center;
	align-items: center;
	backdrop-filter: blur(4rpx);
	-webkit-backdrop-filter: blur(4rpx);
	.icon {
		position: absolute;
		top: 0;
		width: 44px;
		height: 44px;
		display: flex;
		justify-content: center;
		align-items: center;
		&.left {
			left: 0;
		}
		&.right {
			right: 0;
		}
		.iconfont {
			font-size: 16px;
		}
	}
	.title {
		font-size: 18px;
		font-weight: bold;
	}
	.top-level {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}
}
</style>
