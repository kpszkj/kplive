<template>
	<view class="over" :style="{'height':screenHeight+'px'}">
		<view class="centerClass">

			<!-- <image class="srcUrl" src="../../static/logo.png"></image><br /> -->
			<image class="srcUrl" :src="luser.headPortrait"></image><br />
			<text class="userName">{{luser.userName}}</text><br />
			<text class="liveOverClass">直播已结束</text><br />
			<text class="duration">直播时长：{{duration}}</text><br />
			<hr style="margin: 10px 0 ;" />
			<text class="liveNum">{{views}}<br />观看人数</text><br />
			<!-- <text class="liveNum1">直播时长：{{duration}}</text> -->
			<text class="indexClass" @click="returnIndex">返回首页</text>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//时间
				duration: '',
				//登录用户
				luser: {},
				//人数
				views: 19,
				//高度
				screenHeight: 0

			}
		},
		onLoad: function(option) {
			//console.log("liveOver");
			//console.log('duration:' + option);
			//option为object类型，会序列化上个页面传递的参数
			//console.log('duration:' + option.duration);
			//打印出上个页面传递的参数。
			this.duration = option.duration;
		},
		onReady: function() {
			this.views = Math.floor((Math.random() + 1) * 13) * 19;
			try {
				const luser = uni.getStorageSync("luser");
				if (luser) {
					//console.log(luser);
					this.luser = luser;
				}
			} catch (e) {
				console.error(e);
			}
			var that = this;
			setTimeout(function() {
				that.info()
			}, 200)
		},
		methods: {
			returnIndex: function() {
				uni.reLaunch({
					url: "/pages/index/index"
				});
			},
			info: function() {
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						that.statusBarHeight = res.statusBarHeight;
						that.screenHeight = res.screenHeight;
					}
				});
				//console.log(this.screenHeight);
			},
		}
	}
</script>

<style>
	.over {
		background-image: url(../../static/body.png);
	}


	.centerClass {
		width: 70%;
		margin: auto;
		text-align: center;
		margin-left: 15%;
		color: #FFFFFF;
	}


	.srcUrl {
		width: 100px;
		height: 100px;
		margin-top: 100px;
		border-radius: 50%;
	}

	.userName {
		font-size: 18px;
		letter-spacing: 0.1em;
	}

	.liveOverClass {
		display: inline-block;
		margin-top: 30px;
		padding: 10px 0px 10px 0px;
		font-size: 20px;
		letter-spacing: 0.1em;
	}

	.duration {
		display: inline-block;
		font-size: 15px;
	}


	.liveNum {
		//display: inline-block;
		font-size: 15px;
		color: #F8F8F8;
	}

	.indexClass {
		display: inline-block;
		margin-top: 150px;
		border-radius: 30px;
		padding: 8px 0;
		width: 180px;
		text-align: center;
		font-size: 20px;
		background-color: rgba(225, 225, 225, 0.2);
	}

	.indexClass:hover {
		background-color: rgba(225, 225, 225, 0.5);
	}
</style>
