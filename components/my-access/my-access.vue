<template>
	<slot v-if="check"></slot>
</template>

<script>
export default {
	props: {
		roles: { type: Array, default: [] }, // 可查看的身份
		type: { type: String, default: 'include' } // 包含或是排除
	},
	computed: {
		check() {
			return (this.type == 'include' && this.roles.includes(uni.getStorageSync('role'))) || (this.type == 'exclude' && !this.roles.includes(uni.getStorageSync('role')));
		}
	}
};
</script>
