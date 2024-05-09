<script lang="ts" setup>
const goDialog = () => {
    uni.navigateTo({ url: "/pages/dialog/dialog" });
};
const goPicker = () => {
    uni.navigateTo({ url: "/pages/picker/picker" });
};
const goPopup = () => {
    uni.navigateTo({ url: "/pages/popup/popup" });
};
</script>

<template>
    <view class="index">
        <button @tap="goDialog()">Dialog 弹出框</button>
        <button @tap="goPicker()">Picker 选择器</button>
        <button @tap="goPopup()">Popup 弹出层</button>
    </view>
</template>

<style lang="scss" scoped></style>
