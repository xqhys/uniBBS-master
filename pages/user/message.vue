<template>
	<view>
		<view class="view-tabs tabs-border">
			<scroll-view scroll-x class="bg-white nav">
				<view class="flex text-center">
					<view class="cu-item flex-sub" :class="index==TabCur?'text-green cur':''" v-for="(item,index) in tabs" :key="index" @tap="tabSelect" :data-id="index">
						{{item.name}}
					</view>
				</view>
			</scroll-view>
		</view>
		<view>
			<view class="bg-white padding margin text-center">
				暂无消息
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				TabCur: 0,
				tabs: [
					{
						name: '全部'
					},
					{
						name: '未读消息'
					},
					{
						name: '已读消息'
					}
					
				]
			}
		},
		onLoad: function(option) {
			
		},
		methods: {
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60;
				console.log(this.TabCur)
			}
		}
	}
</script>

<style>
	.tabs-position{
		top: 0px;
		z-index: 9999;
		margin-top: 0px !important;
		position: fixed !important;
	}
	
	.padding {
		padding: 15px !important;
		border-radius: 20px 20px 20px 20px;
	}
</style>
