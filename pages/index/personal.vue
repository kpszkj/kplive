<template>
	<view class="mycss1" :style="{'height':screenHeight+'px'}">
		<view class="status_bar">
			<!--  这里是状态栏 -->
			<!-- &nbsp; -->
		</view>
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">
			<view class="mycss">我的</view>
			<view class="bgClass">
				<image class="image2" :src="luser.headPortrait"></image>
				<text class="userName">{{luser.nickName}}</text>
				<text class="userId">ID:whlive</text>
			</view>
			<view class="li">
				
				<text class="liCss" @click="beginDive">
					<image class="iconClass" src="../../static/livePic.png"></image>
					开始直播
					<text class="rightClass">〉</text>
				</text>
				
				<navigator url="/pages/personal/myProfile">
					<text class="liCss">
						<image class="iconClass" src="../../static/livePic (3).png"></image>
						个人资料
						<text class="rightClass">〉</text>
					</text>
				</navigator>
				
				<navigator url="/pages/personal/liveSettings">
					<text class="liCss">
						<image class="iconClass" src="../../static/livePic (1).png"></image>
						直播设置
						<text class="rightClass">〉</text>
					</text>
				</navigator>
				
				<navigator url="/pages/personal/myDevice">
					<text class="liCss">
						<image class="iconClass" src="../../static/livePic (2).png"></image>
						设备管理
						<text class="rightClass">〉</text>
					</text>
				</navigator>
				
				<navigator url="/pages/personal/aboutUs">
					<text class="liCss">
						<image class="iconClass" src="../../static/livePic (4).png"></image>
						关于我们
						<text class="rightClass">〉</text>
					</text>
				</navigator>
				
				<navigator url="/pages/personal/privacyAgreement">
					<text class="liCss">
						<image class="iconClass" src="../../static/livePic (5).png"></image>
						隐私协议
						<text class="rightClass">〉</text>
					</text>
				</navigator>
				
				<text class="liCss" @click="quit">
					<image class="iconClass" src="../../static/livePic (6).png"></image>
					退出登录
					<text class="rightClass">〉</text>
				</text>
					
			</view>

		</view>

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
				console.log('开始直播')
				uni.switchTab({
					url: '/pages/index/index'
				});
			},
			quit: function() {

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
			},
		}
	}
</script>

<style>
	.rightClass{
		float: right;
		margin-right:38px;
		color:#999;
		font-weight: bold;
		font-size:15px;
	}
	.bgClass{
		background-image: url(../../static/top-banner2.png);
		/* border: #0062CC 1px solid; */
		width: 100%;
		height:170px;
		background-size: 100% 100%;
	}
	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
	}

	.mycss1 {
		/* border:#4CD964 3px solid; */
		
	}
	.iconClass{
		width:18px;
		height: 17px;
		margin-right:10px;
	}

	.mycss {
		font-size:18px;
		width:100%;
		text-align: center;
		text-align: center;
		position: fixed;
		color: #fff;
		padding:5px 5px;
		/* top: 50px; */
		/* justify-content: center; */
	}

	.screen {
		background-color:rgb(225, 225, 225);
		/* border: blue 3px solid; */
	}

	.image2 {
		position: fixed;
		top:100px;
		left:45px;
		border: 1px solid #ffffff;
		width: 80px;
		height: 80px;
		border-radius: 50%;
	}
	.userId{
		position: fixed;
		left:138px;
		color: #777;
		font-size:18px;
		top:138px;
	}
	.userName {
		position: fixed;
		left:138px;
		color: #555;
		font-size:20px;
		font-weight:bold;
		top:112px;
	}

	.li {
		/* border: blue 3px solid; */
		/* position: fixed;
		top:180px; */
		padding-top:30px;
	}

	.liCss {
		/* border: red 1px solid; */
		padding: 15px 20px;
		background:#fff;
		width: 100%;
		display: block;
		/* margin-bottom:0px; */
		border-bottom: 1px solid #e5e5e5;
		color: #333;
		font-size: 17px;
	}
</style>
