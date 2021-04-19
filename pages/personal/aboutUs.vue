<template>
	<view>
		<text>鲲鹏live应用简介：鲲鹏live</text>
		<text>版本号：1.0</text>
		<text>联系我们：037157066305</text>
		<button @click="appUpdate">检查更新</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			appUpdate: function() {
				//console.log('App Launch');
				//#ifdef APP-PLUS  
				var url = "http://192.168.0.110:8080/kp/live/appUpdate.do"; //检查更新地址  
				var appid = plus.runtime.appid;
				var version = plus.runtime.versionCode;
				//console.log(appid);
				//console.log(version);
				var data = { //升级检测数据  
					"appid": plus.runtime.appid,
					"version": plus.runtime.version
				};
				uni.request({
					url: url,
					data: {},
					method: "GET",
					success: (res) => {
						if (res.data.appid == appid && parseInt(res.data.Android.version) > version) {
							//console.log(res.data);
							var appUrl = res.data.Android.url;
							uni.showModal({ //提醒用户更新  
								title: "更新提示",
								content: res.data.Android.note,
								cancelText: "暂不更新",
								confirmText: "立即更新",
								success: (res) => {
									if (res.confirm) {
										//console.log('用户点击确定');
										this.update(appUrl);
									} else if (res.cancel) {
										//console.log('用户点击取消');
									}
									/* if (res.confirm) {
										plus.runtime.openURL(res.data.url);
									} */
								}
							})
						} else {
							uni.showToast({
								title: "已是最新版本",
								duration: 2000
							});
						}
					},
					error: (res) => {
						console.log(res.data);
					}
				})
				//#endif  
			},
			update: function(url) {
				uni.showLoading({
					title: '下载中',
					mask: true
				});
				var dtask = plus.downloader.createDownload(url, {}, function(d, status) {
					// 下载完成
					if (status == 200) {
						uni.hideLoading();
						/* var num = dtask.totalSize / 1024 / 1024;
								
										var num1 = parseFloat(num).toFixed(2)
								
										alert(num1); */
						console.log(d.filename);
						uni.showToast({
							title: "下载完成即将安装",
							duration: 2000,
							mask: true
						});
						//layer.msg("下载完成" + d.filename + "即将安装！");
						setTimeout(function() {
							plus.runtime.install(d.filename);
						}, 2000)
					} else {
						console.log("Download failed: " + status);
					}
				});
				//dtask.addEventListener("statechanged", onStateChanged, false);
				dtask.start();
			}
		}
	}
</script>

<style>

</style>
