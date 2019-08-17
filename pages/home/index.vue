<template>
	<view>
		<schools ref='schools' @doGetLocation="doGetLocation"></schools>
		<view v-if="userinfo">
			<view class="bghead">
				<view class="title-search">
					<view class="view-title" @click="goSchools">
						<text class="text-white">{{schools.length > 7 ? schools.substring(0,7) + '...' : schools}}</text>
						<uni-icon type="arrowdown" color="#fff" size="20" />
					</view>
					<view class="view-search" @click="search">
						<uni-icon style="line-height: 20px;" type="search" size="22" color="#666666" />
						<text class="text-gray">请输入搜索关键词</text>
					</view>
				</view>

				<swiper class="screen-swiper swiper-image square-dot radius-imags" :indicator-dots="true" :circular="true"
				 :autoplay="true" interval="5000" duration="500">
					<swiper-item @click="goApp" v-for="(item,index) in swiperList" :key="index" :id="index">
						<image :src="item.url" mode="aspectFill" class="radius-imags"></image>
					</swiper-item>
				</swiper>
			</view>

			<swiper class="swiper-tabs">
				<swiper-item>
					<scroll-view scroll-x>
						<view class="cu-list grid no-border" :class="['col-' + gridCol]">
							<view @click="goTabs" class="cu-item tabs-item" v-for="(item,index) in tabs" :key="index" :id="index" v-if="index<gridCol*2">
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
			<forum-footer tab="home" @apex="apex"></forum-footer>
		</view>

		<view class="cu-modal" :class="{'show':isShow}">
			<view class="cu-dialog">
				<view class="text-black">
					<view class="notice-title">
						平台广告
					</view>
					<view class="content-title">
						<text>
							校鲜大学生生活平台，欢迎您的加入，
							让我们共同维护大学生平台秩序
						</text>
					</view>
				</view>
				<view class="cu-bar bg-ccd">
					<button open-type="getUserInfo" class="bg-ccd flex-sub text-white" @tap="hideModal">确定</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniIcon from '@/components/uni-icon/uni-icon.vue'
	import schools from '@/pages/home/location.vue'
	import forumFooter from "@/components/forumfooter.vue";

	var app = require("@/common/common.js");
	var per_page = 0;
	var isfirst = true;
	var catid = 0;
	var gid = 0;
	export default {
		components: {
			forumFooter,
			uniIcon,
			schools
		},
		data: function() {
			return {
				isCard: true,
				pageLoad: false,
				pageData: {},
				isShow: false,
				gridCol: 4, // 每行显示多少列
				schools: uni.getStorageSync('schools'),
				userinfo: uni.getStorageSync('userinfo'),
				swiperList: [{
					id: 0,
					type: 'image',
					url: '../../static/swiper/1-1.jpg'
				}, {
					id: 1,
					type: 'image',
					url: '../../static/swiper/2-1.jpg'
				}, {
					id: 2,
					type: 'image',
					url: '../../static/swiper/3-1.jpg'
				}, {
					id: 3,
					type: 'image',
					url: '../../static/swiper/4-1.jpg'
				}],
				tabs: [{
						cuIcon: 'mtwm-icon.png',
						name: '美食外卖'
					},
					{
						cuIcon: 'essc-icon.png',
						name: '二手市场'
					}
					,
					{
						cuIcon: 'jzjj-icon.png',
						name: '兼职/家教'
					},
					{
						cuIcon: 'lycx-icon.png',
						name: '旅游出行'
					}
				]
			}
		},
		onReady: function() {
			if (this.userinfo) {
				this.isShow = !this.userinfo.isLogin;
			} else {
				let userinfo = {
					address: '定位中...',
					isLogin: false,
					user_code: '',
					userData: ''
				};
				uni.setStorageSync('userinfo', userinfo);
				this.$refs.schools.doGetLocation();
			}
		},
		onLoad: function(option) {
			if (option.flag) {
				this.isShow = !this.userinfo.isLogin;
			}
			gid = 1;
			catid = option.catid;
			uni.setNavigationBarTitle({
				title: '微校园，微生活'
			});
			this.getPage();
		},
		onShow: function() {},
		onHide: function() {},
		onReachBottom: function() {},
		onPullDownRefresh: function() {
			uni.stopPullDownRefresh();
		},
		computed: {},
		watch: {},
		methods: {
			IsCard(e) {
				this.isCard = e.detail.value
			},
			hideModal() {
				let self = this;
				//#ifdef MP-WEIXIN
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						let user_code = loginRes.code; //js_code可以给后台获取unionID或openID作为用户标识
						uni.getUserInfo({ // 获取用户信息
							provider: 'weixin',
							success: function(infoRes) {
								let userinfo = uni.getStorageSync('userinfo');
								userinfo.isLogin = true;
								userinfo.user_code = user_code;
								userinfo.userData = JSON.parse(infoRes.rawData);
								uni.setStorageSync('userinfo', userinfo);
								self.isShow = false;
							},
							fail: function(res) {}
						})
					},
					fail: function(res) {}
				})
				//#endif
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
			goSchools() {
				uni.navigateTo({
					url: "/pages/home/location"
				})
			},
			goApp(e) {
				switch (e.currentTarget.id){
					case '0':
						uni.showToast({
						    title: '暂无小程序',
							icon: 'none',
						    duration: 1500
						});
						/* uni.navigateToMiniProgram({
						  appId: '',
						  path: 'pages/index/index?id=123',
						  extraData: {
						    'data1': 'test'
						  },
						  success(res) {
						  }
						}) */
						break;
					case '1':
						uni.showToast({
						    title: '暂无小程序',
							icon: 'none',
						    duration: 1500
						});
						/* uni.navigateToMiniProgram({
						  appId: '',
						  path: 'pages/index/index?id=123',
						  extraData: {
						    'data1': 'test'
						  },
						  success(res) {
						  }
						}) */
						break;
					case '2':
						uni.navigateTo({
							url: '/pageforum/schools/index'
						})
						break;
					default:
						uni.navigateTo({
							url: '/pageforum/life/index'
						})
						break;
				}
			},
			goTabs(e) {
				switch (e.currentTarget.id){
					case '2':
						uni.navigateTo({
							url:  '/pageforum/schools/index?tabs=jzjj'
						})
						break;
					case '3':
						uni.navigateTo({
							url:  '/pageforum/schools/index?tabs=lycx'
						})
						break;
					default:
						this.goApp(e);
						break;
				}
			},
			search() {
				uni.navigateTo({
					url: '/pages/home/search'
				})
			},
			apex() {
				uni.pageScrollTo({
					duration: 0,
					scrollTop: 0
				})
			}
		}
	}
</script>

<style scoped>
	.bghead {
		width: 100%;
		height: 435px;
		background-color: #56CC9C;
		border-radius: 0px 0px 190px 190px;
	}

	.title-search {
		display: flex;
		min-height: 88px;
	}

	.view-title {
		width: 60%;
		height: 63px;
		line-height: 63px;
		text-align: center;
	}

	.view-search {
		width: 93%;
		height: 63px;
		line-height: 63px;
		padding: 0 4%;
		margin-right: 25px;
		border-radius: 20px;
		background-color: #fff;
	}


	.tabs-item {
		align-items: center;
	}

	.swiper-tabs {
		height: 185px;
		margin-top: 25px;
		background-color: #FFFFFF;
	}

	
	.icon-tabs {
		background-size: cover;
		width: 95px;
		height: 95px;
	}
	
	.notice-title {
		font-size: 40px;
		font-weight: bold;
		margin-top: 50px;
	}

	.content-title {
		padding: 10px 0px 70px 0px;
	}
</style>
