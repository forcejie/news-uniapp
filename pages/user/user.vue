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
				
			};
		},
		methods: {
			goDetail() {
				uni.navigateTo({
					url: "/pages/detail/detail"
				})
			}
		}
	}
</script>

<style lang="scss">

</style>
