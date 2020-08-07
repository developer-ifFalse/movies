<template>
	<view class="list">
		<view class="listItem" v-for="(item,index) in putlist" :key="item.index">
			<image :src="item.imageUrl"></image>
			<text>{{item.name}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchText: '',
				InputBottom: 0,
				list: [{
					name: "我们的乐队",
					imageUrl: "https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2589466796.jpg"
				}, {
					name: "假偶天成",
					imageUrl: "https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2571538896.jpg"
				}, {
					name: "手可摘星辰",
					imageUrl: "https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2588012755.jpg"
				}, {
					name: "手可摘星辰",
					imageUrl: "https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2588012755.jpg"
				}],
				putlist: []
			}
		},
		methods:{
			search(){
				this.putlist = this.list.filter((item,index) =>{
					return item.name.includes(this.searchText)
				})
				this.searchText = '' 
			}
		},
		onLoad() {
			this.putlist = this.list
		},
		watch:{
			searchText: function(val){
				this.putlist = this.list.filter((item,index) =>{
					return item.name.includes(this.searchText)
				})
			}
		}
	}
</script>

<style lang="scss">
	.list {
		display: flex;
		flex-wrap: wrap;
		.listItem {
			padding-top: 20rpx;
			padding-left: 37rpx;
			image {
				width: 200rpx;
				height: 300rpx;
			}
	
			text {
				display: block;
				text-align: center;
			}
		}
	}
</style>
