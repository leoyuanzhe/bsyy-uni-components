<script lang="ts" setup>
import { ref } from "vue";

const show = ref(true);
</script>

<template>
    <view class="popup">
        <button @click="show = true">打开 Popup</button>
        <bsyy-popup v-model:show="show">
            <view class="popup">Hello Popup!</view>
        </bsyy-popup>
    </view>
</template>

<style lang="scss" scoped>
.popup {
    .popup {
        height: 200rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #fff;
        border-radius: 10rpx;
        box-shadow: 0 0 3rpx 1rpx royalblue;
    }
}
</style>
