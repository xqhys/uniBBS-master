<template>
	<div>
		<div class="footer-row"></div>
		<div class="footer">
			<div class="footer-item icon-home" v-bind:class="{'footer-active':tab=='home'}" @click="goHome()">首页</div>
			<div class="footer-item icon-cascades" v-bind:class="{'footer-active':tab=='campus'}" @click="goCampus()">微校园</div>
			
			<div class="footer-item footer-add" v-bind:class="{'footer-active':tab=='add'}" @click="goAdd()">发布</div>
			<div class="footer-item icon-search" v-bind:class="{'footer-active':tab=='life'}"  @click="goLife()">微生活</div> 
			<div class="footer-item icon-my_light" v-bind:class="{'footer-active':tab=='user'}"  @click="goUser()">我的</div>
		</div>
	</div>
</template>

<script>
	export default{
		props:{
			tab:""
		},
		data:function(){
			return {
				
			}
		},
		methods:{
			goHome:function(){
				uni.reLaunch({
					url:"/pages/home/index?gid=1"
				})
			},
			goCampus:function(){
				uni.reLaunch({
					url:"/pages/campus/index?gid=1"
				})
			},
			goAdd:function(){
				uni.navigateTo({
					url:"/pages/add/add"
				})
			},
			goLife:function(){
				uni.reLaunch({
					url:"/pages/life/index?gid=1"
				})
			},
			goUser:function(){
				uni.reLaunch({
					url:"/pages/user/index"
				})
			}
		}
	}
</script>

<style>
</style>
