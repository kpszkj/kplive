<template>
	<view>
		<text>{{userName}}</text>
		<view>{{name}}</view>
		<view>{{sex}}</view>
		<view>{{intro}}</view>
		<view>{{logoTime}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//用户名
				userName:'千小兔',
				//昵称
				name:'千小兔111',
				//性别
				sex:'女',
				//简介
				intro:'等级考试收到货发的时刻就发货的空间规划科技的复合管',
				//登陆时间
				logoTime:'00:00:00',
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
