<template>
	<view>
		<view class="status_bar">
			<!--  这里是状态栏 -->
			&nbsp;
		</view>
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">

			<view class="uni-form-item uni-column">
				<view class="title">账号</view>
				<input class="uni-input" type="text" :value="username" @input="setUsername" placeholder="请输入账号" />
			</view>

			<view class="uni-form-item uni-column">
				<view class="title">密码</view>
				<input class="uni-input" type="text" :value="password" password @input="setPassword"
					placeholder="请输入密码" />
			</view>

			<button class="mini-btn" @click="login" type="warn" size="mini">登录</button>
			<uni-popup ref="errorTips" type="center">{{errorTips}}</uni-popup>
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
						that.isHeight = res.screenHeight - res.statusBarHeight - 2
					}
				});
			},
			login: function() {
				if (this.username == '' || this.password == '') {
					this.errorTips = "账户密码不能为空";
					this.$refs.errorTips.open()
				} else {
					uni.request({
						url: 'http://192.168.0.110:8080/kp/luser/login.do',
						data: {
							dFk: this.deviceId,
							username: this.username,
							password: this.password
						},
						header: {},
						method: 'POST',
						success: (res) => {
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
	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
	}

	.screen {}
</style>
