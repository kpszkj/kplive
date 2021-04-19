<template>
	<view>
		设备管理<br />
		<text style="font-size: 12px;">设备信息:{{deviceId}}</text>

		<button class="liCss" @click="quit">注销设备</button>
		<text>提示：一个账户限制一个设备使用，如需在其他设备使用，请先注销当前设备或退出登录</text>



	</view>
</template>

<script>
	export default {
		data() {
			return {
				authorToken: "",
				deviceId: "",
			}
		},
		onReady() {
			//从本地缓存中同步获取指定 key 对应的内容
			try {
				const deviceId = uni.getStorageSync('deviceId');
				const authorToken = uni.getStorageSync('authorToken');
				if (deviceId && authorToken) {
					//console.log(deviceId);
					//console.log(authorToken);
					this.deviceId = deviceId;
					this.authorToken = authorToken;
				}
			} catch (e) {
				console.log('err' + e)
			}
		},
		methods: {
			quit: function() {
				uni.request({
					url: 'http://192.168.0.110:8080/kp/luser/logout.do',
					data: {
						authorToken: this.authorToken
					},
					success: (res) => {
						//console.log(res.data);
						try {
							uni.removeStorageSync('authorToken');
						} catch (e) {
							console.log('err' + e);
						}
						if (res.data.success) {
							uni.reLaunch({
								url: "/pages/index/login"
							});
						}
					}
				});

			}
		}
	}
</script>

<style>

</style>
