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
			<u-row gutter="18">
				<u-col span="1"></u-col>
				<u-col span="5">
					<u-button class="button" type="primary" size="middle" plain ripple ripple-bg-color
						@click="createRoom">创建房间</u-button>
				</u-col>
				<u-col span="5">
					<u-button class="button" @click="scanCode" type="primary" size="middle" plain ripple
						ripple-bg-color>扫码进房</u-button>
				</u-col>
				<u-col span="1"></u-col>
			</u-row>
		</view>
		<view class="changeAvatar">
			<view class="text">
				更换头像、昵称
			</view>
			<u-action-sheet @select="actionSelect" :closeOnClickOverlay="true" :actions="list" :show="show">
			</u-action-sheet>
			<button class="avatarWrap" open-type="chooseAvatar" @chooseavatar="onChooseavatar">
				<image class="avatar" :src="defaultAvatarAddr" style="width: 300rpx; height: 300rpx;" mode=""></image>
			</button>
			<view class="tips">
				点击logo可快捷替换微信头像
			</view>
			<view class="defaultInputBox">
				<u--input class="nameInput" inputAlign="center" color="#2979ff" v-model="defaultName"
					placeholder="请输入昵称"></u--input>
			</view>
			<view class="btnWrap">
				<u-button class="confirmChange" @click="confirmChange" type="primary" plain>确认</u-button>
			</view>

		</view>
	</view>


</template>

<script lang="js">
	export default {
		data() {
			return {
				title: 'Hello',
				imgAddress: '../../static/index/pk.png',
				userItem: {},
				defaultAvatarAddr: "",
				defaultName: "",
			}
		},
		onLoad() {
			uni.authorize({
					scope: 'scope.userInfo'
				}),
				uni.getUserInfo({
					desc: '获取用户的头像以及名称',
					success: function(res) {
						let random = Math.floor(Math.random() * 9999)
						const userItem = {
							avatarAddr: res.userInfo.avatarUrl,
							name: res.userInfo.nickName + random,
							score: 0,
							pid: res.cloudID
						}
						this.defaultAvatarAddr = res.userInfo.avatarUrl
						this.defaultName = userItem.name
						//本地先存储一份
						this.userItem = userItem
						uni.setStorage({
							key: 'userItem',
							data: this.userItem
						})
					}.bind(this)
				}),
				uni.getSystemInfo({
					success: (res) => {
						uni.setStorage({
							key: 'windowHeight',
							data: res.windowHeight
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
			scanCode: function() {
				wx.scanCode({
					onlyFromCamera: true,
					success(res) {
					}
				})
			},
			onChooseavatar:function(data){
				this.defaultAvatarAddr = data.detail.avatarUrl
				this.userItem.avatarAddr = data.detail.avatarUrl
				uni.setStorage({
					key: 'userItem',
					data: this.userItem
				})
			},
			confirmChange: function() {
				this.userItem.name = this.defaultName
				uni.setStorage({
					key: 'userItem',
					data: this.userItem
				})
			}
		}
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
			border-radius: 20rpx;
		}
	}

	.changeAvatar {
		position: relative;
		text-align: center;
		border: 4rpx solid rgb(241, 241, 241);
		box-shadow: 2rpx 4rpx 4rpx rgba(0, 0, 255, .2);
		margin: 40rpx auto 0 auto;
		width: 80%;

		.text {
			display: block;
			margin-top: 20rpx;
		}

		.tips {
			font-size: 26rpx;
			margin-top: 20rpx;
		}
		.avatarWrap{
			border-radius: 10rpx;
			margin-top: 20rpx;
			width: 300rpx;
			height: 300rpx;
			border-style: none;
			.avatar{
				margin-left: -30rpx;
			}
		}
		

		.defaultInputBox {
			width: 60%;
			// position: absolute;
			margin: 40rpx auto;
			text-align: center;
		}

		.btnWrap {
			width: 60%;
			margin: 0 auto 80rpx auto;
		}
	}
</style>
