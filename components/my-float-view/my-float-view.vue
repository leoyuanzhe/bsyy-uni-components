<script lang="ts" setup>
import { reactive, watch } from "vue";

interface Props {
    type: "webView" | "video" | "chart";
    zIndex: number;
    src: string;
    opts?: any;
    chartData?: { categories: string[]; series: { name: string; data: number[]; format: string }[] };
}
const point = { startX: 0, startY: 0 };
const safeArea = { top: 0, bottom: 0 };
const screen = { windowWidth: 0, windowHeight: 0 };
const props = withDefaults(defineProps<Props>(), {
    zIndex: 1,
    src: "",
    opts: () => ({
        update: true,
        duration: 20,
        animation: false,
        dataLabel: false,
        padding: [15, 20, 0, 5],
        legend: {},
        dataPointShape: false,
        xAxis: { disableGrid: true, labelCount: 5 },
        yAxis: { gridType: "dash", dashLength: 2 },
        extra: { line: { type: "step", width: 2, activeType: "hollow" } },
    }),
    chartData: () => ({ categories: [], series: [] }),
});
const emits = defineEmits(["close", "fullScreen", "switch"]);
const coverView = reactive({ top: 0, left: 0, width: 320, height: 180, borderWidth: 2, titleHeight: 30, transition: "none", isFullScreen: false });
watch(
    () => coverView.isFullScreen,
    (value) => {
        coverView.transition = "all 0.2s";
        if (value) {
            coverView.top = safeArea.top;
            coverView.left = coverView.borderWidth;
            coverView.width = screen.windowWidth - coverView.borderWidth * 2;
            coverView.height = screen.windowHeight - (safeArea.top + safeArea.bottom);
        } else positionReset();
        setTimeout(() => (coverView.transition = "none"), 1000);
    }
);
init();
function init() {
    uni.getSystemInfo({
        success: ({ windowWidth, windowHeight, safeAreaInsets }) => {
            screen.windowWidth = windowWidth;
            screen.windowHeight = windowHeight;
            safeArea.top = safeAreaInsets.top + coverView.titleHeight + coverView.borderWidth;
            safeArea.bottom = safeAreaInsets.bottom + coverView.borderWidth;
        },
    });
    positionReset();
}
function positionReset() {
    coverView.width = 320;
    coverView.height = 180;
    coverView.left = screen.windowWidth / 2 - coverView.width / 2;
    coverView.top = screen.windowHeight / 2 - coverView.height / 2;
}
const touchstart = ({ touches }) => {
    if (!coverView.isFullScreen) {
        point.startX = touches[0].clientX;
        point.startY = touches[0].clientY;
    }
};
const touchmove = ({ touches }) => {
    if (!coverView.isFullScreen) {
        coverView.left -= point.startX - touches[0].clientX;
        coverView.top -= point.startY - touches[0].clientY;
        point.startX = touches[0].clientX;
        point.startY = touches[0].clientY;
        if (coverView.top <= safeArea.top + 44) coverView.top = safeArea.top + 44;
        if (coverView.top >= screen.windowHeight - (coverView.height + coverView.borderWidth + safeArea.bottom)) coverView.top = screen.windowHeight - (coverView.height + coverView.borderWidth + safeArea.bottom);
    }
};
</script>

<template>
    <!-- #ifdef APP-PLUS -->
    <web-view
        v-if="type == 'webView'"
        class="web-view"
        :fullscreen="false"
        :src="props.src"
        :webview-styles="{ top: coverView.top + 'px', left: coverView.left + 'px', width: coverView.width + 'px', height: coverView.height + 'px', background: 'transparent', scalable: true }"
    ></web-view>
    <!-- #endif -->
    <view
        class="my-float-view"
        :style="{
            top: coverView.top - (coverView.titleHeight + coverView.borderWidth) + 'px',
            left: coverView.left - coverView.borderWidth + 'px',
            width: coverView.width + coverView.borderWidth * 2 + 'px',
            height: coverView.height + (coverView.titleHeight + coverView.borderWidth * 2) + 'px',
            transition: coverView.transition,
            zIndex: props.zIndex,
        }"
    >
        <view class="title" :style="{ height: coverView.titleHeight + 'px' }" @touchstart="touchstart($event)" @touchmove.prevent="touchmove($event)">
            <view class="left">
                <!-- <text class="iconfont icon-video-camera" @touchstart="emits('switch', { icon: 'icon-video-camera' })"></text>
                <text class="iconfont icon-tupian_huaban" @touchstart="emits('switch', { icon: 'icon-tupian_huaban' })"></text> -->
            </view>
            <view class="right">
                <text v-show="!coverView.isFullScreen" class="iconfont icon-quanpingzuidahua" @touchstart="(coverView.isFullScreen = true), emits('fullScreen', true)"></text>
                <text v-show="coverView.isFullScreen" class="iconfont icon-zuidahua" @touchstart="(coverView.isFullScreen = false), emits('fullScreen', false)"></text>
                <text class="iconfont icon-guanbi" @touchstart="emits('close')"></text>
            </view>
        </view>
        <video v-if="type == 'video'" class="iframe" :src="props.src" :style="{ top: coverView.top + 'px', left: coverView.left + 'px', width: coverView.width + 'px', height: coverView.height + 'px' }"></video>
        <!-- #ifdef H5 -->
        <iframe v-if="type == 'webView'" class="iframe" :src="props.src" frameborder="0" :style="{ top: coverView.top + 'px', left: coverView.left + 'px', width: coverView.width + 'px', height: coverView.height + 'px' }"></iframe>
        <!-- #endif -->
        <qiun-data-charts
            v-if="type == 'chart'"
            class="chart"
            type="line"
            :opts="props.opts"
            :chart-data="props.chartData"
            :style="{ top: coverView.top + 'px', left: coverView.left + 'px', width: coverView.width + 'px', height: coverView.height + 'px' }"
        />
    </view>
</template>

<style lang="scss" scoped>
.web-view {
    position: fixed;
    z-index: -1;
}
.my-float-view {
    position: fixed;
    background-color: var(--primary1-2);
    box-shadow: 0 0 3px 3px var(--back4);
    overflow: hidden;
    z-index: 0;
    &::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        backdrop-filter: blur(4px);
    }
    .title {
        box-sizing: border-box;
        position: absolute;
        top: 0;
        left: 0;
        padding: 0 10px;
        width: 100%;
        display: flex;
        justify-content: space-between;
        z-index: 2;
        .left,
        .right {
            flex-shrink: 0;
            display: flex;
            align-items: center;
        }
        .left {
            .iconfont {
                margin-right: 6px;
            }
        }
        .right {
            .iconfont {
                margin-left: 6px;
            }
        }
        .iconfont {
            color: #fff;
            font-size: 18px;
            flex-shrink: 0;
        }
    }
    .iframe {
        position: fixed;
    }
    .chart {
        position: fixed;
        background-color: var(--back1);
    }
}
</style>
