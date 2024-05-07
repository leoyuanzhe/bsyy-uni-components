# bsyy-uni-component-ts

## 指南

### 快速开始

#### easycom 方式

-   配置 package.json：

```json
{
    "easycom": {
        "custom": {
            "^bsyy-([^-].*)": "bsyy-uni-components-ts/components/bsyy-$1/bsyy-$1.vue"
        }
    }
}
```

-   使用 xxx.vue：

    ```html
    <bsyy-dialog>Hello Dialog!</bsyy-dialog>
    ```

## 组件

### Overview 总览

以下是 bsyy-uni-components-ts 提供的所有组件。

-   Dialog 对话框

### Dialog 对话框

#### 基本用法

```html
<script lang="ts" setup>
    import { ref } from "vue";
    const show = ref(true);
</script>

<template>
    <view class="index">
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
```

#### API

##### Attributes

| 属性名                | 说明                                          | 类型    | 默认 |
| --------------------- | --------------------------------------------- | ------- | ---- |
| model-value / v-model | 是否显示 Dialog                               | boolean | true |
| close-on-click-modal  | 是否可以通过点击 modal 关闭 Dialog            | boolean | true |
| z-index               | 和原生的 CSS 的 z-index 相同，改变 z 轴的顺序 | number  | 9    |

##### Slots

| 插槽名 | 说明          |
| ------ | ------------- |
| -      | Dialog 的内容 |

##### Events

| 事件名 | 说明              | Type       |
| ------ | ----------------- | ---------- |
| close  | Dialog 关闭的回调 | () => void |
