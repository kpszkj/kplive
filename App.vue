<script>
	export default {
		onLaunch: function() {
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
							}
						})
					}
				},
				error: (res) => {
					console.log(res.data);
				}
			})
			//#endif  
		},
		methods: {
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
						uni.showToast({
							title: "下载完成" + d.filename + "即将安装！",
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
		/* onLaunch: function() {
			console.warn('当前组件仅支持 uni_modules 目录结构 ，请升级 HBuilderX 到 3.1.0 版本以上！')
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		} */
	}
</script>

<style>
	/*每个页面公共css */
</style>
