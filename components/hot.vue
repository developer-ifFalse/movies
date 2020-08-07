<template>
	<view class="hotmovie">
		<view class="title">
			<text class="lg text-gray cuIcon-hot"></text>
			<text class="name">{{title}}</text>
		</view>
		<view class="list">
			<scroll-view class="scroll-view" scroll-x>
				<view class="scroll-item" v-for="item in comingsoonList" :key="item.id" @click="handleGoToDetail(item.id)">
					<view class="info">
						<view class="pic">
							<image :src="item.images.small"></image>
						</view>
						<view class="name">{{item.title}}</view>
						<view class="rate" v-if="item.rating.average"><uniRate color="#ddd" :value="item.rating.average" max="5" size="14" disabled></uniRate><text class="score">{{item.rating.average}}</text></view>
					</view>
				</view>
			</scroll-view>
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
		props: ['comingsoonList','title'],
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
	.hotmovie {
		 padding-top: 50rpx;

		.list {
			white-space: nowrap;


			.scroll-item {
				display: inline-block;
				width: 200rpx;
				height: 400rpx;
				margin: 0 20rpx;
				vertical-align: top;

				.info {
					display: block;

					.pic {
						width: 200rpx;
						height: 300rpx;

						image {
							width: 100%;
							height: 100%;
						}
					}

					.name {
						font-weight: bold;
						font-size: 30rpx;
						margin: 10rpx 0 10rpx;
						overflow: hidden;
						text-overflow: ellipsis;
						white-space: nowrap;
					}

					.rate {
						margin-top: 10rpx;
						display: flex;
						justify-content: flex-start;
						align-items: center;

						.score {
							color: rgba($color: #000000, $alpha: 0.3);
							font-size: 30rpx;
						}
					}
				}
			}
		}
	}
</style>
