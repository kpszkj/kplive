<template>
	<view>
		<view class="closeClass">
			<image src="../../static/zhuxiao.png"></image>
		</view>

		<view class="closeC1">
			<text style="font-size:21px; color: #333;display:block;">设备注销</text>
			<text style="font-size: 14px;color:#555;">当前设备:{{deviceId}}</text>
		</view>
		<view class="closeClass2">
			<text style="font-size:18px; color: #333;display:block;">· 友情提示</text><br />
			<text style="font-size: 14px;color:#555;">一个账户限制一个设备使用，如需在其他设备使用，请先注销当前设备。</text>
			<view class="liCss" @click="toQuit">注销</view>
		</view>
		<!-- 注销设备弹窗 -->
		<uni-popup ref="quit" type="dialog" class="popupClass">
			<uni-popup-dialog mode="base" class="popupClass" type="error" title="注销设备"
				content="您即将注销设备,设备注销后您将退出登录并允许在其他设备上使用本账户" :before-close="false" @close="close" @confirm="quit">
			</uni-popup-dialog>
		</uni-popup>

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
			toQuit: function() {
				this.$refs.quit.open();
			},
			close: function(done) {
				//this.$refs.quit.close();
				done();
			},
			quit: function() {
				uni.request({
					url: 'http://www.kpszkj.cn/luser/logout.do',
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
	.liCss {
		display: inline-block;
		width: 80%;
		background-color: #e4475d;
		height: 40px;
		margin: 100px 0 0 10%;
		text-align: center;
		line-height: 40px;
		color: #fff;
		border-radius: 65px;
	}

	.closeClass2 {
		position: relative;
		top: 108px;
		width: 90%;
		/* border: #0062CC solid 1px; */
		margin: 0 auto;
	}

	.closeC1 text {
		text-align: center;
	}

	.closeC1 {
		position: relative;
		top: 56px;
		width: 90%;
		/* border: #0062CC 1px solid; */
		margin: auto;
	}

	.closeClass image {
		width: 90px;
		height: 90px;

	}

	.closeClass {
		width: 90px;
		height: 90px;
		/* border: #0062CC 1px solid; */

		position: relative;
		top: 48px;
		margin: auto;
	}
</style>
