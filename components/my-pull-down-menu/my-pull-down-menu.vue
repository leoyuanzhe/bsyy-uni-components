<template>
	<view class="my-pull-down-menu">
		<slot></slot>
		<my-animation :show="show"><view class="modal" :style="{ top: top + 'px', height: height + 'px', backgroundColor }" @touchstart.stop="blankClose && $emit('update:show', false)"></view></my-animation>
		<my-animation :show="show">
			<view class="inner"><slot name="inner"></slot></view>
		</my-animation>
	</view>
</template>

<script lang="ts" setup>
import { defineProps, ref, watch } from 'vue';
const props = defineProps({
	show: { type: Boolean, default: true },
	zIndex: { type: Number, default: 8 },
	backgroundColor: { type: String, default: 'rgba(0, 0, 0, 0.6)' },
	blankClose: { type: Boolean, default: true }
});
let top = ref<number>(0);
let height = ref<number>(0);
watch(
	() => props.show,
	newV => {
		newV && count();
	}
);
// 计算模糊框的高度
function count() {
	const that = this;
	const query = uni.createSelectorQuery().in(this);
	query
		.select('.my-pull-down-menu')
		.boundingClientRect(data => {
			const query2 = uni.createSelectorQuery().in(this);
			query2
				.select('.my-pull-down-menu .inner')
				.boundingClientRect(data2 => {
					uni.getSystemInfo({
						success(res) {
							that.top = data.top + data.height + data2.height;
							that.height = res.screenHeight - that.top;
						}
					});
				})
				.exec();
		})
		.exec();
}
</script>

<style lang="scss" scoped>
.my-pull-down-menu {
	position: relative;
	z-index: 9;
	.inner {
		position: absolute;
		width: 100%;
		background-color: #fff;
	}
}
.modal {
	position: fixed;
	left: 0;
	width: 100vh;
	z-index: 8;
}
</style>
