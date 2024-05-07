<template>
	<label class="my-upload" @click="uploadClick()"><slot></slot></label>
</template>

<script>
export default {
	props: {
		count: { type: Number, default: 1 }, // 最大限制数
		sizeType: { type: Array, default: () => ['original', 'compressed'] }, // 可指定原图、压缩图
		sourceType: { type: Array, default: () => ['album', 'camera'] } // 可从相册、相机选择
	},
	methods: {
		uploadClick() {
			uni.chooseImage({
				count: this.count,
				sizeType: this.sizeType,
				sourceType: this.sourceType,
				success: res => {
					this.$emit('success', res.tempFilePaths);
				},
				fail: err => {
					this.$emit('fail', err);
				},
				complete: res => {
					this.$emit('complete', res);
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped></style>
