<template>
	<view>
		<view class="closeClass">
			<image src="../../static/zhuxiao.png"></image>
		</view>
		
		<view class="closeC1">
			<text style="font-size:21px; color: #333;display:block;">申请账号注销</text>
			<text style="font-size: 14px;color:#555;">当前设备:{{deviceId}}</text>
		</view>
		<view class="closeClass2">
			<text style="font-size:18px; color: #333;display:block;">·友情提示</text>
			<text style="font-size: 14px;color:#555;">一个账户限制一个设备使用，如需在其他设备使用，请先注销当前设备或退出登录。</text>
			<view class="liCss" @click="quit">注销</view>
		</view>

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
.liCss{
		width:100%;
		background-color:#e4475d;
		height: 40px;
		margin-top:215px;
		text-align: center;
		line-height: 40px;
		color:#fff;
		border-radius:65px;
}
.closeClass2{
		position: relative;
		top:108px;
		width:90%;
		/* border: #0062CC solid 1px; */
		margin:0 auto;
}
.closeC1 text{
	text-align: center;
}
.closeC1{
	position: relative;
	top:56px;
	width:90%;
	/* border: #0062CC 1px solid; */
	margin: auto;
}
.closeClass image{
	width:90px;
	height:90px;
	
}
.closeClass{
	width:90px;
	height:90px;
	/* border: #0062CC 1px solid; */
	
	position: relative;
	top:48px;
	margin: auto;
}
</style>
