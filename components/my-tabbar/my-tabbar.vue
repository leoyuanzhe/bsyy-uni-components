<template>
	<view class="tab-bar" :style="{ paddingBottom: paddingBottom + 'px' }">
		<view v-for="(v, i) in list" :key="i" :class="{ li: true, active: props.currentTabbar == i }" @click="switchTab(v)">
			<text :class="{ iconfont: true, ['icon-' + v.icon]: true }"></text>
			<text class="text">{{ v.text }}</text>
		</view>
	</view>
</template>

<script lang="ts" setup>
import { onLoad } from "@dcloudio/uni-app";
import { ref, reactive } from "vue";

const props = defineProps({
	currentTabbar: { type: Number, default: 0 },
});
let paddingBottom = ref<number>(0);
const list = reactive([
	{
		pagePath: "/pages/index/index",
		icon: "iconfontzhizuobiaozhun023101",
		text: "首页",
	},
	{
		pagePath: "/pages/sence/sence",
		icon: "dingwei",
		text: "场景",
	},
	{
		pagePath: "/pages/my/my",
		icon: "wode1",
		text: "我的",
	},
]);
onLoad(() => {
	uni.hideTabBar({ animation: false });
	init();
});
// 切换选项卡
const switchTab = (item: any) => {
	uni.switchTab({ url: item.pagePath });
};
function init() {
	uni.getSystemInfo({
		success: ({ safeAreaInsets }) => {
			paddingBottom.value = safeAreaInsets.bottom;
		},
	});
}
</script>

<style lang="scss" scoped>
.tab-bar {
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	height: 100rpx;
	box-shadow: 0 0 6px 3px var(--back4);
	z-index: 999;
	background-color: var(--rgba2);
	backdrop-filter: saturate(50%) blur(4rpx);
	-webkit-backdrop-filter: saturate(50%) blur(4rpx);
	display: flex;
	justify-content: space-around;
	align-items: center;
	.li {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		transition: all 0.2s;
		.iconfont {
			color: var(--fore1);
			font-size: 32rpx;
		}
		.text {
			color: var(--fore1);
			margin-top: 10rpx;
			font-size: 24rpx;
		}
		&.active {
			.iconfont,
			.text {
				color: var(--primary1);
			}
		}
	}
}
</style>
