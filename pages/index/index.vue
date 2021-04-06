<template>
	<view class="container">
		<view class="intro">鲲鹏云播是鲲鹏数字科技有限公司针对文化云平台直播平台需求推出的一款直播软件，用户使用该软件可一键开启平台直播，极为便捷</view>
		<text class="intro">详见：</text>
		<uni-link :href="href" :text="href"></uni-link>
		<navigator url="/pages/kplive/kplive" hover-class="navigator-hover"
			style="margin-top: 20%;width: 50%;margin-left: 25%;">
			<button type="default">体验直播</button>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				href: 'http://kpszkj.cn'
			}
		},
		methods: {

		}
	}
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>
