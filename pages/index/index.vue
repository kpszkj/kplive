<template>
	<view class="container">
		<view :style="{'height':statusBarHeight+'px'}">
			<!--  这里是状态栏 -->&nbsp;
		</view>
		<!-- 页面内容 -->
		<!-- :style="{'height':isHeight+'px'}" -->
		<view class="screen">
			<!-- 分类列表 -->
			<view class="uni-list">
				<view class="uni-list-cell">
					<view class="uni-list-cell-db">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<view class="uni-input">{{array[index]}}</view>
						</picker>
					</view>
				</view>
			</view>
			<!-- 选择封面图 -->
			<button class="mini-btn" type="default" @click="cover" size="mini">选择封面</button>
			<!-- 封面弹窗 -->
			<uni-popup ref="popup" type="center">
				<!-- <form @submit="formSubmit" @reset="formReset"> -->
				<view class="uni-padding-wrap uni-common-mt">
					<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="120">
						<image v-for="(img,index) in imgUrl" id="demo1" @click="setImg(img,index)"
							class="scroll-view-item_H" :src="img"></image>
					</scroll-view>
				</view>
				<!-- </form> -->
			</uni-popup>
			<image :src="isImg"></image>
			<!-- 填写标题 -->
			<view class="uni-form-item uni-column">
				<!--  focus 点击页面光标定位 -->
				<input class="uni-input" @blur="onInput" v-model="inputValue" placeholder="填写直播标题" />
			</view>

			<picker @change="screenChange" :value="screen" :range="screening">
				<view class="uni-input">{{screening[screen]}}</view>
			</picker>

			<!-- <uni-link :href="href" :text="href"></uni-link> -->
			<!--  url="/pages/kplive/kplive" -->
				<button @click="affirm" type="default">确认信息{{text}}</button>
				<!--  v-if="code ==0" -->
				<!-- <uni-popup ref="popup1" type="center">
					<text class="context1">{{context1}}</text>
					<button class="mini-btn" type="default" @click="consent1">不同意</button>
					<button class="mini-btn" type="default" @click="consent2">同意</button>
				</uni-popup> -->
				
				<view class="popUp" v-if="isShow">
					<view class="popUp1">
						<text class="context1">{{context1}}</text>
						<button class="mini-btn" type="default" @click="consent1">不同意</button>
						<button class="mini-btn" type="default" @click="consent2">同意</button>
					</view>
				</view>

		</view>

	</view>
