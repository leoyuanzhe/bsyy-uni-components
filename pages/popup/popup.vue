<script lang="ts" setup>
const goDialog = () => {
    uni.navigateTo({ url: "/pages/dialog/dialog" });
};
</script>

<template>
    <view class="popup">
        <view class=""></view>
    </view>
</template>

<style lang="scss" scoped>
.popup {
    .popup {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #fff;
        border-radius: 10rpx;
        box-shadow: 0 0 3rpx 1rpx royalblue;
    }
}
</style>
