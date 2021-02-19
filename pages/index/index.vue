<template>
	<view class="container">
		<monkey v-for="(item, index) in content" :key="item.k" :k="item.k" :v="item.v" @refresh="refresh"></monkey>
		<button @click="add" type="primary">添加</button>
	</view>
</template>

<script>
	import monkey from '@/components/monkey/monkey.vue'
	export default {
		comments: {
			monkey
		},
		data() {
			return {
				content: [],
			}
		},
		mounted() {
			this.refresh();
		},
		methods: {
			refresh() {
				var jsonContent = [];
				const res = uni.getStorageInfoSync();
				for (var k of res.keys.reverse()) {
					if (isMonkey(k)) {
						jsonContent.push(new Monkey(k.replace("monkey_", ""), uni.getStorageSync(k)))
					}
				}
				this.content = jsonContent
			},
			add() {
				uni.setStorageSync("monkey_", "");
				this.refresh();
			}
		},
		onLoad() {
			uni.startSoterAuthentication({
				requestAuthModes: ['fingerPrint'],
				authContent: '请用指纹解锁',
				success(res) {
					uni.showToast({
						title: "识别成功",
						duration: 2000,
						icon: 'none'
					})
					//指纹识别成功后，进行后续工作
				},
				fail(err) {
					plus.runtime.quit();
				}
			})
		}
	}
	class Monkey {
		constructor(k, v) {
			this.k = k;
			this.v = v;
		}
	}

	function isMonkey(str) {
		return str.substr(0, 7) === "monkey_";
	}
</script>

<style>

</style>
