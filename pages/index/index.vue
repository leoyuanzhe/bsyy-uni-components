<script lang="ts" setup>
import { ref } from "vue";
const show = ref(true);
</script>

<template>
    <view class="index">
        <text>bsyy-components-ts</text>
        <bsyy-dialog v-model="show">
            <view class="dialog">Hello Dialog!</view>
        </bsyy-dialog>
    </view>
</template>

<style>
.dialog {
    width: 200rpx;
    height: 200rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #fff;
    border-radius: 10rpx;
    box-shadow: 0 0 3rpx 1rpx royalblue;
}
</style>
