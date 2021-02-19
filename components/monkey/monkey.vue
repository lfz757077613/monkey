<template>
	<view class="item">
		<uni-easyinput v-model="inputKey"></uni-easyinput>
		<uni-easyinput v-model="inputValue" type="password"></uni-easyinput>
		<button @click="clickSave" type="primary">保存</button>
		<button @click="clickDelete" type="warn">删除</button>
	</view>
</template>

<script>
	export default {
		props: {
			k: String,
			v: String,
		},
		data() {
			return {
				inputKey: this.k,
				inputValue: this.v
			}
		},
		methods: {
			clickSave() {
				uni.removeStorageSync("monkey_" + this.k);
				uni.setStorageSync("monkey_" + this.inputKey, this.inputValue);
				this.$emit("refresh")
			},
			clickDelete() {
				uni.removeStorageSync("monkey_" + this.inputKey);
				this.$emit("refresh")
			}
		}
	}
</script>

<style lang="scss">
	.item {
		display: flex;
		align-items: center;
	}
</style>
