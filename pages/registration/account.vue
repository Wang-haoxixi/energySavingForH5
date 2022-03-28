<template>
	<view style="padding: 80rpx 40rpx;">
		<view style="border-bottom: #EFF0F1 solid 1rpx;padding: 24rpx 0;display: flex;align-items: center;">
			<image src="../../static/icon_shouji.png" style="width: 36rpx;height: 36rpx;margin-right: 8rpx;"></image>
			<input placeholder="请输入手机号码" maxlength="11" v-model="data.mobile" type="number" placeholder-style="color:#BABEC0" />
		</view>
		<view style="border-bottom: #EFF0F1 solid 1rpx;padding: 24rpx 0;display: flex;align-items: center;justify-content: space-between;">
			<view style="display: flex;align-items: center;">
				<image src="../../static/icon_yanzm.png" style="width: 36rpx;height: 36rpx;margin-right: 8rpx;"></image>
				<input placeholder="请输入验证码" maxlength="4" v-model="data.code" type="number" placeholder-style="color:#BABEC0" />
			</view>
			<view v-if="verificationShow" style="color: #F74437;font-size: 28rpx;line-height: 40rpx;align-self:flex-end" @tap="getVerification">获取验证码</view>
			<view v-if="!verificationShow" style="color: #F74437;font-size: 28rpx;line-height: 40rpx;align-self:flex-end">{{time}}
				<text style="color: #666769;">秒后重试</text></view>
		</view>
		<view class="line">
			<span>以下内容为选填</span>
		</view>
		<view style="border-bottom: #EFF0F1 solid 1rpx;padding: 24rpx 0;display: flex;align-items: center;">
			<image src="../../static/icon_mima.png" style="width: 36rpx;height: 36rpx;margin-right: 8rpx;"></image>
			<input placeholder="请设置登录密码" maxlength="18" v-model="data.password" password placeholder-style="color:#BABEC0" />
		</view>
		<view style="border-bottom: #EFF0F1 solid 1rpx;padding: 24rpx 0;display: flex;align-items: center;">
			<image src="../../static/icon_mima.png" style="width: 36rpx;height: 36rpx;margin-right: 8rpx;"></image>
			<input placeholder="请再次输入密码" maxlength="18" v-model="repsd" password placeholder-style="color:#BABEC0" />
		</view>
		<view style="margin-top: 140rpx;">
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
	import CryptoJS from "crypto-js";
	export default {
		data() {
			return {
				data: {
					mobile: '',
					code: '',
					password: '',
					inviter: 0,
				},
				repsd: '',
				verificationShow: true,
				time: 60,
			};
		},
		methods: {
			register() {
				let that = this;
				console.log(that.data)
				console.log(that.repsd)
				if (that.data.password.length == 0 && that.repsd.length == 0) {
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
						uni.showLoading({
							title: '加载中，请稍后',
							mask: true
						});
						uni.request({
							url: '/api/admin/user/register',
							method: 'POST',
							data: {
								mobile: that.data.mobile,
								code: that.data.code,
								inviter: that.data.inviter
							},
							success: (res) => {
								uni.hideLoading();
								if (res.statusCode == 200) {
									if (res.data.code == 0) {
										uni.navigateTo({
											url: 'download'
										})
									} else {
										uni.hideLoading();
										uni.showToast({
											icon: 'none',
											title: res.data.message
										})
									}
								} else {
									uni.hideLoading();
									uni.showToast({
										icon: 'none',
										title: '网络异常，请稍后再试'
									})
								}
							},
							fail: (res) => {
								uni.hideLoading();
								uni.showToast({
									icon: 'none',
									title: '网络异常，请稍后再试'
								})
							}
						});
					}
				} else {
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
					} else if (that.data.password.length == 0) {
						uni.showToast({
							icon: 'none',
							title: '请输入登录密码'
						})
					} else if (that.data.password.length < 6 && that.data.password.length > 0) {
						uni.showToast({
							icon: 'none',
							title: '密码长度过短'
						})
					} else if (that.data.password != that.repsd) {
						uni.showToast({
							icon: 'none',
							title: '两次输入的密码不一致'
						})
					} else {
						uni.showLoading({
							title: '加载中，请稍后',
							mask: true
						});
						let data = that.encryption({
							data: that.data,
							param: ["password"],
						});
						uni.request({
							url: '/api/admin/user/register',
							method: 'POST',
							data: data,
							success: (res) => {
								if (res.statusCode == 200) {
									if (res.data.code == 0) {
										uni.hideLoading();
										uni.navigateTo({
											url: 'download'
										})
									} else {
										uni.hideLoading();
										uni.showToast({
											icon: 'none',
											title: res.data.message
										})
									}
								} else {
									uni.hideLoading();
									uni.showToast({
										icon: 'none',
										title: '网络异常，请稍后再试'
									})
								}
							},
							fail: (res) => {
								uni.hideLoading();
								uni.showToast({
									icon: 'none',
									title: '网络异常，请稍后再试'
								})
							}
						});
					}
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
								console.log(res.data.code)
								console.log(res.data.message)
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
			encryption(params) {
				let key = "gdscloudprisbest";
				const {
					data,
					type,
					param
				} = params;
				const result = JSON.parse(JSON.stringify(data));
				if (type === "Base64") {
					param.forEach((ele) => {
						result[ele] = btoa(result[ele]);
					});
				} else {
					param.forEach((ele) => {
						const data = result[ele];
						key = CryptoJS.enc.Latin1.parse(key);
						const iv = key;
						// 加密
						const encrypted = CryptoJS.AES.encrypt(data, key, {
							iv: iv,
							mode: CryptoJS.mode.CBC,
							padding: CryptoJS.pad.ZeroPadding,
						});
						result[ele] = encrypted.toString();
					});
				}
				return result;
			},
			isNumber(val) {
				// negative or positive
				return /^[-]?[\.\d]+$/.test(val);
			}
		},
		onLoad(option) {
			if (this.isNumber(option.inviter)) {
				this.data.inviter = option.inviter;
			} else {
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

	.line {
		width: 90%;
		/* 分隔线的长度 */
		margin: auto;
		height: 1px;
		/* 分隔线粗细 */
		text-align: center;
		font-size: 24rpx;
		color: #919397;
		background: #EFF0F1;
		margin-top: 80rpx;
		margin-bottom: 44rpx;
	}

	.line span {
		position: relative;
		top: -8px;
		background: #fff;
		padding: 0px 10px;
	}
</style>
