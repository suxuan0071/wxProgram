<template>
	<view class="root">
		<view class="head">
			<view class="userInfo">
				<image class="userImage" :src="imgAddress"></image>
				<view>
					<text class="userName">欢迎使用! {{userName}} ~</text>
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
		<view class="userList">
			<view class="userItem" v-for="item in userItems">
				<image class="avatar" :src="item.avatarAddr"></image>
				<view class="userName">
					<text>{{item.name}}</text>
				</view>
				<view class="userScore">
					<text>￥{{item.score}}</text>
				</view>
			</view>
			<view class="userItem">
				<image class="avatar" src="../../static/index/add.png"></image>
				<view class="userName">
					<text>添加好友</text>
				</view>
			</view>
		</view>

	</view>
</template>

<script lang="ts">
	export default {
		data() {
			return {
				title: 'Hello',
				imgAddress: '../../static/index/userImage.png',
				userName: 'sx0071',
				userItems: [{
						avatarAddr: '../../static/index/avatar.png',
						name: 'zc',
						score: '0'
					},
					{
						avatarAddr: '../../static/index/avatar.png',
						name: '六一',
						score: '100'
					}
				]
			}
		},
		onLoad() {},
		methods: {
			createRoom: function() {
				uni.navigateTo({
					url: '../room/room'
				})
			}
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

	.userList {
		display: flex;
		height: 200rpx;
		margin-top: 40rpx;
		border-top: solid 8rpx rgb(244, 244, 246);
		overflow-x: scroll;
		overflow-y: hidden;
	}

	.userItem {
		flex-shrink: 0;
		width: 120rpx;
		font-size: 28rpx;
		position: relative;

		.avatar {
			position: absolute;
			top: 20rpx;
			left: 20rpx;
			width: 80rpx;
			height: 80rpx;
			margin: 10rpx, 0;
			border-radius: 25px;
			border: solid 2rpx rgb(244, 244, 246);
		}

		.userName {
			position: absolute;
			top: 120rpx;
			left: 0;
			bottom: 0;
			right: 0;
			display: flex;
			justify-content: center;
			overflow: hidden;
			white-space: nowrap;
			text-overflow: ellipsis;
		}

		.userScore {
			position: absolute;
			top: 160rpx;
			right: 0;
			left: 0;
			font-size: 16px;
			display: flex;
			justify-content: center;
			font-weight: 600;
			color: rgb(158, 45, 37);
			// 	margin-bottom: 10rpx;
		}
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