</template>
<script>
	export default {
		data() {
			return {
				href: 'http://kpszkj.cn',
				text: 'uni.request',
				//屏幕高度
				isHeight: 0,
				//状态栏高度
				statusBarHeight: 0,
				//title: 'picker',
				array: ['古典音乐', '艺术展览', '文化典故'],
				//分类下标
				index: 0,
				//图片路径
				imgUrl: ['http://images.kpszkj.cn/images//6/train/img/2021_01_25/1353507604941676544.jpg',
					'https://img2.baidu.com/it/u=2442148663,2015519237&fm=26&fmt=auto&gp=0.jpg'
				],
				//图片下标
				imgIndex: 0,
				//获取的图片封面
				isImg: '',
				scrollTop: 0,
				old: {
					scrollTop: 0
				},
				//直播标题
				inputValue: '',
				//协议状态
				privacy: 0,
				//直播方向
				screening: ['竖屏直播', '横屏直播'],
				//屏幕方向 0竖屏 1横屏
				screen: 0,
				//设备识别码
				code:0,
				//隐私协议
				context1:'',
				isShow:true,
			}
		},
		onReady() {
			
			//从本地缓存中同步获取指定 key 对应的内容
			try {
			    const value = uni.getStorageSync('keya');
			    if (value) {
			        console.log(value);
					this.code=value;
			    }
			} catch (e) {
			    console.log('err'+e)
			}
			
			
			//从本地缓存中同步获取指定 key 对应的内容
			try {
			    const value = uni.getStorageSync('context');
			    if (value) {
			        // console.log(value);
					this.context1=value;
			    }
			} catch (e) {
			    console.log('err'+e)
			}
			
			//this.$refs.popup1.open() //弹窗
			this.myGetRequest();
			this.Info();
		},
		methods: {
			// priv(){
			// 	this.privacy=1;
			// },
			consent1() {
				if (this.privacy == 0) {
					console.log('隐 私' + this.privacy)
					plus.runtime.quit;
				}
			},
			consent2() {
				this.privacy = 1;
				if (this.privacy == 1) {
					this.isShow=false;
					// this.$refs.popup1.close();
					console.log('隐私状态' + this.privacy)
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
					}
				});
				console.log('gaodu11' + this.isHeight)
			},
			bindPickerChange: function(e) {
				console.log('携带值为', e.target.value)
				this.index = e.target.value
			},
			screenChange: function(e) {
				console.log('携带值为', e.target.value)
				this.screen = e.target.value
			},
			myGetRequest: function() {
				uni.request({
					url: 'http://cim.hjxwhy.cn/info/appUpdate.do',
					data: {},
					header: {
						'custom-header': 'hello' //自定义请求头信息
					},
					success: (res) => {
						// console.log('222res.data222' + res.data);
						this.text = res.data.Android.version;
						// console.log('22211222' + this.text);
					}
				});
			},
			cover() {
				this.$refs.popup.open()
			},
			// formSubmit: function(e) {
			//                 console.log('form发生了submit事件' + JSON.stringify(e.detail.value))
			//                 var formdata = e.detail.value
			//                 uni.showModal({
			//                     content: '表单数据内容：' + JSON.stringify(formdata),
			//                     showCancel: false
			//                 });
			//             },
			// formReset: function(e) {
			//                 console.log('清空数据')
			// },
			scroll: function(e) {
				console.log(e)
				this.old.scrollTop = e.detail.scrollTop
			},
			setImg(img, index) {
				//获取图片下标
				this.imgIndex = index;
				console.log('图片下标' + this.imgIndex)
				//关闭弹窗
				this.$refs.popup.close();
				//图片地址
				this.isImg = img;
				console.log('图片下标' + this.isImg)
			},
			onInput(event) {
				//this.inputValue = event.target.value
				console.log(this.inputValue)
			},
			affirm() {
				if (this.screen == 0) {
					console.log('竖屏' + this.screen)
					uni.navigateTo({
						url: "/pages/kplive/kplive"
					});
				} else if (this.screen == 1) {
					console.log('横屏' + this.screen)
					uni.navigateTo({
						url: "/pages/kplive/slive"
					});
				}

				/* uni.request({//向后台请求数据
					url: 'http://cim.hjxwhy.cn/info/appUpdate.do',
					data: {
						inputValue:'inputValue'
					},
					header: {
						'custom-header': 'hello' //自定义请求头信息
					},
					success: (res) => {
						// console.log('222res.data222' + res.data);
						this.text = res.data.Android.version;
						// console.log('22211222' + this.text);
					}
				}); */
			}

		}
	}
</script>

<style>
	.uni-list-cell-db {
		/* position: fixed;
		top: 100px; */
	}
.context1{
	/* height: 1000px; */
	/* overflow-y: auto; */
}
	.scroll-view-item_H {
		width: 85px;
		height: 85px;
		border: #007AFF solid 2px;
		margin-right: 10px;
		display: inline-block;
	}
	.popUp{
		position:absolute;
		right:0;
		left:0;
		top:0px;
		bottom:0;
		background: #fff;
		/* border: #000000 2px solid; */
		/* display:flex; */
		/* align-items: auto; */
		/* margin: 0 auto; */
		/* width:100%; */
		
	}
	.popUp1{
		margin:50px auto;
		/* align-items: auto; */
		width:83%;
		border: #000000 2px solid;
	}

	.screen {
		/* border: #007AFF solid 2px; */
	}
</style>
