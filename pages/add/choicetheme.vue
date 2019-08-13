<template>
	<view>
		<view class="bghead">
			<swiper class="screen-swiper swiper-image square-dot radius-imags" :indicator-dots="true" :circular="true" :autoplay="true"
			 interval="5000" duration="500">
				<swiper-item v-for="(item,index) in swiperList" :key="index">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'" class="radius-imags"></image>
				</swiper-item>
			</swiper>
		</view>
		
		<view class="text uni-flex title-theme bg-white">
			<text>请选择您要发布的问题栏目</text>
		</view>
		
		<view class="cu-list grid no-border" :class="['col-' + gridCol]">
			<view class="cu-item" v-for="(item,index) in ColorList" :key="index" v-if="index < 9">
				<view @click="publishPosts" class='icon-tabs my-tag' :class="'bg-' + item.name">{{item.theme}}</view>
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
				ColorList: [{
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
			publishPosts() {
				uni.navigateTo({
					url: "/pages/add/add",
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
		height: 70px;
		font-size: 28upx;
		vertical-align: middle;
		position: relative;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
		font-family: Helvetica Neue, Helvetica, sans-serif;
		white-space: nowrap;
		margin: 8px 8px 8px 8px;
		border-radius: 20px 20px 20px 20px;
	}
	
</style>
