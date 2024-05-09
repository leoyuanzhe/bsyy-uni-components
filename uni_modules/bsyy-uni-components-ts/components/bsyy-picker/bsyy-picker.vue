<!-- Created by YuanZhe(https://bsyyyz.github.io/YuanZhe) -->
<script lang="ts" setup>
type Props = { modelValue?: number[]; columns?: { label: string; value: string | number | boolean }[][] };
const props = withDefaults(defineProps<Props>(), { modelValue: () => [], columns: () => [] });
const emits = defineEmits(["update:model-value", "change"]);
</script>

<template>
    <view class="bsyy-picker">
        <picker-view :value="props.modelValue" @change="emits('update:model-value', $event.detail.value), emits('change', $event.detail.value)">
            <picker-view-column v-for="(v, i) in props.columns" :key="i">
                <text v-for="v2 in v" :key="v2.value">{{ v2.label }}</text>
            </picker-view-column>
        </picker-view>
    </view>
</template>

<style lang="scss" scoped>
.bsyy-picker {
    width: 100%;
    height: 200px;
    background-color: var(--bsyy-picker-background-color);
    .title {
        height: 80rpx;
        background-color: var(--bsyy-picker-title-background-color);
    }
    picker-view {
        height: 100%;
        .uni-picker-view-mask {
            background-image: var(--bsyy-picker-mask) !important;
        }
        .uni-picker-view-content {
            display: flex;
            flex-direction: column;
            text {
                display: flex;
                justify-content: center;
                align-items: center;
            }
        }
    }
}
</style>
