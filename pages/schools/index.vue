<template>
	<view>
		<view class="bghead">
			<swiper class="screen-swiper swiper-image square-dot radius-imags" :indicator-dots="true" :circular="true" :autoplay="true"
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
					<view class="bg-img" :class="item.imgslist && item.imgslist.length>1?'':'only-img'" :style="[{'background-image':'url(' + img + '.100x100.jpg)'}]"
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
	import uniIcon from '@/components/uni-icon.vue'
	import forumFooter from "../../components/forumfooter.vue";
	
	var app = require("../../common/common.js");
	var per_page = 0;
	var isfirst = true;
	var catid = 0;
	var gid = 0;
	var activeClass = "tabs-border-active";
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
				pageHide: false,
				pageData: {},
				systems: uni.getSystemInfoSync(),
				defaultActive: activeClass,
				TabCur: 0,
				scrollLeft: 0,
				tabsBorder: 0,
				swiperList: [{
					id: 0,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big84000.jpg'
				}, {
					id: 1,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big37006.jpg',
				}, {
					id: 2,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big39000.jpg'
				}, {
					id: 3,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big10001.jpg'
				}, {
					id: 4,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
				}, {
					id: 5,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big21016.jpg'
				}, {
					id: 6,
					type: 'image',
					url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big99008.jpg'
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
			gid = option.gid;
			catid = option.catid;
			uni.setNavigationBarTitle({
				title: '微校园'
			});
			
			this.getPage();
		},

		onShow: function() {
			if (this.pageHide) {
				this.pageHide = false;
				this.getPage();
			}
		},
		onHide: function() {
			this.pageHide = true;
		},
		onReachBottom: function() {
			this.getList();
		},
		onPullDownRefresh: function() {
			this.refresh();
		},
		computed: {
			// 计算属性的 getter
			reversedMessage: function () {
				let self = this;
				let view = uni.createSelectorQuery().in(this).select(".tabs-border");
				if (self.tabsBorder != 0) {
					view.boundingClientRect(data => {
						self.tabsBorder = data.top;
					}).exec();
				}else{
					self.tabsBorder = 1;
				}
				
			  return self.tabsBorder
			}
		},
		watch: {
			reversedMessage: {
				handler: function(newVal, oldVal) {
				},
				deep: true
			}
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
			setCat: function(cid) {
				catid = cid;
				isfirst = true;
				per_page = 0;
				if (catid == 0) {
					this.defaultActive = activeClass;
				} else {
					this.defaultActive = "";
				}
				var catlist = this.pageData.catlist;
				for (var i in catlist) {
					if (catlist[i].catid == catid) {
						catlist[i].isactive = 1;
					} else {
						catlist[i].isactive = 0;
					}
				}
				this.pageData.catlist = catlist;
				this.getList();
			},
			getList: function() {
				var that = this;
				if (!isfirst && per_page == 0) return false;
				uni.request({
					url: app.apiHost + "/module.php?m=forum&a=list&ajax=1",
					data: {
						per_page: per_page,
						catid: catid,
						gid: gid,
						authcode: app.getAuthCode()
					},
					success: function(res) {

						if (!res.data.error) {
							if (isfirst) {
								that.pageData.list = res.data.data.list;
								isfirst = false;
							} else {

								that.pageData.list = app.json_add(that.pageData.list, res.data.data.list);
							}
							per_page = res.data.data.per_page;

						}


					}
				})
			},
			goForum: function(id) {
				uni.navigateTo({
					url: "../forum/show?id=" + id
				})
			},
			refresh: function() {
				this.getPage();
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000)
			},
			loadMore: function() {
				this.getList();
			},
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60;
				console.log(this.TabCur)
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