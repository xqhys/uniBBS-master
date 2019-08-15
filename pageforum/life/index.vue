<template>
	<view>
		<view class="bghead">
			<swiper class="screen-swiper swiper-image radius-imags" :indicator-dots="false" :circular="true"
			 :autoplay="true" interval="5000" duration="500">
				<swiper-item v-for="(item,index) in swiperList" :key="index">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'" class="radius-imags"></image>
					<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
				</swiper-item>
			</swiper>
		</view>

		<swiper class="swiper-tabs">
			<swiper-item>
				<scroll-view scroll-x>
					<view class="cu-list grid no-border tags-list" :class="['col-' + gridCol]">
						<view style="align-items: center;" class="cu-item " v-for="(item,index) in tabs" :key="index" v-if="index<gridCol*2">
							<view class="icon-tabs" :style="[{'background-image':'url(static/tabs/' + item.cuIcon + ')'}]">
							</view>
							<text>{{item.name}}</text>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>

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
		<forum-footer tab="life" @apex="apex"></forum-footer>
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
	export default {
		components: {
			forumFooter,
			uniIcon
		},
		data: function() {
			return {
				isCard: true,
				pageData: {},
				TabCur: 0,
				scrollLeft: 0,
				gridCol: 4, // 每行显示多少列
				systems: uni.getSystemInfoSync(),
				swiperList: [{
					id: 0,
					type: 'image',
					url: '../../static/swiper/4-1.jpg'
				}],
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
					}
				]
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
					url: "/pageforum/forum/show?id=" + id
				})
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

<style scoped>
	.bghead {
		width: 100%;
		height: 355px;
		background-color: #56CC9C;
		border-radius: 0px 0px 190px 190px;
	}

	.swiper-tabs {
		height: 330px;
		margin-top: 20px;
		background-color: #FFFFFF;
	}

	.tags-list {
		padding: 28px 0px 0px 0px !important;
	}

	.icon-tabs {
		width: 95px;
		height: 95px;
		margin-top: -20px;
		background-size: cover;
	}
</style>
