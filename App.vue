<script>
	export default {
		onLaunch: function () {
			console.log('App Launch');
			// #ifdef APP-PLUS
			// 锁定屏幕方向
			plus.screen.lockOrientation('portrait-primary'); //锁定
			console.log(plus.runtime.version);
			// 检测升级
			uni.request({
				url: 'https://www.fd175.com/index.php?m=app&a=checkupdate', //检查更新的服务器地址
				data: {
					appid: plus.runtime.appid,
					version: plus.runtime.version,
					imei: plus.device.imei
				},
				success: (res) => {
					console.log('success', res);
					if (res.statusCode == 200 && res.data.isUpdate) {
						let openUrl = plus.os.name === 'iOS' ? res.data.iOS : res.data.Android;
						// 提醒用户更新
						uni.showModal({
							title: '更新提示',
							content: res.data.note ? res.data.note : '是否选择更新',
							success: (showResult) => {
								if (showResult.confirm) {
									plus.runtime.openURL(openUrl);
								}
							}
						})
					}
				}
			})
			// #endif
		},
		onShow: function () {
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		},
		 watch: {
		  '$route' (to, from) {
			  
				const toDepth = to.path.split('/').length
				const fromDepth = from.path.split('/').length
			
			
		  }
		}
	}
</script>

<style>	
@import "./common/iconfont.css";
@import "./common/dt-ui-uni.css";
@import "./common/forum.css";
@import "common/main.css";
@import "common/icon.css";
@import "common/uni.css";

uni-input div{
	width: 100%;
}
.input-flex-btn{
	z-index: 2;
}
.header,.header-row{
		display: none;
}
uni-page-head .uni-page-head-hd{
	flex-direction: row;
}
.uni-tabbar,.uni-picker-view-wrapper{
	flex-direction: row !important;
}
.uni-swiper-dots{
	flex-direction: row;
}
.uni-scroll-view>div{
	flex-direction: row;
}
.fixedAdd{
	position: fixed;
	bottom: 200upx;
	right: 7upx;
	width: 92upx;
	height: 92upx;
	text-align: center;
	box-sizing: border-box;
	background-color: rgba(240,85,75,.82);
	color: #fff;
	font-family: iconfont;
	font-size: 32upx;
	padding-top: 11upx;
	border-radius: 22upx;
	
}
.fixedAdd:before{
	content: "\e7e8";
	display: block;
	font-size: 32upx;
}

</style>
