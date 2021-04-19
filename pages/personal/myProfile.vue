<template>
	<view>
		<text>{{luser.nickName}}</text>
		<view>{{luser.username}}</view>
		<view>{{luser.sex}}</view>
		<view>{{luser.username}}</view>
		<view>{{luser.lastLoginTime}}</view>
		<text>{{luser.nickName}}</text>
		<view>{{luser.username}}</view>
		<view>{{luser.sex}}</view>
		<view>{{luser.username}}</view>
		<view>{{luser.lastLoginTime}}</view>
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

</style>
