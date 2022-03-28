<template>
	<view style="padding: 80rpx 40rpx;">
		<view style="border-bottom: #EFF0F1 solid 1rpx;padding: 24rpx 0;display: flex;align-items: center;">
			<image src="../../static/icon_shouji.png" style="width: 36rpx;height: 36rpx;margin-right: 8rpx;"></image>
			<input placeholder="请输入手机号码" maxlength="11" type="number" v-model="data.mobile" placeholder-style="color:#BABEC0" />
		</view>
		<view style="border-bottom: #EFF0F1 solid 1rpx;padding: 24rpx 0;display: flex;align-items: center;margin-top: 20rpx;justify-content: space-between;">
			<view style="display: flex;align-items: center;">
				<image src="../../static/icon_yanzm.png" style="width: 36rpx;height: 36rpx;margin-right: 8rpx;"></image>
				<input placeholder="请输入验证码" maxlength="4" v-model="data.code" type="number" placeholder-style="color:#BABEC0" />
			</view>
			<view v-if="verificationShow" style="color: #F74437;font-size: 28rpx;line-height: 40rpx;align-self:flex-end" @tap="getVerification">获取验证码</view>
			<view v-if="!verificationShow" style="color: #F74437;font-size: 28rpx;line-height: 40rpx;align-self:flex-end">{{time}}
				<text style="color: #666769;">秒后重试</text></view>
		</view>
		<view style="color: #F74437;font-size: 28rpx;line-height: 40rpx;margin-top: 60rpx;" @tap="account">
			完善密码
		</view>
		<view style="margin-top: 300rpx;">
			<view @tap="register" style="background-color: #F74437;color: #FFFFFF;height: 88rpx;width: 670rpx;border-radius: 20rpx;font-size: 32rpx;line-height:88rpx;text-align: center;">注册</view>
		</view>
		<view style="margin-top: 40rpx;font-size: 24rpx;line-height: 34rpx;color: #919397;">
			注册代表您已同意<text style="color: #F74437;" @tap="agreement">《用户协议》</text>和<text style="color: #F74437;" @tap="privacy">《隐私政策》</text>
		</view>
		<view style="margin-top: 120rpx;font-size: 28rpx;line-height: 40rpx;text-align: center;">
			已有账号？<text style="color: #F74437;" @tap="download">立即下载</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data: {
					mobile: '',
					code: '',
					inviter: 0,
				},
				verificationShow: true,
				time: 60,
			};
		},
		methods: {
			register() {
				let that = this;
				if (that.data.mobile.length != 11) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确的手机号'
					})
				} else if (that.data.code.length != 4) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确的验证码'
					})
				} else {
					uni.request({
						url: '/api/admin/user/register',
						method: 'POST',
						data: that.data,
						success: (res) => {
							if(res.data.code == 0){
								uni.navigateTo({
									url:'download'
								})
							}else{
								uni.showToast({
									icon: 'none',
									title: res.data.message
								})
							}
						}
					});
				}
			},
			getVerification() {
				let that = this;
				if (that.data.mobile.length != 11) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确的手机号'
					})
				} else {
					uni.request({
						url: '/api/admin/mobile/' + that.data.mobile, //仅为示例，并非真实接口地址。
						success: (res) => {
							if (res.data.code != 0) {
								console.log(res.data)
								uni.showToast({
									icon: 'none',
									title: res.data.message
								})
							} else {
								that.time = 60;
								that.verificationShow = false;
								let timer = setInterval(function() {
									that.time = that.time - 1;
									console.log(that.time)
									if (that.time < 1) {
										clearInterval(timer);
										that.verificationShow = true;
									}
								}, 1000);
							}
						}
					});
				}
			},
			account() {
				uni.navigateTo({
					url: 'account?inviter=' + this.data.inviter
				})
			},
			download() {
				uni.navigateTo({
					url: 'download'
				})
			},
			agreement() {
				uni.navigateTo({
					url: '../agreement/agreement'
				})
			},
			privacy() {
				uni.navigateTo({
					url: '../privacy/privacy'
				})
			},
			isNumber(val) {
			  // negative or positive
			  return /^[-]?[\.\d]+$/.test(val);
			}
		},
		onLoad(option) {
			if (this.isNumber(option.inviter)){
				this.data.inviter = option.inviter;
			}
			else{
				this.data.inviter = 0;
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #FFFFFF;
	}

	input {
		font-size: 28rpx;
		// color: #BABEC0;
		line-height: 40rpx;
	}
</style>
