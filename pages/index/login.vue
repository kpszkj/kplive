<template>
	<view>
		<view>
			<!--  这里是状态栏 -->&nbsp;
		</view>
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">

			<view class="uni-form-item uni-column">
				<view class="title">账号</view>
				<input class="uni-input" type="text" :value="account" @input="valueChange" placeholder="请输入账号" />
			</view>

			<view class="uni-form-item uni-column">
				<view class="title">密码</view>
				<input class="uni-input"  :value="pwd" @input="valuepwd" password type="text" placeholder="请输入密码" />
			</view>
			
			  <button class="mini-btn" @click="logoin" type="warn" size="mini">登录</button>
			  <uni-popup ref="popup" type="center">账号或密码不能为空</uni-popup>
			  
			  <uni-popup ref="popup1" type="center">账号或密码有误请重新登陆</uni-popup>
			  <!-- <uni-popup ref="popup2" type="center">此账号已在其他设备登陆</uni-popup> -->


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
				//账号
				account:'',
				//密码
				pwd:'',
				//设备id
				facility:0,
				//登陆成功失败状态
				succ:false,
				//设备识别码
				code:0,
				
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log('shijian:' + option.iTime);
			console.log('srcUrl:' + option.srcUrl); //打印出上个页面传递的参数。
		},
		onReady() {
			this.Info();
		},
		methods: {
			valueChange(event){
				this.account=event.target.value;
			},
			valuepwd(event){
				this.pwd=event.target.value;
			},
			logoin(){
				if(this.account=='' || this.pwd==''){
					this.$refs.popup.open()
				}else{
					uni.request({
					    url: 'http://192.168.0.110:8080/kp/luser/login.do',
					    data: {
							dFk: this.facility,
							username:this.account,
							password:this.pwd
					    },
					    header: {
					       // 'custom-header': 'hello' //自定义请求头信息
					    },
						method: 'POST',
					    success: (res) => {
					        console.log('返回内容'+res.data.success);
							this.succ=res.data.success
							
							this.code=res.data.message;
							console.log('设备识别码'+this.code);
							
							//将data存储在本地缓存中指定的key中
							try {
							    uni.setStorageSync('keya', this.code);
							} catch (e) {
							    console.log('err'+e)
							}
							//从本地缓存中同步获取指定 key 对应的内容
							try {
							    const value = uni.getStorageSync('keya');
							    if (value) {
							        console.log(value);
							    }
							} catch (e) {
							    console.log('err'+e)
							}
							//异步获取当前 storage 的相关信息
							uni.getStorageInfo({
							    success: function (res) {
							        console.log(res.keys);
							        console.log(res.currentSize);
							        console.log(res.limitSize);
							    }
							});
							
							if(this.succ==true){
								// if(this.code==this.code){
									console.log('设备识别码'+this.code)
									uni.reLaunch({
										url: "/pages/index/index"
									});
								// }else{
								// 	this.$refs.popup2.open()
								// }
								
							}else{
								this.$refs.popup1.open()
							}
							
					    },
						fail: (res) => {
						    console.log(res.data);
						},
					});
					
					
					
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
						console.log(res.deviceId)
						//设备id
						that.facility = res.deviceId;
						that.statusBarHeight = res.statusBarHeight;
						that.isHeight = res.screenHeight - res.statusBarHeight - 2;
					}
				});
			},
		}
	}
</script>

<style>
	.screen {}
</style>
