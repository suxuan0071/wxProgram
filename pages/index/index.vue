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
			<view class="userItem" v-for="item in userItems" :key="item.score">
				<image class="avatar" :src="item.avatarAddr" @click="openPopUp(item.pid)"></image>
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
			<view>
				<u-popup class="popup" ref="popup" @close="popupClose" :show="show" mode="center" mask="true">
					<view class="confirmBox">
						<view class="confirmBoxHead">
							<view class="confirmBoxUserItem">
								<image class="avatar" :src="payerInfo.avatarAddr"></image>
								<view class="userName">
									<text>{{payerInfo.name}}</text>
								</view>
							</view>
							<view class="info">
								<view>
									支出分数
								</view>
								<image src="../../static/index/arrowRight.png" class="arrow"></image>
							</view>
							<view class="confirmBoxUserItem">
								<image class="avatar" :src="reciverInfo.avatarAddr"></image>
								<view class="userName">
									<text>{{reciverInfo.name}}</text>
								</view>
							</view>
						</view>
						<view class="inputBox">
							<u--input  placeholder="请输入支出分数" v-model="inputScore" type="number"></u--input>
						</view>
						<view class="confirmBtn">
							<u-button  text="支出" type="primary" size="middle" @click="confirmClick">
							</u-button>
						</view>
						
					</view>
				</u-popup>
				<u-notify ref="uNotify" message="自己给自己转没用哟~" :show="notify"></u-notify>
			</view>
		</view>
		<view class="msgBox" ref="msgBoxRef">
			<view class="reflashInfo">
				{{dateTime}} 刷新成功
			</view>
			<view class="scoreInfo" v-for="item in scoreList" :key="item.score">
				<text class="payer">{{item.payer}}</text>
				向
				<text class="reciver">{{item.reciver}}</text>支付
				<text class="score">{{item.score}}</text>分
				<text class="dateTime">{{item.dateTime}}</text>
			</view>
			<view class="joinRoom" v-for="item in roomList" :key="item.pid">
				{{item.name}}加入了房间
			</view>
		</view>
		<!-- <uni-popup ref="popup" type="dialog">
			<uni-popup-dialog mode="input" message="成功消息" :duration="2000" :before-close="true" ></uni-popup-dialog>
		</uni-popup> -->

	</view>


</template>

<script lang="js">
	export default {
		data() {
			return {
				title: 'Hello',
				imgAddress: '../../static/index/userImage.png',
				userName: 'sx0071',
				userId: 1,
				userItems: [{
						avatarAddr: '../../static/index/avatar.png',
						name: 'zc',
						score: 0,
						pid: 1
					},
					{
						avatarAddr: '../../static/index/avatar.png',
						name: '六一',
						score: 100,
						pid: 2
					}
				],
				roomList: [{
					name: 'zc',
					pid: 1
				}, {
					name: '六六',
					pid: 2
				}],
				scoreList: [],
				dateTime: '10:24:30',
				show: false,
				notify: false,
				calendarShow: false,
				payerInfo: {
					name: 'zc',
					avatarAddr: '../../static/index/userImage.png'
				},
				reciverInfo: {
					name: '六一',
					avatarAddr: '../../static/index/userImage.png'
				},
				inputScore: ""
			}
		},
		onLoad() {},
		methods: {
			createRoom: function() {
				uni.navigateTo({
					url: '../room/room'
				})
			},
			setMsgBoxHeight: function() {

			},
			getReciver: function(pid) {
				if (pid === this.userId) {
					// this.$refs.uNotify.show({
					// 	top: 40,
					// 	type: 'warning',
					// 	message: '自己给自己转没用哟~',
					// 	duration: 1000 * 3,
					// 	fontSize: 20,
					// 	safeAreaInsetTop: true
					// })
				} else {
					this.userItems.forEach(item => {
						if (item.pid === pid) {
							this.reciverInfo = item
						}
					})
					this.show = true
				}
			},
			openPopUp: function(pid) {
				this.getReciver(pid)
			},
			popupClose: function() {
				this.show = false
			},
			confirmClick: function() {
				this.show = false
				//payer扣分
				for (let i = 0; i < this.userItems.length; i++) {
					if (this.userItems[i].name === this.payerInfo.name) {
						this.userItems[i].score -= this.inputScore
					}
					if (this.userItems[i].name === this.reciverInfo.name) {
						this.userItems[i].score += parseInt(this.inputScore)
					}
				}
				let myDate = new Date();
				let str = myDate.toTimeString(); 
				let timeStr = str.substring(0, 8);
				this.scoreList.unshift({
					payer: this.payerInfo.name,
					reciver: this.reciverInfo.name,
					score: this.inputScore,
					dateTime: timeStr
				})
				this.inputScore = ""
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
		margin-bottom: 20rpx;
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
			font-size: 15px;
			display: flex;
			justify-content: center;
			font-weight: 600;
			color: rgb(158, 45, 37);
			// 	margin-bottom: 10rpx;
		}
	}

	.msgBox {
		background-color: rgb(246, 246, 246);
		border-top: solid 8rpx rgb(244, 244, 246);
		height: 800rpx;

		.joinRoom,
		.reflashInfo {
			background-color: rgb(206, 206, 206);
			font-size: 30rpx;
			color: white;
			width: fit-content;
			margin: 20rpx auto;
			padding: 0 10rpx;
			border-radius: 10rpx;
			text-align: center;
		}

		.scoreInfo {
			background-color: white;
			height: 100rpx;
			line-height: 100rpx;

			text {
				font-size: 36rpx;
				margin: 0 10rpx;
				font-weight: 600;
			}

			.payer,
			.reciver {
				color: rgb(84, 148, 189);
			}

			.score {
				color: rgb(158, 45, 37);
			}

			.dateTime {
				float: right;
				font-size: 28rpx;
				font-weight: 500;
				color: rgb(182, 182, 182);
			}
		}
	}

	.popup {}

	.confirmBox {
		height: 500rpx;
		width: 600rpx;
		background-color: white;
		font-size: 30rpx;

		.confirmBoxHead {
			margin: 40rpx auto;
			display: flex;
			padding: 20rpx 40rpx;
			text-align: center;
			font-weight: 600;
			color: rgb(109, 181, 255);

			.info {
				flex: 2;

				image {
					width: 100rpx;
					height: 60rpx;
				}
			}

			.confirmBoxUserItem {
				flex: 1;

				.avatar {
					margin: 10rpx, 0;
					border-radius: 25px;
					border: solid 2rpx rgb(244, 244, 246);
					width: 80rpx;
					height: 80rpx;
				}
			}
		}

		.inputBox {
			margin: 0 80rpx;
		}

		.confirmBtn {
			margin-left: 80rpx;
			margin-top: 40rpx;
			width: 74%;
		}
	}
</style>
