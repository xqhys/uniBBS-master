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
		
		<view class="text uni-flex title-theme bg-white">
			<text>请选择您要发布的问题栏目</text>
		</view>
		
		<view style="height: 100%;" class="cu-list grid no-border" :class="['col-' + gridCol]">
			<view class="cu-item" v-for="(item,index) in columns" :key="index" :id="index" @click="publishPosts" v-if="index < 9">
				<view class='icon-tabs my-tag' :class="'bg-' + item.name">{{item.theme}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				gridCol: 4,
				swiperList: [{
					id: 0,
					type: 'image',
					url: '../../static/swiper/5-1.jpg'
				}],
				columns: [{
					theme: '校园',
					title: '嫣红',
					name: 'red',
					color: '#e54d42'
				},
				{
					theme: '问答',
					title: '桔橙',
					name: 'orange',
					color: '#f37b1d'
				},
				{
					theme: '生活',
					title: '明黄',
					name: 'yellow',
					color: '#fbbd08'
				},
				{
					theme: '情感',
					title: '橄榄',
					name: 'olive',
					color: '#8dc63f'
				},
				{
					theme: '旅游',
					title: '森绿',
					name: 'green',
					color: '#39b54a'
				},
				{
					theme: '广告',
					title: '天青',
					name: 'cyan',
					color: '#1cbbb4'
				},
				{
					theme: '失物招领',
					title: '海蓝',
					name: 'blue',
					color: '#0081ff'
				},
				{
					theme: '兼职/家教',
					title: '姹紫',
					name: 'purple',
					color: '#6739b6'
				},
				{
					theme: '二手物品',
					title: '木槿',
					name: 'mauve',
					color: '#9c26b0'
				},
				{
					theme: '',
					title: '桃粉',
					name: 'pink',
					color: '#e03997'
				},
				{
					theme: '',
					title: '棕褐',
					name: 'brown',
					color: '#a5673f'
				},
				{
					theme: '',
					title: '玄灰',
					name: 'grey',
					color: '#8799a3'
				},
				{
					theme: '',
					title: '草灰',
					name: 'gray',
					color: '#aaaaaa'
				},
				{
					theme: '',
					title: '墨黑',
					name: 'black',
					color: '#333333'
				},
				{
					theme: '',
					title: '雅白',
					name: 'white',
					color: '#ffffff'
				},
			]
			};
		},
		methods: {
			publishPosts(e) {
				uni.navigateTo({
					url: "/pageforum/forum/add?theme=" + this.columns[e.currentTarget.id].theme
				})
			}
		}
	}
</script>

<style>
	.bghead {
		width: 100%;
		height: 355px;
		background-color: #56CC9C;
		border-radius: 0px 0px 190px 190px;
	}
	
	.title-theme {
		color: #F0A431;
		font-size: 32upx;
		margin-top: 24px;
		margin-bottom: 16px;
		flex: 1;
		height: 85upx;
		-webkit-flex: 1;
		-webkit-justify-content: center;
		justify-content: center;
		-webkit-align-items: center;
		align-items: center;
		border-radius: 0px 0px 40px 40px;
	}
	
	.icon-tabs {
		height: 120px;
		font-size: 28upx;
		vertical-align: middle;
		position: relative;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
		font-family: Helvetica Neue, Helvetica, sans-serif;
		white-space: nowrap;
		margin: 10px 20px 10px 20px;
		border-radius: 40px 40px 40px 40px;
	}
	
</style>
