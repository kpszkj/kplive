<template>
	<view class="pageBackground" :style="{'height':isHeight+'px'}">
		<view class="status_bar">
			<!--  这里是状态栏 -->
			<!-- &nbsp; -->
		</view>
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">
			<image class="logoClss" src="../../static/kpLogo11.png"></image>

			<view class="uni-form-item uni-column uni-column11">
				<view class="title title1">
					<image src="../../static/user.png" mode=""></image>
				</view>
				<input class="uni-input" type="text" :value="username" @input="setUsername" placeholder="请输入用户名" />
			</view>

			<view class="uni-form-item uni-column uni-column11">
				<view class="title title1">
					<image src="../../static/password.png" mode=""></image>
				</view>
				<input class="uni-input" type="text" :value="password" password @input="setPassword"
					placeholder="请输入密码" />
			</view>
			<view class="mini-btn mini-btn11" @click="login" type="warn" size="mini"><text>立即登录</text></view>
			<uni-popup ref="errorTips" class="errorTips" type="center"> {{errorTips}}</uni-popup>
			<!-- <uni-popup ref="popup2" type="center">此账号已在其他设备登陆</uni-popup> -->


		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//屏幕高度
				isHeight: 0,
				//状态栏高度
				statusBarHeight: 0,
				//账户
				username: '',
				//密码
				password: '',
				//设备id
				deviceId: "",
				//登陆成功失败状态
				success: false,
				//设备识别码
				authorToken: "",
				//失败提示
				errorTips: ""
			}
		},
		onReady() {
			this.info();
		},
		methods: {
			setUsername(event) {
				this.username = event.target.value
			},
			setPassword(event) {
				this.password = event.target.value
			},
			info: function() {
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						//console.log(res.deviceId)
						//设备id
						that.deviceId = res.deviceId;
						that.statusBarHeight = res.statusBarHeight;
						// that.isHeight = res.screenHeight - res.statusBarHeight - 2
						that.isHeight = res.screenHeight
					}
				});
				//console.log(this.isHeight)
			},
			login: function() {
				if (this.username == '' || this.password == '') {
					this.errorTips = "账户密码不能为空";
					this.$refs.errorTips.open()
				} else {
					//console.log(1);
					uni.request({
						url: 'http://www.kpszkj.cn/luser/login.do',
						data: {
							dFk: this.deviceId,
							username: this.username,
							password: this.password
						},
						header: {
							'custom-header': 'hello' //自定义请求头信息
						},
						method: 'POST',
						success: (res) => {
							//console.log(res);
							this.success = res.data.success;
							if (this.success == true) {
								this.authorToken = res.data.message;
								//console.log('校验码' + this.authorToken)
								//将data存储在本地缓存中指定的key中
								try {
									uni.setStorageSync('authorToken', this.authorToken);
									uni.setStorageSync('deviceId', this.deviceId)
								} catch (e) {
									console.log('err' + e)
								}
								//从本地缓存中同步获取指定 key 对应的内容
								try {
									const authorToken = uni.getStorageSync('authorToken');
									if (authorToken) {
										//console.log(authorToken);
									}
								} catch (e) {
									console.log('err' + e);
								}
								uni.reLaunch({
									url: "/pages/index/index"
								});

							} else {
								this.errorTips = res.data.message;
								this.$refs.errorTips.open()
							}

						},
						fail: (res) => {
							console.log(res);
						},
					});



				}
			}
		}
	}
</script>

<style>
	.errorTips {
		color: #e4475d;
		font-size: 18px;
	}

	.mini-btn11 text {
		color: #DCDFE6;

		/* line-height: 40px; */

	}

	.mini-btn11 {
		width: 82%;
		background-color: #e4475d;
		height: 40px;
		margin-top: 150px;
		text-align: center;
		color: #DCDFE6;
		line-height: 40px;
		border-radius: 65px;
		margin-left: 8%;
	}

	.title1 {
		/* display: flex; */
	}

	.title1 image {
		/* border: #0062CC 1px solid; */
		float: left;
		margin-left: 25px;
		width: 20px;
		height: 25px;
	}

	.uni-column11 {
		width: 80%;
		/* height:46px; */
		border-radius: 65px;
		background-color: rgba(220, 220, 220, 0.3);
		text-indent: 1em;
		opacity: 0.7;
		/* text-align:center; */
		padding: 14px 0px 14px 0px;
		color: #F7F7F7;
		margin: 30px auto;
	}

	.logoClss {
		width: 125px;
		height: 125px;
		display: flex;
		margin: 90px auto;
	}

	.pageBackground {
		background-image: url(../../static/login_bg.jpg);
		/* border: red 3px solid; */
		/* background-size: 100% 100%; */
	}

	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
	}

	.screen {
		/* border: #007AFF 3px solid; */
	}
</style>
