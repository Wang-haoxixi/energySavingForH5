<template>
	<view class="containner">
		<view class="" style="padding: 30rpx 30rpx 0rpx;background-color: #FFFFFF;border-radius: 30rpx;margin-bottom: 40rpx;">
			<view class="" style="display: flex;justify-content: flex-start;align-items: center;padding-bottom: 40rpx;">
				<image src="../../static/icon_examinationInfo.png" mode="" style="width: 36rpx;height: 36rpx;margin-right: 16rpx;"></image>
				<view class="" style="color: #303031;font-weight: bold;line-height: 44rpx;font-size: 32rpx;">
					证书信息
				</view>
			</view>
			<view class="" style="display: flex;justify-content: space-between;align-items: center;padding-bottom: 40rpx;color:#666769;font-size: 28rpx;line-height: 40rpx;">
				<view class="" style="margin-right: 40rpx;">
					姓名
				</view>
				<view class="" style="flex: 1;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;text-align: right;">
					{{data.userName}}
				</view>
			</view>
			<view class="" style="display: flex;justify-content: space-between;align-items: center;padding-bottom: 40rpx;color:#666769;font-size: 28rpx;line-height: 40rpx;">
				<view class="" style="margin-right: 40rpx;">
					证书名称
				</view>
				<view class="" style="flex: 1;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;text-align: right;">
					{{data.certificateName}}
				</view>
			</view>
			<view class="" style="display: flex;justify-content: space-between;align-items: center;padding-bottom: 40rpx;color:#666769;font-size: 28rpx;line-height: 40rpx;">
				<view class="" style="margin-right: 40rpx;">
					证书编号
				</view>
				<view class="" style="flex: 1;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;text-align: right;">
					{{data.number}}
				</view>
			</view>
			<view class="" style="display: flex;justify-content: space-between;align-items: center;padding-bottom: 40rpx;color:#666769;font-size: 28rpx;line-height: 40rpx;">
				<view class="" style="margin-right: 40rpx;">
					有效期
				</view>
				<view class="" style="flex: 1;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;text-align: right;">
					{{time}}
				</view>
			</view>
			<view class="" style="display: flex;justify-content: space-between;align-items: center;padding-bottom: 40rpx;color:#666769;font-size: 28rpx;line-height: 40rpx;">
				<view class="" style="margin-right: 40rpx;">
					颁发机构
				</view>
				<view class="" style="flex: 1;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;text-align: right;">
					{{data.awardingOrg}}
				</view>
			</view>
		</view>
		<view class="" style="padding: 30rpx 30rpx 40rpx;background-color: #FFFFFF;border-radius: 30rpx;">
			<view class="" style="display: flex;justify-content: flex-start;align-items: center;padding-bottom: 26rpx;">
				<image src="../../static/icon_examinationCard.png" mode="" style="width: 36rpx;height: 36rpx;margin-right: 16rpx;"></image>
				<view class="" style="color: #303031;font-weight: bold;line-height: 44rpx;font-size: 32rpx;">
					证书
				</view>
			</view>
			<image :src="data.imgUrl" mode="aspectFit" style="width: 100%;height: 860rpx;"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data:'',
				time:''
			};
		},
		onLoad(option) {
			uni.showLoading({
				title: '加载中'
			});
			let that = this;
			uni.request({
				url: '/api/qms/exam_user_certificate/check_certificate/'+ option.id,
				success: (res) => {
					// console.log(res)
					that.data = res.data.data;
					that.time =that.data.validTimeEnd.split(' ')[0]
					uni.hideLoading();
				}
			});
		}
	}
</script>

<style lang="scss">
page{
	background-color: #F4F5F6;
}
.containner{
	padding: 40rpx;
}
</style>
