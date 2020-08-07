<template>
	<view class="user">
		<!-- 头部开始 -->
		<view class="bg-gradual-blue padding radius text-center nav">
			<view style="height: 25rpx;"></view>
			<view class="margin-top-sm text-Abc">
				<view class="cu-bar">
					<view class="content text-bold">
						用户
					</view>
				</view>
			</view>
		</view>
		<!-- 头部结束 -->
		<!-- 登录注册 -->
		
		
		<view class="bg-gradual-blue">
			<scroll-view scroll-y class="DrawerPage" :class="modalName=='viewModal'?'show':''">		
				<view class="cu-list menu-avatar" @tap="showModal" data-target="viewModal">
					<view class="cu-item">
						<view class="cu-avatar round lg" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10001.jpg);"></view>
						<view class="content">
							<view class="text-grey">登录/注册</view>
						</view>
					</view>
				</view>
				<view v-if="logon" class="cu-list menu card-menu margin-top-xl margin-bottom-xl shadow-lg">
					<view class="cu-item " v-for="(item,index) in userIofo" :key="index">
						<view class="content">
							<text class="cuIcon-github text-grey"></text>
							<text class="text-grey">{{item}}</text>
						</view>
						<view class="user_info">{{item}}</view>
					</view>
					<view class="cu-item">
						<view class="content">
							<text class="cuIcon-github text-grey"></text>
							<text class="text-grey">退出</text>
						</view>
					</view>
				</view>
			</scroll-view>
			<view class="DrawerClose" :class="modalName=='viewModal'?'show':''" @tap="hideModal">
				<text class="cuIcon-pullright"></text>
			</view>
			<scroll-view scroll-y class="DrawerWindow" :class="modalName=='viewModal'?'show':''">
				<view class="cu-form-group margin-top">
					<view class="title">账号:</view>
					<input v-model="name" placeholder="请输入账号" name="input"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">密码:</view>
					<input v-model="password" type="password" placeholder="请输入密码" name="input"></input>
				</view>
				<view class="padding">
					<button v-if="flag" class="cu-btn block bg-blue margin-tb-sm lg" @click="logons" >登录/注册</button>
					<button v-else class="cu-btn block bg-blue margin-tb-sm lg">
						<text class="cuIcon-loading2 cuIconfont-spin"></text> 登录中</button>
				</view>
			</scroll-view>
		</view>
		<!-- 登录注册 -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				modalName:null,
				logon:true,
				name:'',
				password:'',
				flag:true,
				userIofo:['昵称','性别','年龄']
			}
		},
		onLoad() {
			
		},
		methods: {
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			},
			logons(){
				this.flag = false
				let name = this.name
				let password = this.password
				console.log(name)
				uni.request({
					url:'https://h5sm.com/uni/users/loginAndRegister',
					method:'POST', 
					data:{
						name,
						password
					},
					success(res) {
						console.log(res)
					}
				})
			}
		},
	}
</script>

<style lang="scss">
	.padding {
		padding-bottom: 0;
	}
	
	page {
		background-image: var(--gradualBlue);
		width: 100vw;
		overflow: hidden;
	}
	
	.user_info{
		// border: 1rpx solid red;
		color: #8799a3;
	}
	
	.DrawerPage {
		position: fixed;
		width: 100vw;
		height: 100vh;
		left: 0vw;
		background-color: #f1f1f1;
		transition: all 0.4s;
	}
	
	.DrawerPage.show {
		transform: scale(0.9, 0.9);
		left: 85vw;
		box-shadow: 0 0 60upx rgba(0, 0, 0, 0.2);
		transform-origin: 0;
	}
	
	.DrawerWindow {
		position: absolute;
		width: 85vw;
		height: 100vh;
		left: 0;
		top: 45rpx;
		transform: scale(0.9, 0.9) translateX(-100%);
		opacity: 0;
		pointer-events: none;
		transition: all 0.4s;
		padding: 100upx 0;
	}
	
	.DrawerWindow.show {
		transform: scale(1, 1) translateX(0%);
		opacity: 1;
		pointer-events: all;
	}
	
	.DrawerClose {
		position: absolute;
		width: 40vw;
		height: 100vh;
		right: 0;
		top: 0;
		color: transparent;
		padding-bottom: 30upx;
		display: flex;
		align-items: flex-end;
		justify-content: center;
		background-image: linear-gradient(90deg, rgba(0, 0, 0, 0.01), rgba(0, 0, 0, 0.6));
		letter-spacing: 5px;
		font-size: 50upx;
		opacity: 0;
		pointer-events: none;
		transition: all 0.4s;
	}
	
	.DrawerClose.show {
		opacity: 1;
		pointer-events: all;
		width: 15vw;
		color: #fff;
	}
	
	.DrawerPage .cu-bar.tabbar .action button.cuIcon {
		width: 64upx;
		height: 64upx;
		line-height: 64upx;
		margin: 0;
		display: inline-block;
	}
	
	.DrawerPage .cu-bar.tabbar .action .cu-avatar {
		margin: 0;
	}
	
	.DrawerPage .nav {
		flex: 1;
	}
	
	.DrawerPage .nav .cu-item.cur {
		border-bottom: 0;
		position: relative;
	}
	
	.DrawerPage .nav .cu-item.cur::after {
		content: "";
		width: 10upx;
		height: 10upx;
		background-color: currentColor;
		position: absolute;
		bottom: 10upx;
		border-radius: 10upx;
		left: 0;
		right: 0;
		margin: auto;
	}
	
	.DrawerPage .cu-bar.tabbar .action {
		flex: initial;
	}
	.cu-form-group .title {
		// min-width: calc(4em + 15px);
		color: black;
	}
</style>
