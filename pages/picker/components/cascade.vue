<script lang="ts" setup>
import { reactive, ref } from "vue";

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
const modelValue = ref([0, 0, 0]);
const columns: { label: string; value: string }[][] = reactive([[], [], []]);
columns[0] = citys.map((v) => ({ label: v.label, value: v.code }));
const change = (e: { selectedValues: string[]; selectedOptions: { label: string; value: string }[]; selectedIndexes: number[]; columnIndex: number }) => {
    if (e.columnIndex == 0) {
        modelValue.value[1] = 0;
        modelValue.value[2] = 0;
    } else if (e.columnIndex == 1) {
        modelValue.value[2] = 0;
    }
    countColumns();
};
countColumns();
function countColumns() {
    columns[1] = citys[modelValue.value[0]].children.map((v) => ({ label: v.label, value: v.code }));
    columns[2] = citys[modelValue.value[0]].children[modelValue.value[1]].children.map((v) => ({ label: v.label, value: v.code }));
}
</script>

<template>
    <view class="section">
        <text>级联用法：{{ JSON.stringify(modelValue) }}</text>
        <bsyy-picker v-model="modelValue" :columns="columns" @change="change($event)" />
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
