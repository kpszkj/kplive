<template>
	<view>
		设备管理
		
		<text>设备信息:{{facility}}</text>
		
		<button class="liCss" @click="quit">注销设备</button>
		<text>提示：一个账户限制一个设备使用，如需在其他设备使用，请先注销当前设备或退出登录</text>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				facility:0,
			}
		},
		onReady(){
			//从本地缓存中同步获取指定 key 对应的内容
			try {
			    const value = uni.getStorageSync('keya');
			    if (value) {
			        console.log(value);
					this.facility=value;
			    }
			} catch (e) {
			    console.log('err'+e)
			}
		},
		methods: {
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
				
			}
		}
	}
</script>

<style>

</style>
