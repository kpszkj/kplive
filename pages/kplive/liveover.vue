<template>
	<view>
		<text>{{duration}}</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				duration: '',
			}
		},
		onLoad: function(option) {
			console.log("liveOver");
			console.log('duration:' + option);
			//option为object类型，会序列化上个页面传递的参数
			console.log('duration:' + option.duration);
			//打印出上个页面传递的参数。
			this.duration = option.duration;
		},
		onReady: function() {

		},
		methods: {

		}
	}
</script>

<style>

</style>
