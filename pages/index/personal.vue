<template>
	<view class="mycss1" :style="{'height':screenHeight+'px'}">
		<view class="status_bar">
			<!--  这里是状态栏 -->
			<!-- &nbsp; -->
		</view>
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">
			<view class="mycss">我的</view>
			<view class="bgClass" @click="toMyself">
				<image class="image2" :src="luser.headPortrait"></image>
				<text class="userName">{{luser.nickName}}</text>
				<text class="userId">ID:{{luser.username}}</text>
			</view>
			<view class="li">

				<!-- <view class="liCss" @click="beginDive">
					<image class="iconClass" src="../../static/livePic.png"></image>
					开始直播
					<text class="rightClass">〉</text>
				</view> -->

				<navigator url="/pages/personal/myProfile">
					<view class="liCss">
						<image class="iconClass" src="../../static/livePic (3).png"></image>
						个人资料
						<text class="rightClass">〉</text>
					</view>
				</navigator>

				<navigator url="/pages/personal/liveSettings">
					<view class="liCss">
						<image class="iconClass" src="../../static/livePic (1).png"></image>
						直播设置
						<text class="rightClass">〉</text>
					</view>
				</navigator>

				<navigator url="/pages/personal/myDevice">
					<view class="liCss">
						<image class="iconClass" src="../../static/livePic (2).png"></image>
						设备管理
						<text class="rightClass">〉</text>
					</view>
				</navigator>

				<navigator url="/pages/personal/aboutUs">
					<view class="liCss">
						<image class="iconClass" src="../../static/livePic (4).png"></image>
						关于我们
						<text class="rightClass">〉</text>
					</view>
				</navigator>

				<navigator url="/pages/personal/privacyAgreement">
					<view class="liCss">
						<image class="iconClass" src="../../static/livePic (5).png"></image>
						隐私协议
						<text class="rightClass">〉</text>
					</view>
				</navigator>

				<view class="liCss" @click="toQuit">
					<image class="iconClass" src="../../static/livePic (6).png"></image>
					退出登录
					<text class="rightClass">〉</text>
				</view>

			</view>

		</view>


		<!-- 退出登录弹窗 -->
		<uni-popup ref="quit" type="dialog" class="popupClass">
			<uni-popup-dialog mode="base" class="popupClass" type="error" title="退出登录" content="您即将退出登录" message="成功消息"
				:before-close="true" @close="close" @confirm="quit"></uni-popup-dialog>
		</uni-popup>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				//屏幕高度
				screenHeight: 0,
				//登录用户
				luser: {}

			}
		},
		onBackPress() {
			//console.log(JSON.stringify(this.$refs.popup.ty));
			this.$refs.quit.close();
		},
		onReady() {
			this.Info();
		},
		methods: {
			Info: function() {
				try {
					const luser = uni.getStorageSync("luser");
					if (luser) {
						//console.log(luser);
						this.luser = luser;
					}
				} catch (e) {
					console.error(e);
				}
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						that.screenHeight = res.screenHeight - 60;
					},
				});
			},
			beginDive: function() {
				//console.log('开始直播')
				uni.switchTab({
					url: '/pages/index/index'
				});
			},
			toMyself: function() {
				uni.navigateTo({
					url: "/pages/personal/myProfile"
				});
			},
			toQuit: function() {
				this.$refs.quit.open();
			},
			close: function(done) {
				//this.$refs.quit.close();
				done();
			},
			quit: function(done) {

				/* uni.request({
					url: '',
					data: {
						facility: 'facility',
						account: 'account',
					},
					success: (res) => {
						console.log(res.data);
					}
				}); */

				//从本地缓存中同步移除指定 key
				try {
					uni.clearStorageSync();
					uni.redirectTo({
						url: "/pages/index/login"
					});
				} catch (e) {
					//console.log('err' + e);
				}
				done();
			},
		}
	}
</script>

<style>
	.rightClass {
		float: right;
		margin-right: 20px;
		color: #999;
		font-weight: bold;
		font-size: 15px;
	}

	.bgClass {
		background-image: url(../../static/topBanner.png);
		/* border: #0062CC 1px solid; */
		width: 100%;
		height: 170px;
		background-size: 100% 100%;
	}

	.status_bar {
		height: var(--status-bar-height);
		background-color: #353637;
		width: 100%;
	}

	.mycss1 {
		/* border:#4CD964 3px solid; */

	}

	.iconClass {
		width: 18px;
		height: 17px;
		margin-left: 20px;
		margin-right: 10px;
	}

	.mycss {
		font-size: 18px;
		width: 100%;
		text-align: center;
		text-align: center;
		position: fixed;
		color: #fff;
		padding: 5px 5px;
		/* top: 50px; */
		/* justify-content: center; */
	}

	.screen {
		background-color: rgb(225, 225, 225);
		/* border: blue 3px solid; */
	}

	.image2 {
		position: fixed;
		top: 100px;
		left: 45px;
		border: 1px solid #ffffff;
		width: 80px;
		height: 80px;
		border-radius: 50%;
	}

	.userId {
		position: fixed;
		left: 138px;
		color: #777;
		font-size: 18px;
		top: 138px;
	}

	.userName {
		position: fixed;
		left: 138px;
		color: #555;
		font-size: 20px;
		font-weight: bold;
		top: 112px;
	}

	.li {
		/* border: blue 3px solid; */
		/* position: fixed;
		top:180px; */
		padding-top: 30px;
	}

	.liCss {
		/* border: red 1px solid; */
		width: 100%;
		padding: 15px 0px;
		background: #FFFFFF;
		overflow: hidden;
		display: block;
		/* margin-bottom:0px; */
		//border: 1px solid red;
		border-bottom: 1px solid #e5e5e5;
		color: #333;
		font-size: 17px;
	}
</style>
