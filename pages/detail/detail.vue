<template>
	<view class="detail">
		<view class="title">{{detail.title}}</view>
		<view class="info">
			<view class="author">编辑:{{detail.author}}</view>
			<view class="time">发布日期：{{detail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		<view class="des">
			快乐的学习uniapp啦啦啦啦
		</view>
	</view>
</template>

<script>
	import { parseTime } from '../../utils/formatTime.js'
	export default {
		data() {
			return {
				detail:{},
				options: null,
			};
		},
		onLoad(e) {
			this.options=e,
			this.getDetail()
		},
		methods:{
			getDetail() {
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data: this.options,
					success:res => {
						console.log(res)
						// 格式转换一下
						res.data.posttime=parseTime(res.data.posttime)
						res.data.content=res.data.content.replace(/<img/gi, '<img style="max-width:100%"')
						this.detail=res.data
						
						this.saveHistory()
						
						uni.setNavigationBarTitle({
							title:this.detail.title
						})
					}
				})
			},
			saveHistory() {
				let historyArr=uni.getStorageSync("historyArr") || []
				let item={
					id:this.detail.id,
					classid:this.detail.classid,
					picurl:this.detail.picurl,
					title:this.detail.title,
					looktime:parseTime((Date.now()))
				}
				let index=historyArr.findIndex(i => {
					return i.id = this.detail.id
				})
				if(index >= 0) {
					historyArr.splice(index, 1)
				} 
				historyArr.unshift(item)
				historyArr=historyArr.slice(0, 10)
				uni.setStorageSync("historyArr", historyArr)
			}
		}
	}
</script>

<style lang="scss">
.detail {
	padding: 30rpx;
	.title {
		font-size: 46rpx;
		color: #333;
	}
	.info {
		display: flex;
		background: #F6F6F6;
		justify-content: space-between;
		padding: 20rpx;
		font-size: 25rpx;
		margin: 40rpx 0;
	}
	.content {
		padding-bottom: 50rpx;
		font-size: 30rpx;
	}
	.des {
		background-color: #fef0f0;
		font-size: 26rpx;
		padding: 20rpx;
		color: #F89898;
		line-height: 1.8em;
	}
}

</style>
