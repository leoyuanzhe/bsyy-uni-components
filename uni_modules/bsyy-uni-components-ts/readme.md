# bsyy-uni-component-ts

## 指南

### 快速开始

#### 下载插件并导入 HBuilderX

#### 引入样式

-   App.vue：

```scss
@import "@/uni_modules/bsyy-uni-components-ts/styles/style.scss";
```

#### 注册组件

##### easycom 方式

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

-   Dialog 弹出框
-   Picker 选择器
-   Popup 弹出层
-   ...正在开发中

<iframe width="414" height="736" src="https://bsyyyz.github.io/YuanZhe/bsyy-uni-components-ts/#/" frameborder="0" allowfullscreen></iframe>

### Dialog 弹出框

#### 基本用法

```html
<script lang="ts" setup>
    const show = ref(true);
</script>

<template>
    <button @click="show = true">打开 Dialog</button>
    <bsyy-dialog v-model:show="show">
        <view class="dialog">Hello Dialog!</view>
    </bsyy-dialog>
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

| 属性名                 | 说明                                          | 类型    | 默认 |
| ---------------------- | --------------------------------------------- | ------- | ---- |
| v-model:show           | 是否显示 Dialog                               | boolean | true |
| close-on-click-overlay | 是否在点击遮罩层后关闭 Dialog                 | boolean | true |
| z-index                | 和原生的 CSS 的 z-index 相同，改变 z 轴的顺序 | number  | 2000 |

##### Slots

| 插槽名 | 说明          |
| ------ | ------------- |
| -      | Dialog 的内容 |

##### Events

| 事件名 | 说明              | Type       |
| ------ | ----------------- | ---------- |
| close  | Dialog 关闭的回调 | () => void |

### Picker 选择器

#### 基本用法

```html
<script lang="ts" setup>
    const modelValue = ref([0]);
    const columns = reactive([
        [
            { label: "汽车", value: 1 },
            { label: "高铁", value: 2 },
            { label: "飞机", value: 3 },
        ],
    ]);
</script>

<template>
    <bsyy-picker v-model="modelValue" :columns="columns" />
</template>
```

#### 多选用法

```html
<script lang="ts" setup>
    const modelValue = ref([0, 1]);
    const columns = reactive([
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
</script>

<template>
    <bsyy-picker v-model="modelValue" :columns="columns" />
</template>
```

#### 级联用法

```html
<script lang="ts" setup>
    const citys = [
        {
            label: "北京",
            code: "0",
            children: [
                {
                    label: "北京市",
                    code: "00",
                    children: [
                        { label: "东城区", code: "000", children: [] },
                        { label: "西城区", code: "001", children: [] },
                        { label: "崇文区", code: "002", children: [] },
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
                        {
                            label: "上城区",
                            code: "100",
                            children: [],
                        },
                    ],
                },
            ],
        },
    ];
    const modelValue3 = ref([0, 0, 0]);
    const columns3: { label: string; value: string }[][] = reactive([[], [], []]);
    columns3[0] = citys.map((v) => ({ label: v.label, value: v.code }));
    const countColumns3 = () => {
        modelValue3.value[2] = modelValue3.value[1] = 0;
        columns3[1] = citys[modelValue3.value[0]].children.map((v) => ({ label: v.label, value: v.code }));
        columns3[2] = citys[modelValue3.value[0]].children[modelValue3.value[1]].children.map((v) => ({ label: v.label, value: v.code }));
    };
    countColumns3();
</script>

<template>
    <bsyy-picker v-model="modelValue" :columns="columns" @change="countColumns($event)" />
</template>
```

#### API

##### Attributes

| 属性名                | 说明                 | 类型                                                   | 默认 |
| --------------------- | -------------------- | ------------------------------------------------------ | ---- |
| model-value / v-model | 当前选中项对应的下标 | number[]                                               | []   |
| columns               | 弹出位置             | { label: string; value: string\| number \| boolean }[] | []   |

##### Events

| 事件名 | 说明                 | Type                      |
| ------ | -------------------- | ------------------------- |
| change | 选中的选项改变时触发 | (value: number[]) => void |

### Popup 弹出层

#### 基本用法

```html
<script lang="ts" setup>
    const show = ref(true);
</script>

<template>
    <button @click="show = true">打开 Popup</button>
    <bsyy-popup v-model:show="show">
        <view class="popup">Hello Popup!</view>
    </bsyy-popup>
</template>

<style lang="scss" scoped>
    .popup {
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

| 属性名                 | 说明                                          | 类型                                  | 默认     |
| ---------------------- | --------------------------------------------- | ------------------------------------- | -------- |
| v-model:show           | 是否显示  Popup                               | boolean                               | true     |
| position               | 弹出位置                                      | "left"\| "right" \| "top" \| "bottom" | "bottom" |
| close-on-click-overlay | 是否在点击遮罩层后关闭  Popup                 | boolean                               | true     |
| z-index                | 和原生的 CSS 的 z-index 相同，改变 z 轴的顺序 | number                                | 2000     |

##### Slots

| 插槽名 | 说明         |
| ------ | ------------ |
| -      | Popup 的内容 |

##### Events

| 事件名 | 说明             | Type       |
| ------ | ---------------- | ---------- |
| close  | Popup 关闭的回调 | () => void |

## 更多

欢迎提出在留言区提出你需要的功能或 bug。
