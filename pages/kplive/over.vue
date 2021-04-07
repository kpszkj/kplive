<template>
	<view>
		11111111
		<text>{{liveTime}}</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				liveTime:'',
			}
		},
		 onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		        console.log('shijian:'+option.iTime);
				this.liveTime=option.iTime;
				console.log('srcUrl:'+option.srcUrl);//打印出上个页面传递的参数。
		    },
		methods: {
			
		}
	}
</script>

<style>

</style>
