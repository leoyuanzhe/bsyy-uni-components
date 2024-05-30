<script lang="ts" setup>
import { reactive, ref } from "vue";

const modelValue = ref([0, 1]);
const columns = reactive([
    [
        { label: "汽车", value: 1 },
        { label: "高铁", value: 2 },
        { label: "飞机", value: 3 },
    ],
    [
        { label: "矿泉水", value: 1 },
        { label: "汽水", value: 2 },
        { label: "果汁", value: 3 },
    ],
]);
</script>

<template>
    <view class="section">
        <text>多选用法：{{ JSON.stringify(modelValue) }}</text>
        <bsyy-picker v-model="modelValue" :columns="columns" />
    </view>
</template>

<style lang="scss" scoped>
.section {
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 0 0 3rpx 1rpx #ccc;
    border-radius: 20rpx;
    overflow: hidden;
    text {
        width: 100%;
        height: 60rpx;
        font-size: 24rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        border-bottom: 1rpx solid #eee;
        background-color: #fff;
    }
}
</style>
