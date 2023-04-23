<template>
	<view class="user">
		<view>
			<newbox @click.native="goDetail()"></newbox>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr: []
			};
		},
		// onLoad() {
		// 	this.getHistory()
		// }
		methods: {
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			// getHistory() {
			// 	let hisArr = uni.getStorageSync(("his"))
			// }
		}
	}
</script>

<style lang="scss">

</style>
