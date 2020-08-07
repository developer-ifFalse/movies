<template>
	<view class="like">
		<view class="title">
			<text class="lg text-gray cuIcon-newshot"></text>
			<text class="name">{{title}}</text>
		</view>
		<view 
			class="list"
			v-for="item in inTheaters" :key = "item.id"	@click="handleGoToDetail(item.id)"	>
			<view class="pic">
				<image :src="item.images.small"></image>
			</view>
			<view class="info">
				<text class="name">{{item.title}}</text>
				<view class="detail">
					类型:{{item.genres.join(",")}}
				</view>
				<view class="detail">
					上映时间:{{item.mainland_pubdate}}
				</view>
				<view class="rate">
					<uniRate color="#ddd" :value="item.rating.average" max="5" size="14" disabled></uniRate>
					<text class="score">{{item.rating.average}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from '@dcloudio/uni-ui/lib/uni-rate/uni-rate.vue'
	export default {
		data() {
			return {

			};
		},
		components: {
			uniRate
		}, 
		props: ['inTheaters', 'title'],
		methods:{
			handleGoToDetail(id){
				uni.navigateTo({
					url:'/pages/detail/detail?id='+id
				})
			}
		}
	}
</script>

<style lang="scss">
	.title {
		height: 100rpx;
		font-weight: bold;
		font-size: 40rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding-left: 20rpx;

		.text-gray {
			color: hotpink;
		}

		.name {
			padding-left: 20rpx;
		}
	}

	.like {
		.list {
			display: flex;
			padding: 20rpx;

			.pic {
				width: 200rpx;
				height: 300rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.info {
				flex: 1;
				// height: 300rpx;
				

				.name {
					font-weight: bold;
					font-size: 40rpx;
					margin-left: 10rpx;
				}

				.rate {
					display: flex;
					justify-content: flex-start;
					align-items: center;
					margin-left: 10rpx;
					margin-top: 25rpx;
					.score {
						color: rgba($color: #000000, $alpha: 0.3);
						font-size: 30rpx;
						margin-left: 10rpx;
					}
				}

				.detail {
					margin-left: 10rpx;
					margin-top: 25rpx;
					display: block;
					overflow: hidden;
					display: -webkit-box;
					-webkit-box-orient: vertical;
					-webkit-line-clamp: 5;
				}
			}
		}
	}
</style>
