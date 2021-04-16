<template>
	<view>

		<view class="fontClass">
			是否推送到云平台<switch class="rightC" style="transform:scale(0.9)" @change="switch2Change" color="#f3304b" />
		</view>

		<!-- 直播清晰度 （标清 高清）SD（标清）, HD（高清）, FHD（超清）。 -->
		
		<view class="fontClass">
			<text>直播清晰度</text>
			<label class="rightC">
				标清<checkbox style="transform:scale(0.7)" color="#f3304b" value="cb" name="a" checked="true" />&nbsp;
				高清<checkbox style="transform:scale(0.7)" color="#f3304b" value="cb" name="a" />
			</label>
		</view>
		
		
		
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
	.rightC{
		font-size:14px;
		float: right;
		margin-right:15px;
		color: #777;
	}
	.fontClass{
		font-size:16px;
		color: #555555;
		margin:18px 15px 0px 15px;
		padding-bottom:12px;
		border-bottom:1px solid #E1E1E1;
	}

</style>
