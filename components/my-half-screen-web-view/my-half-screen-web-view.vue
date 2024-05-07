<script lang="ts" setup>
import { onMounted, ref } from "vue";

interface Props {
	height: string;
	src: string;
}
const props = withDefaults(defineProps<Props>(), {
	height: "200px",
});
const paddingTop = ref(0);
onMounted(() => {
	init();
});
function init() {
	safeAreaInit();
	function safeAreaInit() {
		uni.getSystemInfo({
			success: ({ safeAreaInsets }) => {
				paddingTop.value = safeAreaInsets.top;
			},
		});
	}
}
</script>

<template>
	<!-- #ifdef APP-PLUS -->
	<web-view class="my-configure" :fullscreen="false" :src="props.src" :webview-styles="{ top: paddingTop + 'px', width: '100%', height: props.height }"></web-view>
	<!-- #endif -->
	<!-- #ifdef H5 -->
	<iframe class="my-configure" :src="props.src" frameborder="0" :style="{ top: paddingTop + 'px', height: props.height }"></iframe>
	<!-- #endif -->
</template>

<style lang="scss" scoped>
.my-configure {
	display: block;
	width: 100%;
}
</style>
