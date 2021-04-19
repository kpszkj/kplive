<template>
	<view class="mycss1">
		<view class="status_bar">
			<!--  这里是状态栏 -->
			&nbsp;
		</view>
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">
			<view class="mycss">个人中心</view>
			<view>
				<image class="image2" :src="luser.headPortrait"></image>
				<text class="userName">{{luser.nickName}}</text>
			</view>
			<view class="li">

				<!-- <text class="liCss" @click="beginDive">开始直播</text> -->
				<navigator url="/pages/personal/myProfile">
					<text class="liCss">个人资料</text>
				</navigator>
				<navigator url="/pages/personal/liveSettings">
					<text class="liCss">直播设置</text>
				</navigator>
				<navigator url="/pages/personal/myDevice">
					<text class="liCss">设备管理</text>
				</navigator>
				<navigator url="/pages/personal/aboutUs">
					<text class="liCss">关于我们</text>
				</navigator>
				<navigator url="/pages/personal/privacyAgreement">
					<text class="liCss">隐私协议</text>
				</navigator>
				<text class="liCss" @click="quit">退出登录</text>
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
						that.screenHeight = res.screenHeight - res.statusBarHeight - 2;
					},
				});
			},
			beginDive: function() {
				console.log('开始直播')
				uni.redirectTo({
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
	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
	}

	.mycss1 {
		/* border:#4CD964 3px solid; */
	}

	.mycss {
		font-size: 28px;
		text-align: center;
		position: fixed;
		top: 50px;
		/* justify-content: center; */
	}

	.screen {
		/* border: blue 3px solid; */
	}

	.image2 {
		position: fixed;
		top: 96px;
		right: 130px;
		border: 1px solid #ffffff;
		width: 80px;
		height: 80px;
		border-radius: 50%;
	}

	.userName {
		position: fixed;
		right: 138px;
		font-size: 21px;
		top: 185px;
	}

	.li {
		/* border: blue 3px solid; */
		/* position: fixed;
		top:180px; */
		padding-top: 188px;
	}

	.liCss {
		/* border: red 1px solid; */
		padding: 15px 0px;
		background: rgb(0, 0, 0, 0.2);
		width: 100%;
		display: block;
		/* margin-bottom:0px; */
		border-bottom: 1px solid #E4A951;
		color: #666;
		font-size: 18px;
	}
</style>
