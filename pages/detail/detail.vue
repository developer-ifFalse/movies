<template>
	<view class='movieDetail'>
		<!-- 轮播 -->
		<swiper class="screen-swiper" :class="dotStyle?'square-dot':'round-dot'" :indicator-dots="true" :circular="true"
		 :autoplay="true" interval="5000" duration="500">
			<swiper-item v-for="(item,index) in trailers" :key="index">
				<video :poster="item.medium" :src="item.resource_url" loop :show-play-btn="false" :controls="true" objectFit="cover"></video>
				{{item.resource_url}}
			</swiper-item>
		</swiper>

		<!-- 电影信息 -->
		<view class="list">
			<view v-if="falg" class="pic">
				<image :src="info.images.small"></image>
			</view>
			<view v-if="falg" class="info">
				<text class="name">{{info.title}}</text>
				<view class="detail">
					上映时间:{{info.mainland_pubdate}}
				</view>
				<view class="detail">
					导演:{{info.directors[0].name}}
				</view> 
				<view class="detail">
					制片国家:{{info.countries.join('')}}
				</view>
				<view class="detail">
					类型:{{info.genres.join(",")}}
				</view>
				<view class="detail">
					影片时长:{{info.durations.join(',')}}
				</view>
			</view>
		</view>

		<!-- 电影介绍 -->
		<view class="info_des">
			<view class="info_title">剧情介绍</view>
			<view class="des">{{info.summary}}</view>
		</view>

		<!-- 演员阵容 -->
		<!-- #ifndef MP-ALIPAY -->
		<view class="info_title">演职人员</view>
		<view class="tower-swiper" @touchmove="TowerMove" @touchstart="TowerStart" @touchend="TowerEnd">
			<view class="tower-item" :class="item.zIndex==1?'none':''" v-for="(item,index) in swiperList" :key="index" :style="[{'--index': item.zIndex,'--left':item.mLeft}]" :data-direction="direction">
				<view class="swiper-item">
					<image :src="item.avatars.small" mode="aspectFill" ></image>
				</view> 
			</view> 
		</view>
		<!-- #endif -->
		
	</view>


</template>

<script>
	export default {
		data() {
			return {
				cardCur: 0,
				swiperList: [],
				dotStyle: false,
				towerStart: 0,
				direction: '',
				trailers:[],
				info:{},
				falg:false
			};
		},
		onLoad(id) {
			this.TowerSwiper('swiperList');
			// 初始化towerSwiper 传已有的数组名即可
			const _this = this
			// console.log(id)
			uni.request({
				url:'https://h5sm.com/uni/api/movie_subject',
				method:'POST',
				data:{
					...id
				},
				success(res) { 
					const { 
						trailers,
						images,
						title,
						mainland_pubdate,
						countries,
						genres,
						durations,
						directors,
						tags,
						summary,
						casts
					} = res.data
					_this.trailers = trailers 
					_this.info = {
						images,
						title,
						mainland_pubdate,
						countries,
						genres,
						durations,
						directors,
						tags,
						summary
					}
					_this.falg = true
					_this.swiperList = casts
					_this.TowerSwiper('swiperList');
					// console.log(_this.swiperList)
				}
			})
			uni.setNavigationBarTitle({
			    title: '电影详情'
			});
		},
		methods: {
			DotStyle(e) {
				this.dotStyle = e.detail.value
			},
			// cardSwiper
			cardSwiper(e) {
				this.cardCur = e.detail.current
			},
			// towerSwiper
			// 初始化towerSwiper
			TowerSwiper(name) {
				let list = this[name];
				for (let i = 0; i < list.length; i++) {
					list[i].zIndex = parseInt(list.length / 2) + 1 - Math.abs(i - parseInt(list.length / 2))
					list[i].mLeft = i - parseInt(list.length / 2)
				}
				this.swiperList = list
			},
	
			// towerSwiper触摸开始
			TowerStart(e) {
				this.towerStart = e.touches[0].pageX
			},
	
			// towerSwiper计算方向
			TowerMove(e) {
				this.direction = e.touches[0].pageX - this.towerStart > 0 ? 'right' : 'left'
			},
	
			// towerSwiper计算滚动
			TowerEnd(e) {
				let direction = this.direction;
				let list = this.swiperList;
				if (direction == 'right') {
					let mLeft = list[0].mLeft;
					let zIndex = list[0].zIndex;
					for (let i = 1; i < this.swiperList.length; i++) {
						this.swiperList[i - 1].mLeft = this.swiperList[i].mLeft
						this.swiperList[i - 1].zIndex = this.swiperList[i].zIndex
					}
					this.swiperList[list.length - 1].mLeft = mLeft;
					this.swiperList[list.length - 1].zIndex = zIndex;
				} else {
					let mLeft = list[list.length - 1].mLeft;
					let zIndex = list[list.length - 1].zIndex;
					for (let i = this.swiperList.length - 1; i > 0; i--) {
						this.swiperList[i].mLeft = this.swiperList[i - 1].mLeft
						this.swiperList[i].zIndex = this.swiperList[i - 1].zIndex
					}
					this.swiperList[0].mLeft = mLeft;
					this.swiperList[0].zIndex = zIndex;
				}
				this.direction = ""
				this.swiperList = this.swiperList
			},
		}
	}
</script>

<style lang="scss">
	.movieDetail {
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

		.list {
			display: flex;
			padding: 20rpx;

			.pic {
				width: 280rpx;
				height: 380rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.info {
				flex: 1;
				overflow: hidden;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
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
					color: rgba($color: #000000, $alpha: 0.3);
					margin-left: 10rpx;
					margin-top: 10rpx;
					display: block;
					overflow: hidden;
					display: -webkit-box;
					-webkit-box-orient: vertical;
					-webkit-line-clamp: 5;
				}
			}
		}

		.info_des {
			// padding: 20rpx 20rpx;

			.info_title {
				font-size: 36rpx;
				color: rgba($color: #000000, $alpha: 0.3);
			}

			.des {
				padding: 0rpx 20rpx;
				color: #333;
			}
		}
		.info_title{
			padding: 20rpx 20rpx;
			font-size: 36rpx;
			color: rgba($color: #000000, $alpha: 0.3);
		}
			
		.tower-swiper .tower-item {
			transform: scale(calc(0.5 + var(--index) / 10));
			margin-left: calc(var(--left) * 100upx - 150upx);
			z-index: var(--index);
		}
	}
</style>
