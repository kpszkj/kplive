<template>
	<view>
		<view class="logoClass">
			<image src="../../static/kpLogo.png"></image>
			<view class="logoC">当前版本：1.0</view>
		</view>
		
		<view class="contentClass">
			<text style="display: block;">鲲鹏live应用简介：</text>
			<text>{{intro}}</text>
		</view>
		
		<text class="phoneCla">官网地址：http://www.kpszkj.cn/</text>
		<text class="phoneCla">联系我们：037157066305</text>
		
		<view class="updateBtn">
			<view class="vv" @click="appUpdate">检查版本更新</view>
			<view class="Copy">Copyright By 郑州鲲鹏数字科技有限公司</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				intro:`鲲鹏live是一款界面清新、操作简洁的直播软件，主要功能是为鲲鹏数字科技有限公司所服务的数字文化云平台提供一站式的直播解决方案。通过本软件，您将可以极其便捷的将本单位的文化直播视频推送到云服务平台与数字文化一体机设备上，实现民众与文化的零距离接触。`
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
	.updateBtn .vv{
		/* width:90%; */
		background-color:#e4475d;
		height: 40px;
		text-align: center;
		color:#fff;
		line-height: 40px;
		border-radius:65px;
		margin: auto;
		/* margin: auto; */
	}
	.updateBtn .Copy{
		margin-top:10px;
		text-align: center;
		/* width:95%; */
		/* border: #0062CC 1px solid; */
		color:#888;
		font-size:14px;
	}
	.updateBtn{
		position: relative;
		top:285px;
		/* border: #0062CC 1px solid; */
		width:85%;
		margin: auto;
	}
	.phoneCla{
		font-size:15px;
		color:#333;
		position: relative;
		top:128px;
		display: block;
		width: 90%;
		margin: auto;
	}
	.contentClass{
		position: relative;
		top:90px;
		width:90%;
		line-height:21px;
		/* border: #0062CC 1px solid; */
		margin: auto;
		color:#666;
		font-size:15px;
	}
.logoClass .logoC{
	font-size:15px;
	color: #666;
	/* display: block; */
	text-align:center;
}
.logoClass image{
	width:98px;
	height:98px;
	display: block;
	margin: auto;
}
.logoClass{
	position: relative;
	top:48px;
	/* height:150px; */
	/* border: #0062CC 1px solid; */
	width:40%;
	margin: auto;
}
</style>
