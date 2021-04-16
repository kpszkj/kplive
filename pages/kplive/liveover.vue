<template>
	<view class="over"  :style="{'height':screenHeight+'px'}">
		<view class="centerClass">
			<image class="srcUrl" src="../../static/logo.png"></image>
			<text class="userName">{{userName}}</text>
			
			<text class="liveOverClass">直播已结束-{{duration}}</text>
			<text class="liveNum">观看人数{{number}}</text>
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
				//头像
				srcUrl:'',
				//用户名
				userName:'卫辉云播',
				//人数
				number:100,
				//高度
				screenHeight:0
				
			}
		},
		onLoad: function(option) {
			console.log("liveOver");
			console.log('duration:' + option);
			//option为object类型，会序列化上个页面传递的参数
			console.log('duration:' + option.duration);
			//打印出上个页面传递的参数。
			this.duration = option.duration;
			// this.srcUrl=
		},
		onReady: function() {
			this.info()
		},
		methods: {
			returnIndex:function () {
				uni.reLaunch({
					url: "/pages/index/index"
				});
			},
			info: function() {
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						that.statusBarHeight = res.statusBarHeight;
						that.screenHeight = res.screenHeight - res.statusBarHeight - 2;
					}
				});
			},
		}
	}
</script>

<style>
	.indexClass{
		position: fixed;
		bottom:180px;
		border-radius:55px;
		padding:8px 0 8px 0;
		width:72%;
		/* border: red 1px solid; */
		margin: auto;
		text-align: center;
		font-size:19px;
		/* letter-spacing:0.1em; */
		color: #F8F8F8;
		background-color: rgba(225,225,225,0.2);
	}
	.liveNum{
		position: fixed;
		top:300px;
		width:72%;
		margin: auto;
		/* padding:10px 0px; */
		text-align: center;
		/* border: red 1px solid; */
		color: #F8F8F8;
		
	}
	.liveOverClass{
		position: fixed;
		top:225px;
		width:72%;
		text-align: center;
		/* border: red 1px solid; */
		margin: auto;
		font-size:20px;
		letter-spacing:0.1em;
		color: #FFFFFF;
		border-bottom:#FFFFFF 1px solid;
		padding:10px 0px 10px 0px;
	}
	.userName{
		font-size:19px;
		letter-spacing:0.1em;
		color: #FFFFFF;
		position: fixed;
		/* border: red 1px solid; */
		/* padding: 2px 2px; */
		width:72%;
		top:185px;
		text-align: center;
		margin: auto;
	}
	.srcUrl{
		width:100px;
		height:100px;
		position: fixed;
		top:80px;
		left:35%;
		/* display: flex; */
		/* margin: auto; */
	}
	.centerClass{
		/* border:  #0062CC 1px solid; */
		width:75%;
		margin: auto;
		height:80%;
	}
	.over{
		background-image: url(../../static/body.png);
		background-size:auto;
	}
</style>
