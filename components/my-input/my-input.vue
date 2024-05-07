<template>
	<view class="my-input">
		<view class="left">
			<slot name="left">
				<text :style="{ fontSize: leftFontSize, color: leftColor }">{{ left }}</text>
			</slot>
		</view>
		<view class="center">
			<slot name="center">
				<input
					:type="type"
					:value="value"
					:password="password"
					:placeholder="placeholder"
					:placeholder-style="placeholderStyle"
					:placeholder-class="placeholderClass"
					:disabled="disabled"
					:maxlength="maxlength"
					:style="{ fontSize, color }"
					@input="inputInput"
					@click="$emit('click')"
				/>
			</slot>
		</view>
		<view
			class="right"
			:style="{
				width: rightWidth,
				height: rightHeight,
				border: rightBorder,
				borderRadius: rightBorderRadius,
				backgroundColor: rightBackgroundColor
			}"
			@click="$emit('rightClick')"
		>
			<slot name="right">
				<text
					:style="{
						fontSize: rightFontSize,
						color: rightColor
					}"
				>
					{{ right }}
				</text>
			</slot>
		</view>
	</view>
</template>

<script>
export default {
	props: {
		type: { type: String, default: 'text' },
		value: { type: [String, Number] },
		password: { type: Boolean, default: false },
		placeholder: { type: String, default: '请输入' },
		placeholderStyle: { type: String },
		placeholderClass: { type: String, default: 'input-placeholder' },
		disabled: { type: Boolean, defalut: false },
		maxlength: { type: Number, default: 140 },
		fontSize: { type: String },
		color: { type: String },
		left: { type: [String, Number] },
		leftFontSize: { type: String },
		leftColor: { type: String },
		right: { type: [String, Number] },
		rightWidth: { type: String },
		rightHeight: { type: String },
		rightFontSize: { type: String },
		rightColor: { type: String },
		rightBackgroundRadius: { type: String },
		rightBorderRadius: { type: String },
		rightBorder: { type: String },
		rightBackgroundColor: { type: String }
	},
	data() {
		return {};
	},
	methods: {
		inputInput(e) {
			this.$emit('update:value', e.detail.value);
			this.$emit('input', e.detail.value);
		}
	}
};
</script>

<style lang="scss" scoped>
.my-input {
	display: flex;
	justify-content: center;
	align-items: center;
	.left {
		flex-shrink: 0;
	}
	.center {
		flex: 1;
	}
	.right {
		flex-shrink: 0;
		display: flex;
		justify-content: center;
		align-items: center;
	}
}
</style>
