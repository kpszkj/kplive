<template>
	<view class="mycss1">
		<view :style="{'height':statusBarHeight+'px'}">
		</view>
		 <!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">
			<view class="mycss">个人中心</view>
			<view>
			<image class="image2" :src="srcUrl"></image>
			<text class="userName">{{userName}}</text>
			</view>
			<view class="li">
			
				<text class="liCss" @click="beginDive">开始直播</text>
				
			<navigator url="/pages/kplive/liveSet">
				<text class="liCss">直播设置</text>
			</navigator>
			<navigator url="/pages/kplive/manager">
				<text class="liCss">设备管理</text>
			</navigator>
			<navigator url="/pages/kplive/myData">
				<text class="liCss">个人资料</text>
			</navigator>
			<navigator url="/pages/kplive/aboutMe">
				<text class="liCss">关于我们</text>
			</navigator>
			<navigator url="/pages/kplive/privacy">
				<text class="liCss">隐私协议</text>
			</navigator>
				<text class="liCss" @click="quit">退出登录</text>
			</view>
			
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
				//图片路径
				srcUrl: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/6acec660-4f31-11eb-a16f-5b3e54966275.jpg',
				userName:'千小兔',
			}
		},
		onReady() {
			this.Info();
		},
		methods: {
			beginDive(){
				console.log('开始直播')
				uni.reLaunch({
				    url: '/pages/index/index'
				});
			},
			quit(){
				
				uni.request({
				    url: '', 
				    data: {
				        facility: 'facility',
						account:'account',
				    },
				    success: (res) => {
				        console.log(res.data);
				        
				    }
				});
				uni.redirectTo({
					url: "/pages/index/login?iTime="+this.iTime+"&srcUrl="+this.srcUrl
				});
				
				
				//从本地缓存中同步移除指定 key
				try {
				    uni.removeStorageSync('keya');
				} catch (e) {
				    console.log('err'+e)
				}
				//清理本地数据缓存。
				uni.clearStorage();
				//同步清理本地数据缓存
				try {
				    uni.clearStorageSync();
				} catch (e) {
				    // error
				}
				
			},
			Info() {
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						// console.log(res.model);
						// console.log(res.pixelRatio);
						// console.log(res.windowWidth);
						// console.log(res.windowHeight);
						// console.log(res.language);
						// console.log(res.version);
						// console.log(res.platform);
						that.statusBarHeight = res.statusBarHeight;
						that.isHeight = res.screenHeight - res.statusBarHeight - 2;
					},
				});
			},
		}
	}
</script>

<style>
	.mycss1{
		/* border:#4CD964 3px solid; */
	}
	.mycss{
		font-size:28px;
		text-align: center;
		position: fixed;
		top: 50px;
		/* justify-content: center; */
	}
	.screen{
		/* border: blue 3px solid; */
	}
	.image2 {
		position: fixed;
		top: 96px;
		right:130px;
		border: 1px solid #ffffff;
		width:80px;
		height:80px;
		border-radius: 50%;
	}
	.userName{
		position: fixed;
		right:138px;
		font-size: 21px;
		top:185px;
	}
	.li{
		/* border: blue 3px solid; */
		/* position: fixed;
		top:180px; */
		padding-top: 188px;
	}
	.liCss{
		/* border: red 1px solid; */
		padding:15px 0px;
		background:rgb(0,0,0,0.2);
		width:100%;
		display:block;
		/* margin-bottom:0px; */
		border-bottom:1px solid #E4A951;
		color: #666;
		font-size:18px;
	}
</style>
