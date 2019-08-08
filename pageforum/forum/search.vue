<template>
	<view>
		<view style="min-height: 380upx;">
			<view class="bghead" :style="{'height': systems.windowHeight * 0.25 + 'px'}">
				<swiper class="screen-swiper swiper-image square-dot" :indicator-dots="true" :circular="true" :autoplay="true"
				 interval="5000" duration="500">
					<swiper-item v-for="(item,index) in swiperList" :key="index">
						<image :src="item.url" mode="aspectFill" v-if="item.type=='image'" style="align-items: center;"></image>
						<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
					</swiper-item>
				</swiper>
			</view>
		</view>

		<!-- <view class="tabs-border">
			<scroll-view scroll-x class="bg-white nav" scroll-with-animation :scroll-left="scrollLeft">
				
			</scroll-view>
		</view> -->
		<swiper style="height: 175px;background-color: #FFFFFF;">
			<swiper-item>
				<view>
					<scroll-view scroll-x>
						<view class="cu-list grid no-border" :class="['col-' + gridCol]">
							<view style="align-items: center;" class="cu-item" v-for="(item,index) in tabs" :key="index" v-if="index<gridCol*2">
								<view class="icon-tabs" :style="[{'background-image':'url(static/tabs/' + item.cuIcon + ')'}]">
								</view>
								<text>{{item.name}}</text>
							</view>
						</view>
					</scroll-view>
				</view>
			</swiper-item>
		</swiper>
		
		<view v-for="(item,fkey) in pageData.list" :key="fkey" class="cu-card dynamic" :class="isCard?'no-card':''">
			<view class="cu-item shadow">
				<view class="cu-list menu-avatar">
					<view class="cu-item">
						<view class="cu-avatar round lg" :style="[{'background-image':'url(' + item.user_head + '.100x100.jpg)'}]"></view>
						<view class="content flex-sub">
							<view>{{item.nickname}}</view>
							<view class="text-gray text-sm flex justify-between">
								{{item.timeago}}
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
				
				<view class="text-gray text-sm text-right padding">
					<text class="cuIcon-appreciatefill margin-lr-xs"></text> 20 赞
					<text class="cuIcon-attentionfill margin-lr-xs"></text> 10 阅读
				</view>
			</view>
		</view>
		<forum-footer tab="search"></forum-footer>
	</view>
</template>

<script>
	import forumFooter from "../../components/forumfooter.vue";
	var app = require("../../common/common.js");
	var per_page = 0;
	var isfirst = true;
	var catid = 0;
	var gid = 0;
	var activeClass = "tabs-border-active";
	export default {
		components: {
			forumFooter
		},
		data: function() {
			return {
				isCard: true,
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
				gridCol: 4, // 每行显示多少列
				tabs: [{
					cuIcon: 'mtwm-icon.png',
					name: '美团外卖'
				},
				{
					cuIcon: 'jzjj-icon.png',
					name: '兼职/家教'
				},
				{
					cuIcon: 'essc-icon.png',
					name: '二手市场'
				},
				{
					cuIcon: 'lycx-icon.png',
					name: '旅游出行'
				},
				{
					cuIcon: 'pt-icon.png',
					name: '跑腿'
				}]
			}

		},
		onReady: function() {
			
		},
		onLoad: function(option) {
			gid = 1;
			catid = option.catid;
			uni.setNavigationBarTitle({
				title: '微生活'
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
					url: "/pageforum/forum/show?id=" + id
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
			scrolltoupper() {
				console.log(22222222222222222222)
			}
		},
	}
</script>

<style>
	.bghead {
		width: 100%;
		background-color: #3CC48D;
		margin-top: -5px;
		border-radius: 0px 0px 150px 150px;
	}
	
	.icon-tabs {
		background-size: cover;
		width: 100px;
		height: 100px;
	}
	
</style>