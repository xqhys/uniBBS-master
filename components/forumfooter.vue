<template>
	<div>
		<div class="footer-row"></div>
		<view class="add-position" v-if="isShow">
			<view class="release" @click="goAdd()">发布</view>
			<view class="apex" @click="apex">
				<text class="text-white cuIcon-top" style="font-size: 20px;"></text>
			</view>
		</view>
		<div class="footer">
			<!-- <div class="footer-item icon-home" v-bind:class="{'footer-active':tab=='home'}" @click="goHome()">首页</div>
			<div class="footer-item icon-cascades" v-bind:class="{'footer-active':tab=='schools'}" @click="goSchools()">微校园</div>
			<div class="footer-item footer-add" v-bind:class="{'footer-active':tab=='add'}" @click="goAdd()">发布</div>
			<div class="footer-item icon-search" v-bind:class="{'footer-active':tab=='life'}"  @click="goLife()">微生活</div> 
			<div class="footer-item icon-my_light" v-bind:class="{'footer-active':tab=='user'}"  @click="goUser()">我的</div> -->
			<view class="footer-list cu-list grid col-4 no-border">
				<view style="align-items: center;" class="footer-item paren-item cu-item" @click="goHome()">
					<view class="icon-tabs footer-home" v-bind:class="{'footer-home-active':tab=='home'}">
					</view>
					<view :class="{'text-cl':tab=='home'}">首页</view>
				</view>
				<view style="align-items: center;" class="footer-item paren-item cu-item" @click="goSchools()">
					<view class="icon-tabs footer-schools" v-bind:class="{'footer-schools-active':tab=='schools'}">
					</view>
					<view :class="{'text-cl':tab=='schools'}">微校园</view>
				</view>
				<view style="align-items: center;" class="footer-item paren-item cu-item" @click="goLife()">
					<view class="icon-tabs footer-life" v-bind:class="{'footer-life-active':tab=='life'}">
					</view>
					<view :class="{'text-cl':tab=='life'}">微生活</view>
				</view>
				<view style="align-items: center;" class="footer-item paren-item cu-item" @click="goUser()">
					<view class="icon-tabs footer-my" v-bind:class="{'footer-my-active':tab=='user'}">
					</view>
					<view :class="{'text-cl':tab=='user'}">我的</view>
				</view>
			</view>
		</div>
	</div>
</template>

<script>
	export default{
		props:{
			tab:"",
			isShow: {
				type: Boolean,
				default: true
			}
		},
		data:function(){
			return {
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
				}]
			}
		},
		methods:{
			goHome:function(){
				uni.reLaunch({
					url:"/pages/home/index?gid=1"
				})
			},
			goSchools:function(){
				uni.reLaunch({
					url:"/pageforum/schools/index?gid=1"
				})
			},
			goAdd:function(){
				uni.navigateTo({
					url:"/pageforum/forum/choicetheme"
				})
			},
			goLife:function(){
				uni.reLaunch({
					url:"/pageforum/life/index?gid=1"
				})
			},
			goUser:function(){
				uni.reLaunch({
					url:"/pages/user/index"
				})
			},
			apex:function(){
				this.$emit('apex');
			}
		}
	}
</script>

<style>
	.footer-list {
		padding: 0px 0px !important;
	}
	
	.footer-item {
		padding: 10px 0px 0px 0px !important;
	}
	
	.footer-home {
		background-image: url('../static/banner/home.png');
	}
	
	.footer-home-active {
		background-image: url('../static/banner/home-active.png');
	}
	
	.footer-schools {
		background-image: url('../static/banner/schools.png');
	}
	
	.footer-schools-active {
		background-image: url('../static/banner/schools-active.png');
	}
	
	.footer-life {
		background-image: url('../static/banner/life.png');
	}
	
	.footer-life-active {
		background-image: url('../static/banner/life-active.png');
	}
	
	.footer-my {
		background-image: url('../static/banner/my.png');
	}
	
	.footer-my-active {
		background-image: url('../static/banner/my-active.png');
	}
	
	.paren-item {
		height: 100px !important;
	}
	
	.text-cl {
		color: #29E89B;
	}
	
	.icon-tabs {
		background-size: cover;
		width: 40px;
		height: 40px;
	}
	
	.add-position {
		color: #fff;
		right: 30px;
		bottom: 130px;
		z-index: 9999;
		margin-top: 0px !important;
		position: fixed !important;
	}
	
	.release {
		min-height: 80px;
		min-width: 80px;
		text-align: center;
		padding-top: 18px;
		background: rgba(222,140,16,.6);
		border-radius: 80px 80px 80px 80px;
	}
	
	.apex {
		margin-top: 20px;
		min-height: 80px;
		min-width: 80px;
		text-align: center;
		background: rgba(0,0,0,.6);
		border-radius: 80px 80px 80px 80px;
	}
</style>
