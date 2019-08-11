<template>
	<view>
		<view class="main-body">
			<form @submit="formSubmit">
				<view class="input-flex">
					<view class="input-flex-label w60">主题</view>
					<input type="text" name="title" class="input-flex-text" />
				</view>
		
				<view class="textarea-flex">
					<textarea name="content" class="h100 textarea-flex-text"></textarea>
				</view>
				<upimg-box></upimg-box>
				<button formType="submit" class="btn-row-submit">提交</button>
			</form>
		</view>
	</view>
</template>

<script>
	import upimgBox from "../../components/upimgbox.vue";
	var app = require("../../common/common.js");
	var id;
	export default {
		components: {
			upimgBox
		},
		data: function() {
			return {}
		},
		onLoad: function(option) {
			
		},
		onReady: function() {
			uni.setNavigationBarTitle({
				title: "发布帖子",
			})
		},
		methods: {
			formSubmit: function(e) {
				
			}
		},
	}
</script>

<style>

</style>
