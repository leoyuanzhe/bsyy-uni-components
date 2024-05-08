<script lang="ts" setup>
const goDialog = () => {
    uni.navigateTo({ url: "/pages/dialog/dialog" });
};
</script>

<template>
    <view class="index">
        <button @tap="goDialog()">Dialog 对话框</button>
    </view>
</template>

<style lang="scss" scoped></style>
