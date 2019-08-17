<template>
	<view class="uni-flex uni-row">
		<view class="input-view">
			<uni-icon type="search" size="22" color="#666666" />
			<input confirm-type="search" class="input" type="text" placeholder="输入搜索关键词" @confirm="confirm" />
		</view>
		
		<view class="btn-view">
			搜索
		</view>
	</view>
</template>

<script>
	import uniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue'
	import uniIcon from '@/components/uni-icon/uni-icon.vue'

	export default {
		components: {
			uniNavBar,
			uniIcon
		},
		data() {
			return {
				
			};
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			confirm() {
				uni.showToast({
					title: '搜索'
				});
			}
		}
	};
</script>

<style>
	.btn-view {
		color: #fff;
		width: 100px;
		height: 65px;
		line-height: 65px;
		text-align: center;
		margin: 32px 10px;
		border-radius: 10px;
		background-color: #56CC9C;
	}
	
	.input-view {
		width: 70%;
		display: flex;
		height: 70px;
		flex-wrap: nowrap;
		line-height: 70px;
		padding: 0 15px;
		margin: 30px 25px;
		border-radius: 20px;
		background-color: #fff;
	}

	.input-view .uni-icon {
		line-height: 70px !important;
	}

	.input-view .input {
		width: 100%;
		height: 70px;
		line-height: 70px;
		padding: 0 5px;
	}
</style>