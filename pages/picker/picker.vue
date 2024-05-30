<script lang="ts" setup>
import Basic from "./components/basic.vue";
import MultipleColumns from "./components/multiple-columns.vue";
import Cascade from "./components/cascade.vue";
</script>

<template>
    <view class="picker">
        <basic />
        <multiple-columns />
        <cascade />
    </view>
</template>

<style lang="scss" scoped>
.picker {
    padding: 20rpx;
    display: flex;
    flex-direction: column;
    row-gap: 20rpx;
    background-color: #eee;
}
</style>
