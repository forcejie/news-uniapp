<template>
	<view class="user">
		<view>
			<view class="row" v-for="item in listArr" :key="item.id">
				<newbox @click.native="goDetail(item)" :item="item"></newbox>
			</view>
		</view>
		<view v-if="!listArr.length">
			<view>没有数据啦</view>
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
		onShow() {
			this.getHistory()
		},
		methods: {
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			getHistory() {
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr=hisArr
				console.log(this.listArr)
			}
		}
	}
</script>

<style lang="scss">

</style>
