<template>
	<view class="detail">
		<view class="title">
			{{datail.title}}
		</view>
		<view class="info">
			<view class="author">编辑：{{datail.author}}</view>
			<view class="time">发布日期：{{datail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="datail.content"></rich-text>
		</view>
		<view class="description">
			声明：
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				options:null,
				datail:{}
			};
		},
		onLoad(e) {
			this.options=e,
			this.getDetail()
		},
		methods:{
			getDetail(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
							
					data:this.options,
					success:res=>{
						console.log(res);
						res.data.content=res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
						this.datail=res.data
						this.saveHistory()
						uni.setNavigationBarTitle({
							title:this.datail.title
						})
					}
				})
			},
			saveHistory(){
				let historyArr=uni.getStorageSync("historyArr") || []
				let item={
					id:this.datail.id,
					classid:this.datail.classid,
					picurl:this.datail.picurl,
					title:this.datail.title
				}
				let index=historyArr.findIndex(i=>{
					return i.id==this.datail.id
				})
				if(index>=0){
					historyArr.splice(index,1)
				}
				historyArr.unshift(item)
				historyArr=historyArr.slice(0,10)
				uni.setStorageSync("historyArr", historyArr)
			}
		}
	}
</script>

<style lang="scss">
.detail{
	padding: 30rpx;
	.title{
		font-size: 46rpx;
		color: #333;
		
	}
	.info{
		background: #F6F6F6;
		padding: 10rpx;
		font-size: 22rpx;
		color: #666;
		display: flex;
		justify-content: space-between;
		margin: 40rpx 0;
	}
	.content{
		padding-top: 50rpx;
		/deep/ img{
			max-width: 100%;
		}
	}
	.description{
		background: #fef0f0;
		font-size: 26rpx;
		padding: 20rpx;
		color: #f89898;
		line-height: 1.8em;
	}
}
</style>
