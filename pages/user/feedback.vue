<template>
	<view>
		<view class="bg-white padding margin" @click="phone">
			<image src="../../static/tabs/phone.png"  mode="aspectFill" class="image-icon"></image>
			13265933478
		</view>
		<view class="bg-white padding margin">
			<image src="../../static/tabs/weixin.png"  mode="aspectFill" class="image-icon"></image>
			13265933478
		</view>
		
		<view class="bg-white padding margin">
			<image src="../../static/tabs/kuku.png"  mode="aspectFill" class="image-icon"></image>
			13265933478
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad: function(option) {
			
		},
		methods: {
			phone() {
				uni.makePhoneCall({
				    phoneNumber: '13265933478'
				});
			}
		}
	}
</script>

<style>
	.padding {
		flex: 1;
		font-size: 45px;
		padding: 50px !important;
		border-radius: 20px 20px 20px 20px;
	}
	
	.padding>text{
		color: #E61A1A;
		font-size: 110px;
	}
	
	.image-icon {
		width: 100px;
		height: 100px;
	}
</style>