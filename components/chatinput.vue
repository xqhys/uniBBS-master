<template>
	<view class="input-footer">
		<!-- <view class="footer-left">
			<view class="uni-icon uni-icon-mic" @tap="startRecognize"> </view>
		</view> -->
		<view class="footer-center">
			<input class="input-text" type="text" v-model="inputValue" :focus="focus" :placeholder="placeholder"></input>
		</view>
		<view class="footer-right">
			<view id='msg-type' class="send-comment" @tap="sendMessge">发送</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "chat-input",
		data() {
			return {
				inputValue: ''
			}
		},
		props:{
			placeholder: {
				type: String,
				required: true
			},
			focus: {
				type:Boolean,
				required: true
			}
		},
		methods: {
			startRecognize: function () {
				var options = {};
				var that = this;
				options.engine = 'iFly';
				that.inputValue = "";
				plus.speech.startRecognize(options, function (s) {
					console.log(s);
					that.inputValue += s;
				}, function (e) {
					console.log("语音识别失败：" + e.message);
				});
			},
			sendMessge: function () {
				if (!this.inputValue) {
					uni.showModal({
						content:"还没有输入内容哦!",
						showCancel:false
					})
					return;
				}
				var that = this;
				//点击发送按钮时，通知父组件用户输入的内容
				this.$emit('send-message', {
					type: 'text',
					content: that.inputValue
				});
				that.inputValue = '';//清空上次输入的内容
			}
		}
	}
</script>

<style>
	.input-footer {
		width: 100%;
		height: 120upx;
		bottom: 0upx;
		left: 0upx;
		right: 0upx;
		min-height: 120upx;
		display: flex;
		position: fixed;
		z-index: 9999;
		align-items: center;
		overflow: hidden;
		flex-direction: row;
		background-color: #F4F5F6;
		border-top: solid 1px #bbb;
		/* padding: 50px 0px 60px 0px; */
	}
	
	.footer-left {
		width: 80upx;
		height: 80upx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.footer-right {
		width: 120upx;
		height: 90upx;
		font-size: 30px;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #1482D1;
	}
	.footer-center {
		flex: 1;
		padding-left: 20upx;
		height: 90upx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.footer-center .input-text {
		height: 80upx;
		flex: 1;
		background: #fff;
		/* border: solid 1upx #ddd; */
		padding: 10upx !important;
		font-family: verdana !important;
		overflow: hidden;
		border-radius: 15upx;
	}
	.footer-right .send-comment{
		background-color: #007AFF;
		text-align: center;
		line-height: 70upx;
		color: #FFFFFF;
		width: 90upx;
		height: 70upx;
		border-radius: 5px;
		font-size: 30px;
	}
</style>