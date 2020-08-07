<template>
	<view class="main">
		<view class="bg-gradual-blue padding radius text-center nav">
			<view style="height: 25rpx;"></view>
			<view class="margin-top-sm text-Abc">
				<view class="cu-bar" >
					<view class="content text-bold">
						拼夕夕电影
					</view>
				</view>
			</view>
		</view>

		<!-- 轮播开始 -->
		<Banner></Banner>
		<!-- 轮播结束 -->
		<!-- 即将上映 开始 -->
		<Hot title = "即将上映" :comingsoonList = "comingsoonList"></Hot>
		<!-- 即将上映 结束 -->

		<!-- 预告 开始 -->
		<Notice></Notice>
		<!-- 预告 结束 -->
		
		<!-- 排行榜 开始 -->
		<Hot title = "排行榜" :comingsoonList = "top250List"></Hot>
		<!-- 排行榜结束 -->
		
		<!-- 猜你喜欢 开始-->
		<Like title = "正在热映" :inTheaters = "inTheaters"></Like>
		<!-- 猜你喜欢 结束-->
	</view>  
</template>

<script>
	import uniRate from '@dcloudio/uni-ui/lib/uni-rate/uni-rate.vue'
	import Banner from '../../components/Banner.vue'
	import Hot from '../../components/hot.vue'
	import Notice from '../../components/Notice.vue'
	import Like from '../../components/Like.vue'
	export default {
		components: {
			uniRate,
			Banner,
			Hot,
			Notice,
			Like
		},
		data() {
			return {
				comingsoonList:[],
				top250List:[],
				inTheaters:[]
			}
		},
		onLoad() {
			var _this = this
			uni.request({
				url:'https://h5sm.com/uni/api/coming_soon?start=1?count=10',
				method:"POST",
				success(res) {
					_this.comingsoonList = res.data.subjects
				}
			})
			uni.request({
				url:'https://h5sm.com/uni/api/top250?start=2?count=10',
				method:"POST",
				success(res) {
					_this.top250List = res.data.subjects
				}
			})
			uni.request({
				url:'https://h5sm.com/uni/api/in_theaters?start=1?count=10',
				method:"POST",
				success(res) {
					_this.inTheaters = res.data.subjects
				}
			})
		},
		methods: {
		}
	}
</script>

<style lang="scss">
	.main {

		//头部 开始
		.padding{
			padding-bottom: 0;
		}
		.nav{
			position: fixed;
			left: 0;
			top: 0;
			width: 100%;
			z-index: 100;
		}
		//头部 结束
	}
</style>
