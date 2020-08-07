<template>
	<view class="trailer">
		<view class="title">
			<text class="lg text-gray cuIcon-newshot"></text>
			<text class="name">英雄预告</text>
		</view>

		<view class="video-border">
			<view 
				class="video"
				v-for="(item,index) in video"
				:key = "index"
			>
				<video :src="item.video_url"
							 :poster="item.pic_url">
				</video>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				video:[]
			};
		},
		created() {
		  const	_this = this
			uni.request({
				url:'https://h5sm.com/uni/api/index_video',
				method:"POST",
				success(res) {
					_this.video = res.data
					// console.log(_this.video)
				}
			})
		}
	}
</script>

<style lang="scss">
	.trailer {
		.video-border {
			display: flex;
			justify-content: space-around;

			.video {
				width: 350rpx;
				height: 220rpx;

				video {
					width: 100%;
					height: 100%;
				}
			}
		}
	}
</style>
