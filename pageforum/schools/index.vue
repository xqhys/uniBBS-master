<template>
	<view>
		<view class="bghead">
			<swiper class="screen-swiper swiper-image radius-imags" :indicator-dots="false" :circular="true" :autoplay="true"
			 interval="5000" duration="500">
				<swiper-item v-for="(item,index) in swiperList" :key="index">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'" class="radius-imags"></image>
					<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
				</swiper-item>
			</swiper>
		</view>

		<view class="view-tabs tabs-border" :class="{'tabs-position':isPosition}">
			<scroll-view scroll-x class="bg-white nav" scroll-with-animation :scroll-left="scrollLeft">
				<view class="cu-item" :class="index==TabCur?'text-green cur':''" v-for="(item,index) in tabs" :key="index"
				 @tap="tabSelect" :data-id="index">
					{{item.name}}
				</view>
			</scroll-view>
		</view>

		<view v-for="(item,fkey) in pageData.list" :key="fkey" class="cu-card dynamic" :class="isCard?'no-card':''">
			<view class="cu-item shadow">
				<view class="cu-list menu-avatar">
					<view class="cu-item">
						<view class="cu-avatar round lg" :style="[{'background-image':'url(' + item.user_head + '.100x100.jpg)'}]"></view>
						<view class="content flex-sub">
							<view class="text-blue">{{item.nickname}}</view>
							<view class="uni-flex uni-row">
								<view class="uni-bg-fea">
									兼职家教
								</view>
							</view>
						</view>
					</view>
				</view>
				<view class="text-content" @click="goForum(item.id)">
					{{item.title}}
				</view>
				<view class="grid flex-sub padding-lr" :class="item.imgslist && item.imgslist.length>1?'col-3 grid-square':'col-1'">
					<view @click="showImage(item.imgslist, imgIndex)" class="bg-img" :class="item.imgslist && item.imgslist.length>1?'':'only-img'" :style="[{'background-image':'url(' + img + '.100x100.jpg)'}]"
					 v-for="(img,imgIndex) in item.imgslist" :key="imgIndex">
					</view>
				</view>
				
				<view class="uni-flex cu-bar uni-row text-gray text-sm text-right padding">
					<view class="text-grey">
						<uni-icon type="location-filled" size="15" color="green" />
						来自[河南师范大学]
					</view>
					<view>
						<text class="cuIcon-appreciatefill margin-lr-xs"></text> 20 赞
						<text class="cuIcon-attentionfill margin-lr-xs"></text> 10 阅读
					</view>
				</view>
			</view>
		</view>
		<forum-footer tab="schools" @apex="apex"></forum-footer>
	</view>
</template>

<script>
	import uniIcon from '@/components/uni-icon/uni-icon.vue'
	import forumFooter from "@/components/forumfooter.vue";
	
	var app = require("@/common/common.js");
	var per_page = 0;
	var isfirst = true;
	var catid = 0;
	var gid = 0;
	export default {
		components: {
			forumFooter,
			uniIcon
		},
		data: function() {
			return {
				isCard: true,
				isPosition: false,
				pageLoad: false,
				pageData: {},
				TabCur: 0,
				scrollLeft: 0,
				systems: uni.getSystemInfoSync(),
				swiperList: [{
					id: 0,
					type: 'image',
					url: '../../static/swiper/3-1.jpg'
				}],
				tabs: [{
					cuIcon: '',
					name: '全部'
				},
				{
					cuIcon: '',
					name: '校园'
				},
				{
					cuIcon: '',
					name: '问答'
				},
				{
					cuIcon: '',
					name: '生活'
				},
				{
					cuIcon: '',
					name: '情感'
				},
				{
					cuIcon: '',
					name: '旅游'
				},
				{
					cuIcon: '',
					name: '广告'
				},
				{
					cuIcon: '',
					name: '失物招领'
				},
				{
					cuIcon: '',
					name: '兼职/家教'
				},
				{
					cuIcon: '',
					name: '二手物品'
				}]
			}

		},
		onReady: function() {
			
		},
		onLoad: function(option) {
			if (option.tabs === 'jzjj') {
				this.TabCur = 8;
			} else if (option.tabs === 'lycx') {
				this.TabCur = 5;
			} else if (option.tabs === 'paotui') {
				this.TabCur = 7;
			}
			this.scrollLeft = (this.TabCur - 1) * 60;
			
			gid = 1;
			catid = option.catid;
			uni.setNavigationBarTitle({
				title: '微校园'
			});
			this.getPage();
		},

		onShow: function() {
			
		},
		onHide: function() {

		},
		onReachBottom: function() {

		},
		onPullDownRefresh: function() {
			uni.stopPullDownRefresh();
		},
		computed: {
			
		},
		watch: {
			
		},
		onPageScroll: function(e) {  
		  let top = e.scrollTop;
		  if (top > 185) {
		  	this.isPosition = true;
		  } else {
		  	this.isPosition = false;
		  }
		},
		methods: {
			IsCard(e) {
				this.isCard = e.detail.value
			},
			getPage: function() {
				var that = this;
				uni.request({
					url: app.apiHost + "/module.php?m=forum&a=list&ajax=1",
					data: {
						gid: gid,
						catid: catid,
						authcode: app.getAuthCode()
					},
					success: function(res) {
						//登录
						if (res.data.error == 1000) {
							uni.navigateTo({
								url: "/pages/login/index",
							})
						} else {
							isfirst = false;
							that.pageLoad = true;
							that.pageData = res.data.data;
							per_page = res.data.data.per_page;
							/* uni.setNavigationBarTitle({
								title: that.pageData.group.title
							}); */
						}

					}
				})
			},
			goForum: function(id) {
				uni.navigateTo({
					url: "/pageforum/forum/comment?id=" + id
				})
			},
			showImage(imgslist, imgIndex) {
				uni.previewImage({
					current: imgslist[imgIndex],
					urls: imgslist
				});
			},
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60;
			},
			apex() {
				uni.pageScrollTo({
					duration: 0,
					scrollTop: 0
				})
			}
		},
	}
</script>

<style>
	.bghead {
		width: 100%;
		height: 355px;
		background-color: #56CC9C;
		border-radius: 0px 0px 190px 190px;
	}
	
	.view-tabs {
		margin-top: 22px;
	}
	
	.tabs-position{
		top: 0px;
		z-index: 9999;
		margin-top: 0px !important;
		position: fixed !important;
	}
</style>