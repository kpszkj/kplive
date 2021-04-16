<template>
	<view  class="screen">
		
		<!-- <view class="mycss">我的</view> -->
		<view class="bgClass">
			<image class="image2" :src="luser.headPortrait"></image>
			<text class="userName">{{luser.nickName}}</text>
			<text class="userId">ID:whlive</text>
		</view>
		
		
		<view class="li">
			
			<text class="liCss">
				姓名
				<text class="rightClass">{{luser.nickName}}</text>
			</text>
			
			<text class="liCss">
				性别
				<text class="rightClass">{{luser.sex}}</text>
			</text>
			
			<text class="liCss">
				昵称
				<text class="rightClass">{{luser.username}}</text>
			</text>
			
			<text class="liCss">
				账户状态
				<text class="rightClass">{{luser.sex}}</text>
			</text>
			
			<text class="liCss">
				登陆时间
				<text class="rightClass">{{luser.lastLoginTime}}</text>
			</text>
			
			<text class="liCss">
				宣言
				<text class="rightClass">{{luser.username}}</text>
			</text>
				
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				luser: {}
			}
		},
		onReady() {
			this.Info();
		},
		methods: {
			Info: function() {
				try {
					const luser = uni.getStorageSync("luser");
					if (luser) {
						//console.log(luser);
						this.luser = luser;
					}
				} catch (e) {
					console.error(e);
				}
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						that.screenHeight = res.screenHeight - res.statusBarHeight - 2;
					},
				});
			},
		}
	}
</script>

<style>
	.li {
		/* border: blue 3px solid; */
		/* position: fixed;
		top:180px; */
		padding-top:30px;
	}
	
	.liCss {
		/* border: red 1px solid; */
		padding: 15px 20px;
		background:#fff;
		width: 100%;
		display: block;
		/* margin-bottom:0px; */
		border-bottom: 1px solid #e5e5e5;
		color: #333;
		font-size: 17px;
	}
	.userId{
		position: fixed;
		left:138px;
		color: #777;
		font-size:18px;
		top:108px;
	}
	.userName {
		position: fixed;
		left:138px;
		color: #555;
		font-size:20px;
		font-weight:bold;
		top:85px;
	}
	.image2 {
		position: fixed;
		top:65px;
		left:45px;
		border: 1px solid #ffffff;
		width: 80px;
		height: 80px;
		border-radius: 50%;
	}
	.bgClass{
		background-image: url(../../static/top-banner2.png);
		/* border: #0062CC 1px solid; */
		width: 100%;
		height:170px;
		background-size: 100% 100%;
	}
	.mycss {
		font-size:18px;
		width:100%;
		text-align: center;
		text-align: center;
		position: fixed;
		color: #fff;
		padding:5px 5px;
		/* top: 50px; */
		/* justify-content: center; */
	}
	.rightClass{
		float: right;
		margin-right:38px;
		color:#999;
		font-weight: bold;
		font-size:15px;
	}
	.screen {
		background-color:rgb(225, 225, 225);
		/* border: blue 3px solid; */
	}
</style>
