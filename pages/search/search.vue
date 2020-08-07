<template>
	<view class="search">
		<!-- 头部 开始 -->
		<view class="bg-gradual-blue padding radius text-center nav">
			<view style="height: 45rpx;"></view>
			<view class="cu-bar search">
				<view class="search-form round">
					<text class="cuIcon-search"></text>
					<input @focus="InputFocus" @blur="InputBlur" :adjust-position="false" type="text" placeholder="搜索电影" confirm-type="search"
					 v-model="searchText"></input>
				</view>
				<view class="action">
					<button class="cu-btn bg-green shadow-blur round" @click="search">搜索</button>
				</view>
			</view>
		</view>
		<!-- 头部  结束 -->

		<!-- 搜索列表 开始 -->

		<view class="list">
			<view v-if="putlist.length" class="listItem" @click="handleGoToDetail(item.id)" v-for="(item,index) in putlist" :key="item.index">
				<image :src="item.images.small"></image>
				<text>{{item.title}}</text>
			</view>
			<view v-if="putlist.length"></view>
			<view v-else>没有找到!</view>
		</view>

		<!-- 搜索列表 结束 -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchText: '',
				InputBottom: 0,
				list: [],
				putlist: []
			}
		},
		methods: {
			InputFocus(e) {
				this.InputBottom = e.detail.height
			},
			InputBlur(e) {
				this.InputBottom = 0
			},
			search() {
				this.putlist = this.list.filter((item, index) => {
					return item.title.includes(this.searchText)
				})
			},
			handleGoToDetail(id) {
				uni.navigateTo({
					url: '/pages/detail/detail?id=' + id
				})
			}
		},
		onLoad() {
			const _this = this
			uni.request({
				url: 'https://h5sm.com/uni/api/coming_soon?start=1?count=10',
				method: "POST",
				success(res) {
					_this.putlist = res.data.subjects
					_this.list = _this.putlist
					// console.log(_this.list)
				}
			})
			uni.request({
				url: 'https://h5sm.com/uni/api/top250?start=2?count=10',
				method: "POST",
				success(res) {
					_this.putlist = [..._this.putlist, ...res.data.subjects]
					_this.list = _this.putlist
					// console.log(_this.list)
				}
			})
			uni.request({
				url: 'https://h5sm.com/uni/api/in_theaters?start=1?count=10',
				method: "POST",
				success(res) {
					_this.putlist = [..._this.putlist, ...res.data.subjects]
					_this.list = _this.putlist
					// console.log(_this.putlist)
				}
			})
			// this.putlist = this.list
			// console.log(this.putlist)
		},
		watch: {
			searchText: function(val) {
				this.putlist = this.list.filter((item, index) => {
					return item.title.includes(this.searchText)
				})
				console.log(this.putlist.length)
			}
		}
	}
</script>

<style scoped lang="scss">
	.search {

		//头部 开始
		.padding {
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
		// 搜索结果列表 开始
		.list {
			display: flex;
			flex-wrap: wrap;
			margin-top: 200rpx;
			.listItem {
				padding-top: 20rpx;
				padding-left: 37rpx;
				max-width: 274rpx;

				// border: 1rpx solid red;
				image {
					width: 200rpx;
					height: 300rpx;
				}

				text {
					width: 200rpx;
					display: block;
					text-align: center;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}
			}
		}

		// 搜索结果列表 结束
	}
</style>
