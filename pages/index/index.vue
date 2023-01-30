<template>
	<view class="root">
		<view class="head">
			<view class="userInfo">
				<image class="userImage" :src="imgAddress"></image>
				<view>
					<text class="userName">欢迎使用! {{userItem.name}} ~</text>
				</view>
			</view>
		</view>
		<view class="btnBox">
			<u-row gutter="12">
				<u-col span="6">
					<u-button class="button" type="primary" size="middle" plain ripple ripple-bg-color
						@click="createRoom">创建房间</u-button>
				</u-col>
				<u-col span="6">
					<u-button class="button" type="primary" size="middle" plain ripple ripple-bg-color>扫码进房</u-button>
				</u-col>
			</u-row>
		</view>
	</view>


</template>

<script lang="js">
	export default {
		data() {
			return {
				title: 'Hello',
				imgAddress: '../../static/index/pk.png',
				userItem:{}
				
			}
		},
		onLoad() {
			uni.authorize({
				scope:'scope.userInfo'
			}),
			uni.getUserInfo({
				desc:'获取用户的头像以及名称',
				success:function(res){		
					let random = Math.floor(Math.random() * 9999)
					const userItem = {
							avatarAddr:res.userInfo.avatarUrl,
							name:res.userInfo.nickName + random ,
							score:0,
							pid:res.cloudID
					}
					this.userItem = userItem
					uni.setStorage({
						key:'userItem',
						data:userItem
					})
				}.bind(this)
			}),
			uni.getSystemInfo({
				success: (res) => {
					uni.setStorage({
						key:'windowHeight',
						data:res.windowHeight
					})
				}
			})
		},
		methods: {
			createRoom: function() {
				uni.navigateTo({
					url: '../room/room'
				})
			},
		},

	}
</script>

<style lang="scss">
	.head {
		.userInfo {
			position: relative;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 200rpx;

			.userImage {
				width: 100rpx;
				height: 100rpx;
				margin-right: 60rpx;
			}
		}
	}

	.btnBox {
		.button {
			width: 300rpx;
		}
	}
</style>
