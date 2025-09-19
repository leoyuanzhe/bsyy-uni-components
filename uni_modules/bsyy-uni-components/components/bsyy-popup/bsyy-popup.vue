<!-- Created by YuanZhe(https://bsyyyz.github.io/YuanZhe) -->
<script lang="ts" setup>
import { ref, watch } from "vue";

type Props = { show?: boolean; position?: "top" | "bottom" | "right" | "left"; zIndex?: number; closeOnClickOverlay?: boolean };
let animationData: UniApp.Animation | null = null;
const props = withDefaults(defineProps<Props>(), { show: true, position: "bottom", zIndex: 2000, closeOnClickOverlay: true });
const emits = defineEmits(["update:show", "close"]);
const display = ref("none");
const animation = ref(null);
watch(
    () => props.show,
    (value) => (value ? showAnimation() : hideAnimation())
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

<template>
    <view
        class="bsyy-popup"
        :animation="animation"
        :style="{
            display,
            zIndex,
            justifyContent: props.position == 'left' ? 'flex-start' : props.position == 'right' ? 'flex-end' : 'normal',
            alignItems: props.position == 'top' ? 'flex-start' : props.position == 'bottom' ? 'flex-end' : 'normal',
        }"
        @click.stop="overlayClick()"
    >
        <view
            class="inner"
            :style="{
                width: props.position == 'top' || props.position == 'bottom' ? '100%' : 'auto',
                height: props.position == 'left' || props.position == 'right' ? '100%' : 'auto',
                transform: props.show
                    ? 'none'
                    : props.position == 'left'
                    ? 'translateX(-100%)'
                    : props.position == 'right'
                    ? 'translateX(100%)'
                    : props.position == 'top'
                    ? 'translateY(-100%)'
                    : props.position == 'bottom'
                    ? 'translateY(100%)'
                    : 'none',
            }"
            @click.stop
        >
            <slot></slot>
        </view>
    </view>
</template>

<style lang="scss" scoped>
.bsyy-popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px) brightness(100%);
    -webkit-backdrop-filter: blur(5px) brightness(100%);
    .inner {
        max-width: 100vw;
        max-height: 100vh;
        transition: transform 0.2s;
    }
}
</style>
