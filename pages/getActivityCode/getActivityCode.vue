<template>
	<swiper :interval="3000" :duration="1000" style="height: 1000rpx">
		<swiper-item v-for="(item,index) in data" :key="index">
			<view style="display: flex;flex-direction: column;align-items: center;">
				<view style="margin-top: 60rpx;font-weight: bold;color: #303031;font-size: 44rpx;">{{index - -1}}/{{data.length}}</view>
				<view style="margin-top: 40rpx;color: #666769;font-size: 28rpx;">
					签到码：编号{{item.orderNo}}
				</view>
				<view style="margin-top: 80rpx;display: flex;align-items: center;">
					<view style="width: 30rpx;height: 30rpx;;margin-right: 50rpx;">
						<image v-show="index!=0" src="../../static/icon_houfan@3x.png" style="width: 30rpx;height: 30rpx;"></image>
					</view>
					<image :src="item.qrimg" style="width: 510rpx;height: 510rpx;border-radius: 30rpx;"></image>
					<view style="width: 30rpx;height: 30rpx;margin-left: 50rpx;">
						<image v-show="index!=data.length && data.length>1" src="../../static/icon_qianfan@3x.png" style="width: 30rpx;height: 30rpx;"></image>
					</view>
				</view>
				<view style="margin-top: 36rpx;font-weight: bold;color: #303031;font-size: 32rpx;">
					到活动现场可出示签到
				</view>
			</view>
		</swiper-item>
	</swiper>
</template>

<script>
	import QRCode from 'qrcode'
	export default {
		data() {
			return {
				data: [],
				id: ''
			};
		},
		methods: {
			generateQR(data) {
				data.forEach(async (item, index) => {
					try {
						item.qrimg = await QRCode.toDataURL('signIn{' + item.orderNo + '}', {
							width: 255,
							margin: 0
						});
					} catch (err) {
						console.error(err)
					}
				});
				this.data = data;
			}
		},
		onLoad(option) {
			this.id = option.id;
			uni.request({
				url: '/api/qms/conference_ticketing/sign_in_code?enrollId=' + option.id,
				success: (res) => {
					if(res.data.code == 0){
						let data = res.data.data;
						this.generateQR(data);
					}
					else{
						uni.showModal({
						    title: '提示',
						    content: res.data.message,
							showCancel: false,
						    success: function (res) {
						        if (res.confirm) {
						            console.log('用户点击确定');
						        } else if (res.cancel) {
						            console.log('用户点击取消');
						        }
						    }
						});
					}
				},
			});
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #FFFFFF;
	}
</style>
