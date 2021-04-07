<template>
	<view>

		<view>
			<switch @change="switch2Change" />是否推送直播到云平台
		</view>

		<!-- 直播清晰度 （标清 高清）SD（标清）, HD（高清）, FHD（超清）。 -->

		<label>
			<checkbox value="cb" name="a" checked="true" />标清
			<checkbox value="cb" name="a" />高清
		</label>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//清晰度
				clearly: 0,
			}
		},
		methods: {
			switch2Change: function(e) {
				console.log('switch2 发生事件', e.target.value)
			}
		}
	}
</script>

<style>

</style>
