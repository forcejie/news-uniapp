<template>
	<view class="content">
		<scroll-view scroll-x class="nav">
			<view v-for="(item, index) in navdata" class="item"
				:class="index === navIndex ? 'active': ''"
				:key="item.id"
				@click="clickNav(index,item.id)"
			>{{item.classname}}</view>
		</scroll-view>
		<view class="content">
			<view v-for="item in newsdata" class="row" :key="item.id">
				<newbox @click.native="goDetail(item)" :item="item"></newbox>
			</view>
		</view>
		<view class="nodata" v-if="!newsdata.length">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>
		<view class="loading" v-if="newsdata.length">
			<view v-if="loading===1">数据加载中...</view>
			<view v-if="loading===2">没有更多啦...</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navdata: [],
				newsdata: [],
				currentId:50,
				currentPage:1,
				loading:0
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsList()
		},
		onReachBottom() {
			if (this.loading === 2) {
				return
			}
			this.currentPage++
			this.loading = 1
			this.getNewsList()
		},
		methods: {
			clickNav(index, id) {
				this.navIndex=index;
				this.currentPage=1;
				this.currentId=id;
				this.newsdata=[];
				this.loading=0;
				this.getNewsList(id)
			},
			// 跳转详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			
			// 导航栏数据
			getNavData() {
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						console.log(res)
						this.navdata = res.data
						console.log(this.navdata)
					}
				})
			},
			getNewsList() {
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: this.currentId,
						page: this.currentPage
					},
					success: res => {
						console.log(res)
						if(res.data.length === 0) {
							this.loading = 2
						}
						this.newsdata = [...this.newsdata, ...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	scroll-view {
		// width: 100%;
		height: 100rpx;
		position: fixed;
		white-space: nowrap;
		z-index: 10;
		background: #F7F8FA;
		left: 0;
		top: var(--window-top);
		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: inline-block;
		}
		.item {
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;
			&.active {
				color: deeppink;
			}
		}
	}
	.content {
		padding: 30rpx;
		padding-top: 130rpx;
		.row {
			border-bottom: 1px dotted #DDDDDD;
			padding: 20rpx 0;
		}
	}
	.nodata {
		display: flex;
		justify-content: center;
		image{
			width: 360rpx;
		}
	}
	.loading {
		text-align: center;
		font-size: 26rpx;
		color:#888;
		line-height: 2em;
	}
</style>
