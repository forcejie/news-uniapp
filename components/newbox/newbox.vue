<template>
	<view class="newbox">
		<view class="pic">
			<image :src="item.picurl" mode="aspectFill"></image>
		</view>
		<view class="text">
			<view class="title">
				{{item.title}}
			</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}</text>
			</view>
			<view class="info" v-else>
				<text>浏览时间: {{item.looktime}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"newbox",
		props: {
			item: {
				type:Object,
				default() {
					return {
						title:"组件内默认的标题",
						author: "KK",
						hits:668,
						picurl: "../../static/images/nopic.jpg"
					}
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.newbox {
	display: flex;
	.pic {
		width: 230rpx;
		height: 160rpx;
		image {
			width: 100%;
			height: 100%;
		}
	}
	.text {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding-left: 20rpx;
		justify-content: space-between;
		.title {
			font-size: 36rpx;
			color:#333;
			text-overflow: -o-ellipsis-lastline;
			overflow: hidden;				//溢出内容隐藏
			text-overflow: ellipsis;		//文本溢出部分用省略号表示
			display: -webkit-box;			//特别显示模式
			-webkit-line-clamp: 2;			//行数
			line-clamp: 2;					
			-webkit-box-orient: vertical;	//盒子中内容竖直排列	
		}
		.info {
			font-size: 26rpx;
			color: #999;
			text {
				padding-left: 30rpx;
			}
		}
	}
}
</style>