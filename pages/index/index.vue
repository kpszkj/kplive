<template>
	<view class="container">
		<view class="status_bar">
			<!--  这里是状态栏 -->
			&nbsp;
		</view>
		<!-- 页面内容 -->
		<!-- :style="{'height':isHeight+'px'}" -->
		<view>
			<!-- 分类列表 -->
			<!-- {{liveCategoryList}} -->
			<view v-if="liveCategoryList.length>0">
				<view>
					<view>
						<text>类别：</text>
						<picker style="display: inline-block;" mode="selector" :range="liveCategoryList"
							range-key="name" :value="caIndex" @change="bindPickerChange">
							<view>{{liveCategoryList[caIndex].name}}</view>
						</picker>
					</view>
				</view>
			</view>
			<!-- 选择封面图 -->
			<text>封面：</text><button class="mini-btn" type="default" @click="cover" size="mini">选择封面</button>
			<!-- 封面弹窗 -->
			<uni-popup ref="popup" type="center">
				<!-- <form @submit="formSubmit" @reset="formReset"> -->
				<view class="uni-padding-wrap uni-common-mt">
					<view class="scroll-view_H">
						<image v-for="(img,index) in imgList" @click="setImg(img,index)" class="scroll-view-item_H"
							:src="img">
						</image>
					</view>
				</view>
				<!-- </form> -->
			</uni-popup>
			<image :src="img"></image>
			<!-- 填写标题 -->
			<view class="uni-form-item uni-column">
				<!--  focus 点击页面光标定位 -->
				<text>标题：</text><input class="uni-input" @blur="onInput" v-model="title" placeholder="填写直播标题" />
			</view>

			<picker @change="orientationChange" :value="orientation" :range="orientationList">
				<view class="uni-input">{{orientationList[orientation]}}</view>
			</picker>

			<!-- <uni-link :href="href" :text="href"></uni-link> -->
			<!--  url="/pages/kplive/kplive" -->
			<button @click="ready()" type="default" v-if="this.liveStatus==0">开始直播</button>
			<button @click="ready()" type="default" v-if="this.liveStatus==1">继续直播</button>
			<!--  v-if="code ==0" -->
			<!-- <uni-popup ref="popup1" type="center">
					<text class="context1">{{context1}}</text>
					<button class="mini-btn" type="default" @click="consent1">不同意</button>
					<button class="mini-btn" type="default" @click="consent2">同意</button>
				</uni-popup> -->

			<view class="popUp" v-if="privacy==0">
				<view class="popUp1">
					<text class="context">
						用户隐私服务协议是本App对用户隐私保护的许诺，请您务必仔细阅读《<text @click="toPrivacy"
							style="color: #007AFF;">用户隐私服务协议</text>》，以了解我们关于管理您个人信息的情况。
						点击同意继续使用，不同意则退出App。
					</text>
					<button class="mini-btn" type="default" @click="quit"
						style="width: 50%;display: inline-block;">退出</button>
					<button class="mini-btn" type="default" @click="agree"
						style="width: 50%;display: inline-block;">同意</button>
				</view>
			</view>

		</view>

		<!-- 错误提示 -->
		<uni-popup ref="error" type="bottom">
			<uni-popup-message type="error" :message="error" :duration="1000"></uni-popup-message>
		</uni-popup>

	</view>
