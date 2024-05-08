<template>
    <view class="bsyy-dialog" :animation="animation" :style="{ display, zIndex }" @click.stop="modalClick()">
        <view class="inner" @click.stop><slot></slot></view>
    </view>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";

type Props = { modelValue?: boolean; zIndex?: number; closeOnClickModal?: boolean };
let animationData: UniApp.Animation | null = null;
const props = withDefaults(defineProps<Props>(), { modelValue: true, zIndex: 9, closeOnClickModal: true });
const emits = defineEmits(["update:modelValue", "close"]);
const display = ref("none");
const animation = ref(null);
watch(
    () => props.modelValue,
    (value) => (value ? showAnimation() : hideAnimation())
);
init();
function init() {
    animationData = uni.createAnimation({ duration: 200, timingFunction: "ease-in-out", delay: 0 });
    props.modelValue ? showAnimation() : hideAnimation();
}
const modalClick = () => {
    if (props.closeOnClickModal) {
        emits("update:modelValue", false);
        emits("close");
    }
};
function showAnimation() {
    // #ifdef H5
    document.body.style.overflow = "hidden";
    // #endif
    display.value = "flex";
    setTimeout(() => {
        animationData.opacity(1).step();
        animation.value = animationData.export();
    }, 17);
}
function hideAnimation() {
    // #ifdef H5
    document.body.style.overflow = "visible";
    // #endif
    animationData.opacity(0).step();
    animation.value = animationData.export();
    setTimeout(() => (display.value = "none"), 200);
}
</script>

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
