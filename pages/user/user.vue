<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/1.png" mode=""></image>
		</view>
		<view class="content">
			<view class="row" v-for="item in listArr" >
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="content">
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[]
			};
		},
		onShow(){
			this.getData()
		},
		methods: {
			//跳转到详情页	
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			
			//获取缓存浏览记录
			getData(){
				let hisArr=uni.getStorageSync("historyArr") || []
				this.listArr=hisArr
				console.log(this.listArr);
			}
		}
	}
</script>

<style lang="scss">
	.user {
		.top {
			padding: 50rpx 0;
			background: #F8F8F8;
			color: #666;
			display: flex;
			justify-content: center;
			align-items: center;

			image {
				width: 150rpx;
				height: 150rpx;
			}

			.text {
				font-size: 38rpx;
				padding-top: 20rpx;
			}
		}
	}
</style>