</template>
<script>
	export default {
		data() {
			return {
				//设备识别码
				authorToken: "",
				//协议状态
				privacy: 0,
				//直播状态
				liveStatus: 0,
				//错误提示
				error: "",
				//直播分类下标
				caIndex: 0,
				//直播分类列表
				liveCategoryList: [],
				//封面列表
				imgList: [
					'http://images.kpszkj.cn/images//6/train/img/2021_01_25/1353507604941676544.jpg',
					'https://img2.baidu.com/it/u=2442148663,2015519237&fm=26&fmt=auto&gp=0.jpg',
					'http://images.kpszkj.cn/images//6/train/img/2021_01_25/1353507604941676544.jpg',
					'https://img2.baidu.com/it/u=2442148663,2015519237&fm=26&fmt=auto&gp=0.jpg',
					'http://images.kpszkj.cn/images//6/train/img/2021_01_25/1353507604941676544.jpg',
					'https://img2.baidu.com/it/u=2442148663,2015519237&fm=26&fmt=auto&gp=0.jpg'
				],
				//用户信息
				luser: {},
				//直播信息
				live: {},
				//直播ID信息
				liveId: 0,
				//图片下标
				imgIndex: 0,
				//获取的图片封面
				img: '',
				//直播标题
				title: '',
				//直播方向
				orientationList: ['竖屏直播', '横屏直播'],
				//屏幕方向 0竖屏 1横屏
				orientation: 0
			}
		},
		onReady() {
			this.init();
		},
		onPullDownRefresh() {
			//console.log('onPullDownRefresh');
			this.init();
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 500);
		},
		onShow() {
			//console.log('onShow');
			//刷新当前页  
			this.init();
		},
		methods: {
			//页面初始化
			init: function() {
				this.liveId = 0;
				this.caIndex = 0;
				this.img = " ";
				this.title = "";
				this.orientation = 0;
				this.liveStatus = 0;
				//判断登陆状态
				//从本地缓存中同步获取authorToken 对应的内容
				try {
					const authorToken = uni.getStorageSync('authorToken');
					if (authorToken) {
						//console.log(authorToken);
						this.authorToken = authorToken;
						const privacy = uni.getStorageSync('privacy');
						if (privacy) {
							//console.log(privacy);
							this.privacy = privacy;
							this.findActiveUser();
							//this.findActiveUser();
						}
					} else {
						this.toLogin();
					}

				} catch (e) {
					/* try {
						uni.removeStorageSync('authorToken');
					} catch (e) {
						// error
					} */
					//uni.setStorageSync('authorToken', "authorToken");
					console.log('err' + e)
				}
			},
			//跳转到登录页
			toLogin: function() {
				uni.redirectTo({
					url: "/pages/index/login"
				});
			},
			//跳转到隐私协议页
			toPrivacy: function() {
				uni.navigateTo({
					url: "/pages/personal/privacyAgreement"
				});
			},
			//同意协议
			agree: function() {
				//this.privacy = 1;
				try {
					uni.setStorageSync('privacy', 1);
					this.privacy = 1;
					this.findActiveUser();
				} catch (e) {
					console.log(e)
				}
			},
			//退出app
			quit: function() {
				// #ifdef APP-PLUS
				plus.runtime.quit();
				// #endif
			},

			//选择直播类别
			bindPickerChange: function(e) {
				//console.log('携带值为', e.target.value)
				this.caIndex = e.target.value;
			},
			//选择直播状态
			orientationChange: function(e) {
				//console.log('携带值为', e.target.value)
				this.orientation = e.target.value
			},


			//弹出封面
			cover: function() {
				this.$refs.popup.open();
			},
			//设置图片路径
			setImg: function(img, index) {
				//获取图片下标
				//this.imgIndex = index;
				//console.log('图片下标' + this.imgIndex)
				//关闭弹窗
				this.$refs.popup.close();
				//图片地址
				this.img = img;
				//console.log('图片下标' + this.isImg)
			},
			//设置标题
			onInput: function(event) {
				this.title = event.target.value
			},


			//查找登陆的用户信息
			findActiveUser: function() {
				uni.request({
					url: 'http://192.168.0.110:8080/kp/luser/findActiveUser.do',
					data: {
						authorToken: this.authorToken,
					},
					header: {
						//自定义请求头信息
						//'custom-header': 'hello'
					},
					success: (res) => {
						this.luser = res.data;
						try {
							uni.setStorageSync('luser', this.luser);
						} catch (e) {
							console.log(e);
						}
						this.findList();
						//console.log('findActiveUser:' + res.data.cFk);
						//this.text = res.data.Android.version;
						// console.log('22211222' + this.text);
					}
				});
			},
			//查找直播类别
			findList: function() {
				uni.request({
					url: 'http://192.168.0.110:8080/kp/liveCategory/findAll.do',
					data: {
						cFk: this.luser.cFk,
					},
					header: {
						//自定义请求头信息
						//'custom-header': 'hello' 
					},
					success: (res) => {
						this.liveCategoryList = res.data;
						this.onLiveReady();
					}
				});
			},
			//查找是否存在正在直播的内容
			onLiveReady: function() {
				uni.request({
					url: 'http://192.168.0.110:8080/kp/live/onReady.do',
					data: {
						authorToken: this.authorToken,
					},
					header: {
						//自定义请求头信息
						//'custom-header': 'hello'
					},
					success: (res) => {
						if (res.data.success) {
							//console.log(res.data);
							if (res.data.message == "") {

								//console.log(1);
							} else {
								this.liveId = res.data.message;
								//console.log(2);
								this.findOne();
							}
						} else {
							//console.log(3);
							this.error = res.data.message;
							this.$refs.error.open();

						}
						//console.log('findActiveUser:' + res.data.cFk);
						//this.text = res.data.Android.version;
						// console.log('22211222' + this.text);
					}
				});
			},
			//查找单个直播
			findOne: function() {
				uni.request({
					url: 'http://192.168.0.110:8080/kp/live/findOne.do',
					data: {
						liveId: this.liveId
					},
					header: {
						//自定义请求头信息
						//'custom-header': 'hello'
					},
					method: "GET",
					success: (res) => {
						//console.log(res.data);
						this.live = res.data;
						this.liveId = this.live.id;
						this.img = this.live.img;
						for (var i = 0; i < this.liveCategoryList.length; i++) {
							if (this.liveCategoryList[i].id == this.live.caFk) {
								this.caIndex = i;
							}
						}
						this.title = this.live.title;
						this.orientation = this.live.orientation;
						this.liveStatus = 1;
					}
				});
			},
			//直播准备
			ready: function() {
				if (this.img == "") {
					this.error = "请选择封面";
					this.$refs.error.open();
					return;
				}
				if (this.title == "") {
					this.error = "标题不能为空";
					this.$refs.error.open();
					return;
				}
				if (this.liveId == 0) {
					uni.request({
						url: 'http://192.168.0.110:8080/kp/live/ready.do',
						data: {
							img: this.img,
							title: this.title,
							url: this.luser.playUrl,
							orientation: this.orientation,
							type: 0,
							managerName: this.luser.nickName,
							caFk: this.liveCategoryList[this.caIndex].id,
							luFk: this.authorToken,
							cuFk: this.luser.cFk
						},
						header: {
							//自定义请求头信息
							//'custom-header': 'hello'
						},
						method: "POST",
						success: (res) => {
							if (res.data.success) {
								this.liveId = res.data.message;
								try {
									uni.setStorageSync('liveId', this.liveId);
									//console.log(this.liveId);
									//console.log("标题：" + this.title);
									if (this.orientation == 0) {
										//console.log('竖屏' + this.orientation)
										uni.navigateTo({
											url: "/pages/kplive/kplive"
										});
									} else if (this.orientation == 1) {
										//console.log('横屏' + this.orientation)
										uni.navigateTo({
											url: "/pages/kplive/hkplive"
										});
									}
								} catch (e) {
									console.log(e);
								}
							} else {
								console.log(this.luser.nickName);
								this.error = res.data.message;
								this.$refs.error.open();
							}
						}
					});
				} else {
					uni.request({
						url: 'http://192.168.0.110:8080/kp/live/ready.do',
						data: {
							id: this.liveId,
							img: this.img,
							title: this.title,
							url: this.luser.playUrl,
							orientation: this.orientation,
							type: 0,
							managerName: this.luser.nickName,
							caFk: this.liveCategoryList[this.caIndex].id,
							luFk: this.authorToken,
							cuFk: this.luser.cFk
						},
						header: {
							//自定义请求头信息
							//'custom-header': 'hello'
						},
						method: "POST",
						success: (res) => {
							if (res.data.success) {
								this.liveId = res.data.message;
								try {
									uni.setStorageSync('liveId', this.liveId);
									//console.log(this.liveId);
									//console.log("标题：" + this.title);
									if (this.orientation == 0) {
										//console.log('竖屏' + this.orientation)
										uni.navigateTo({
											url: "/pages/kplive/kplive"
										});
									} else if (this.orientation == 1) {
										//console.log('横屏' + this.orientation)
										uni.navigateTo({
											url: "/pages/kplive/hkplive"
										});
									}
								} catch (e) {
									console.log(e)
								}
							} else {
								console.log(this.luser.nickName);
								this.error = res.data.message;
								this.$refs.error.open();
							}
						}
					});
				}

			},


			/* info: function() {
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
					}
				});
				console.log('gaodu11' + this.isHeight)
			}, */
			/* scroll: function(e) {
				//console.log(e);
				this.old.scrollTop = e.detail.scrollTop
			}, */

			/* uni.request({//向后台请求数据
				url: 'http://cim.hjxwhy.cn/info/appUpdate.do',
				data: {
					inputValue:'inputValue'
				},
				header: { 
					//自定义请求头信息
					//'custom-header': 'hello'
				},
				success: (res) => {
					// console.log('222res.data222' + res.data);
					this.text = res.data.Android.version;
					// console.log('22211222' + this.text);
				}
			}); */
		}

	}
</script>
<style>
	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
	}

	.uni-list-cell-db {
		/* position: fixed;
		top: 100px; */
	}

	.context {
		/* height: 1000px; */
		/* overflow-y: auto; */
	}

	.scroll-view_H {
		//white-space: nowrap;
		width: 100%;
	}

	.scroll-view-item_H {
		margin-left: 20px;
		display: inline-block;
		width: 150px;
		height: 100px;
		line-height: 100px;
		//text-align: center;
		//font-size: 36rpx;
	}

	.popUp {
		position: absolute;
		right: 0;
		left: 0;
		top: 0px;
		bottom: 0;
		background: #fff;
		/* border: #000000 2px solid; */
		/* display:flex; */
		/* align-items: auto; */
		/* margin: 0 auto; */
		/* width:100%; */

	}

	.popUp1 {
		margin: 50px auto;
		/* align-items: auto; */
		width: 83%;
		border: #000000 2px solid;
	}

	.screen {
		//height: 400px;
		//overflow: hidden;
		//overflow-y: auto;
		/* border: #007AFF solid 2px; */
	}
</style>
