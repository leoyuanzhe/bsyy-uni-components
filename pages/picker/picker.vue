<script lang="ts" setup>
import { reactive, ref } from "vue";

const modelValue = ref([0]);
const columns = reactive([
    [
        { label: "汽车", value: 1 },
        { label: "高铁", value: 2 },
        { label: "飞机", value: 3 },
    ],
]);
const modelValue2 = ref([0, 1]);
const columns2 = reactive([
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
const citys = [
    {
        label: "北京",
        code: "0",
        children: [
            {
                label: "北京市",
                code: "00",
                children: [
                    { label: "东城区", code: "000" },
                    { label: "西城区", code: "001" },
                    { label: "崇文区", code: "002" },
                ],
            },
        ],
    },
    {
        label: "浙江省",
        code: "1",
        children: [
            {
                label: "杭州市",
                code: "10",
                children: [
                    { label: "上城区", code: "100" },
                    { label: "下城区", code: "101" },
                ],
            },
            {
                label: "宁波市",
                code: "11",
                children: [
                    { label: "海曙区", code: "110" },
                    { label: "江东区", code: "111" },
                ],
            },
        ],
    },
];
const modelValue3 = ref([0, 0, 0]);
const columns3: { label: string; value: string }[][] = reactive([[], [], []]);
columns3[0] = citys.map((v) => ({ label: v.label, value: v.code }));
const countColumns3 = () => {
    if (!citys[modelValue3.value[0]].children[modelValue3.value[1]]) modelValue3.value[1] = 0;
    if (!citys[modelValue3.value[0]].children[modelValue3.value[1]].children[modelValue3.value[2]]) modelValue3.value[2] = 0;
    columns3[1] = citys[modelValue3.value[0]].children.map((v) => ({ label: v.label, value: v.code }));
    columns3[2] = citys[modelValue3.value[0]].children[modelValue3.value[1]].children.map((v) => ({ label: v.label, value: v.code }));
};
countColumns3();
</script>

<template>
    <view class="picker">
        <view class="section">
            <text>基本用法：{{ JSON.stringify(modelValue) }}</text>
            <bsyy-picker v-model="modelValue" :columns="columns" />
        </view>
        <view class="section">
            <text>多选用法：{{ JSON.stringify(modelValue2) }}</text>
            <bsyy-picker v-model="modelValue2" :columns="columns2" />
        </view>
        <view class="section">
            <text>级联用法：{{ JSON.stringify(modelValue3) }}</text>
            <bsyy-picker v-model="modelValue3" :columns="columns3" @change="countColumns3($event)" />
        </view>
    </view>
</template>

<style lang="scss" scoped>
.picker {
    padding: 20rpx;
    display: flex;
    flex-direction: column;
    row-gap: 20rpx;
    background-color: #eee;
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
}
</style>
