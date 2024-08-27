<!-- Created by YuanZhe(https://bsyyyz.github.io/YuanZhe) -->
<script lang="ts" setup>
import { ref, watch } from "vue";

type Props = { show?: boolean; zIndex?: number; closeOnClickOverlay?: boolean };
let animationData: UniApp.Animation | null = null;
const props = withDefaults(defineProps<Props>(), { show: true, zIndex: 2000, closeOnClickOverlay: true });
const emits = defineEmits(["update:show", "close"]);
const display = ref("none");
const animation = ref(null);
watch(
    () => props.show,
    (value) => {
        value ? showAnimation() : hideAnimation();
    }
);
init();
function init() {
    animationData = uni.createAnimation({ duration: 200, timingFunction: "ease-in-out", delay: 0 });
    props.show ? showAnimation() : hideAnimation();
}
const overlayClick = () => {
    if (props.closeOnClickOverlay) {
        emits("update:show", false);
        emits("close");
    }
};
function showAnimation() {
    // #ifdef H5
    document.body.style.overflow = "hidden";
    // #endif
    display.value = "flex";
    setTimeout(() => {
        if (props.show) {
            animationData.opacity(1).step();
            animation.value = animationData.export();
        }
    }, 17);
}
function hideAnimation() {
    // #ifdef H5
    document.body.style.overflow = "visible";
    // #endif
    animationData.opacity(0).step();
    animation.value = animationData.export();
    setTimeout(() => {
        if (!props.show) display.value = "none";
    }, 200);
}
</script>

<template>
    <view class="bsyy-dialog" :animation="animation" :style="{ display, zIndex }" @click.stop="overlayClick()">
        <view class="inner" @click.stop><slot></slot></view>
    </view>
</template>

<style lang="scss" scoped>
.bsyy-dialog {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px) brightness(100%);
    -webkit-backdrop-filter: blur(5px) brightness(100%);
    .inner {
        max-width: 100vw;
        max-height: 100vh;
    }
}
</style>
