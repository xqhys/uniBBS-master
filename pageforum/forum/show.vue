<template>
	<view>
		<view class="cu-card dynamic" :class="isCard?'no-card':''">
			<view class="cu-item shadow">
				<view class="solid-bottom text-xl padding">
					<text class="text-black text-bold">{{pageData.data.title}}</text>
				</view>
				
				<view class="cu-list menu-avatar">
					<view class="cu-item">
						<view class="cu-avatar round lg" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"></view>
						<view class="content flex-sub">
							<view>{{pageData.author.nickname}}</view>
							<view class="text-gray text-sm flex justify-between">
								{{pageData.data.timeago}}
							</view>
						</view>
					</view>
				</view>
				
				<view class="text-content">
					{{pageData.data.content}}
				</view>

				<view class="d-content">
					<image mode="widthFix" v-for="(item,key) in pageData.imgslist" :key="key" :src="item"></image> 
				</view>
				
				<view class="text-gray text-sm text-right padding">
					<text class="cuIcon-attentionfill margin-lr-xs"></text> 10
					<text class="cuIcon-appreciatefill margin-lr-xs"></text> 20
				</view>
				
				<view class="cu-bar bg-white solid-bottom" :class="isCard?'margin-top':''">
					<view class="action">
						<text class="cuIcon-titles text-orange "></text> 评论
					</view>
					<view class="action" @click="isShowInput">
						<text class="cuIcon-messagefill margin-lr-xs"></text> 30 我要评论
					</view>
				</view>
				
				<view class="cu-list menu-avatar comment solids-top">
					<view class="cu-item">
						<view class="cu-avatar round" style="background-image:url(https://ossweb-img.qq.com/images/lol/img/champion/Morgana.png);"></view>
						<view class="content">
							<view class="text-grey">莫甘娜</view>
							<view class="text-gray text-content text-df">
								凯尔，你被自己的光芒变的盲目。
							</view>
							<view class="bg-grey padding-sm radius margin-top-sm  text-sm">
								<view class="flex">
									<view>凯尔：</view>
									<view class="flex-sub">妹妹，你在帮他们给黑暗找借口吗?</view>
								</view>
							</view>
							<view class="margin-top-sm flex justify-between">
								<view class="text-gray text-df">2018年12月4日</view>
								<view>
									<text class="cuIcon-appreciatefill text-red"></text>
									<text @click="isShowInput" class="cuIcon-messagefill text-gray margin-left-sm"></text>
								</view>
							</view>
						</view>
					</view>

					<view class="cu-item">
						<view class="cu-avatar round" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"></view>
						<view class="content">
							<view class="text-grey">凯尔</view>
							<view class="text-gray text-content text-df">
								妹妹，如果不是为了飞翔，我们要这翅膀有什么用?
							</view>
							<view class="bg-grey padding-sm radius margin-top-sm  text-sm">
								<view class="flex">
									<view>莫甘娜：</view>
									<view class="flex-sub">如果不能立足于大地，要这双脚又有何用?</view>
								</view>
							</view>
							<view class="margin-top-sm flex justify-between">
								<view class="text-gray text-df">2018年12月4日</view>
								<view>
									<text class="cuIcon-appreciate text-gray"></text> 赞
									<text class="cuIcon-messagefill text-gray margin-left-sm"></text>  回复
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view v-show="showInput">
			<chat-input @send-message="send_comment" :focus="focus" :placeholder="input_placeholder"></chat-input>
		</view>
	</view>
</template>

<script>
	import dUserbox from "../../components/d-userbox.vue";
	import cmform from "../../components/cmform.vue";
	import chatInput from '../../components/chatinput.vue';
	
	var app = require("../../common/common.js");
	var id;
	export default {
		components: {
			dUserbox,
			chatInput,
			cmform
		},
		data: function() {
			return {
				isCard: true,
				showInput: false,
				input_placeholder: '评论', //占位内容
				focus: false, //是否自动聚焦输入框
				is_reply: false, //回复还是评论
				showInput: false, //评论输入框
				pageLoad: false,
				pageHide: false,
				pageData: {},
			}

		},
		onLoad: function(option) {
			id = option.id;
			this.getPage();
			this.addClick();

		},
		onShareAppMessage: function() {

		},
		onPullDownRefresh: function() {
			this.refresh();
		},
		methods: {
			IsCard(e) {
				this.isCard = e.detail.value
			},
			isShowInput() {
				this.showInput = !this.showInput;
				this.focus = true;
			},
			init_input() {
				this.showInput = false;
				this.focus = false;
				this.input_placeholder = '评论';
			},
			send_comment: function(message) {
				console.log(message.content);
				this.init_input();
			},
			refresh: function() {
				this.getPage();
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000)
			},
			addClick: function() {
				uni.request({
					url: app.apiHost + "/module.php?fromapp=wxapp&m=forum&ajax=1&a=addclick&id=" + id,
					success: function(res) {}
				})
			},
			getPage: function() {
				var that = this;
				uni.request({
					url: app.apiHost + "/module.php?fromapp=wxapp&m=forum&ajax=1&a=show&id=" + id,
					data: {
						authcode: this.app.getAuthCode()
					},
					success: function(res) {

						that.pageLoad = true;
						//res.data.data.data.content+='<style>img{max-width:100%;width:220px;height:auto;}</style>';
						res.data.data.data.content = app.html(res.data.data.data.content);
						console.log(res.data.data.data.content);
						that.pageData = res.data.data;

					}
				})
			},
			favToggle: function(id) {
				var that = this;
				uni.request({
					url: that.app.apiHost + "?fromapp=wxapp&m=fav&a=toggle&ajax=1",
					data: {
						objectid: id,
						authcode: that.app.getAuthCode(),
						tablename: "mod_forum"
					},
					success: function(res) {
						if (res.data.error == 1000) {
							that.app.goLogin();
							return false;
						}
						if (res.data.data == 'delete') {
							that.pageData.isfav = false;
						} else {
							that.pageData.isfav = true;
						}

					}
				})
			},
			loveToggle: function(id) {
				var that = this;
				uni.request({
					url: that.app.apiHost + "?m=love&a=toggle&ajax=1",
					data: {

						fromapp: that.app.fromapp(),
						objectid: id,
						authcode: that.app.getAuthCode(),
						tablename: "mod_forum"
					},
					success: function(res) {
						if (res.data.error == 1000) {
							that.app.goLogin();
							return false;
						}
						if (res.data.data == 'delete') {
							that.pageData.islove = false;
						} else {
							that.pageData.islove = true;
						}

					}
				})
			}

		},
	}
</script>

<style>
	.d-content{
		width: 100%;
		text-align: center;
	}
</style>
