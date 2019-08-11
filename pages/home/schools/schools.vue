<template>
	<view v-if="isShow">
		<view style="padding: 10px 0px 10px 15px;background-color: #fff;">
			当前位置:
		</view>
		<view class="cu-bar bg-white solid-bottom">
			<view class="action">
				{{address.length > 16 ? address.substring(0, 16) + '...':address}}
			</view>
			<view class="action text-green" @click="doGetLocation">
				<text class="cuIcon-focus margin-lr-xs" style="font-size: 20px;"></text>重新定位
			</view>
		</view>

		<view class="cu-bar bg-white">
			<view class="action text-orange">
				请选择地区：
			</view>
			<view class="action text-orange">
				{{pickerText}}
			</view>
			<view class="action text-gray" @click="showMulLinkageThreePicker">
				<text class="cuIcon-right margin-lr-xs" style="font-size: 15px;"></text>
			</view>
		</view>

		<view class="uni-flex uni-row bg-white view-search">
			<view class="input-view bg-gray">
				<uni-icon type="search" size="22" color="#666666" />
				<input confirm-type="search" class="input" type="text" placeholder="输入搜索关键词" @confirm="confirm" />
			</view>
			<view class="search-form">
				搜索
			</view>
		</view>

		<view class="title-sc">
			附近学校:
		</view>

		<view class="view-school text-orange">
			<view class="name-schools solid-bottom" @click="goIndex">
				河南师范大学
			</view>
			<view class="name-schools solid-bottom">
				湖北师范大学
			</view>
		</view>

		<view class="advert">
			<text>
				更多高校正在加盟中...

				有疑问请联系客服
				查看案例请搜索“河南师范大学”
			</text>
		</view>

		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValueDefault"
		 @onCancel="onCancel" @onConfirm="onConfirm"></mpvue-city-picker>
	</view>
</template>

<script>
	import mpvueCityPicker from '@/components/mpvue-citypicker/mpvueCityPicker.vue'
	import cityData from '@/common/city.data.js';
	import uniIcon from '@/components/uni-icon.vue'
	var util = require('../../../common/util.js');
	var formatLocation = util.formatLocation;

	export default {
		components: {
			mpvueCityPicker,
			uniIcon
		},
		data() {
			return {
				isShow: false,
				mulLinkageTwoPicker: cityData,
				cityPickerValueDefault: [0, 0, 1],
				themeColor: '#007AFF',
				pickerText: '北京市-市辖区-东城区',
				address: ''
			};
		},
		onLoad() {
			const userinfo = uni.getStorageSync('userinfo');
			this.address = userinfo.address;
			this.isShow = true;
		},
		methods: {
			goIndex() {
				this.isShow = false;
				uni.reLaunch({
					url: "/pages/home/index?flag=true"
				})
			},
			onCancel(e) {
				
			},
			// 三级联动选择
			showMulLinkageThreePicker() {
				this.$refs.mpvueCityPicker.show()
			},
			onConfirm(e) {
				this.pickerText = e.label;
			},
			doGetLocation() {
				let self = this;
				self.isShow = true;
				let userinfo = uni.getStorageSync('userinfo');
				self.address = userinfo.address;
				//#ifdef MP-WEIXIN
					uni.authorize({
					    scope: 'scope.userLocation',
					    success() {
							uni.chooseLocation({
					        	success: (res) => {
					        		self.address = res.address;
									userinfo.address = res.address;
									uni.setStorageSync('userinfo', userinfo);
					        	}
					        });
					    },
						fail: (err) => {
							
						}
					});
				//#endif
				
			}
		},
		onBackPress() {
			if (this.$refs.mpvueCityPicker.showPicker) {
				this.$refs.mpvueCityPicker.pickerCancel();
				return true;
			}
		},
		onUnload() {
			if (this.$refs.mpvueCityPicker.showPicker) {
				this.$refs.mpvueCityPicker.pickerCancel()
			}
		}
	};
</script>

<style scoped>
	.view-search {
		align-items: center;
		padding: 15px 0px 10px 15px;
		box-shadow: gray 0px 5px 10px 0px; //边框内阴影
	}

	.input-view {
		width: 77%;
		display: flex;
		background-color: #e7e7e7;
		height: 70px;
		line-height: 70px;
		border-radius: 15px;
		padding: 0 4%;
		flex-wrap: nowrap;
		margin: 7px 0;
	}

	.input-view .uni-icon {
		line-height: 70px !important;
	}

	.input-view .input {
		height: 70px;
		line-height: 70px;
		width: 94%;
		padding: 0 3%;
	}

	.search-form {
		padding-left: 45px;
		font-size: 40px;
		color: #00BCD4;
		font-weight: bold;
	}

	.title-sc {
		margin-top: 20px;
		padding: 10px 0px 25px 15px;
		/* background-color: #F7F7F7; */
	}

	.view-school {
		background-color: #fff;
		box-shadow: gray 0px 5px 10px 0px; //边框内阴影
	}

	.name-schools {
		padding: 15px 0px 15px 25px;
	}

	.advert {
		text-align: center;
		padding-top: 40px;
		/* background-color: #F7F7F7; */
	}
</style>
