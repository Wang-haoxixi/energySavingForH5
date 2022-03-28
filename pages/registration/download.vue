<template>
	<view>
		<image src="../../static/welcome.png" mode="aspectFill" style="width: 750rpx;height: 100vh;"></image>
		<view style="width: 750rpx;position:fixed;bottom: 110rpx;display: flex;flex-direction: column;align-items: center;">
			<view style="background-color: #FDF6ED;width: 480rpx;height: 88rpx;border:4rpx solid #FFFFFF;border-radius: 20rpx;color: #F74437;font-size: 32rpx;text-align: center;line-height: 88rpx;border: 4rpx solid #FD7D58;"
			 @tap="download">立即下载</view>
			<view style="font-size: 28rpx;line-height: 52rpx;color: #FFFFFF;margin-top: 80rpx;opacity:0.6;">组件宝成就知识网红</view>
		</view>
		<view class="cu-modal" :class="modal" @tap="modalhidden">
			<view class="cu-dialog" style="float: right;background-color: transparent;height:160rpx;width: 409rpx;margin-right: 30rpx;margin-top: 10rpx;">
				<view class="bg-img" style="background-image: url('../../static/tc-open@3x.png');height:160rpx;width: 409rpx;margin-right: 30rpx;"></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				modal: ''
			}
		},
		methods: {
			modalhidden() {
				this.modal = '';
			},
			download() {
				let that = this;
				var ua = navigator.userAgent.toLowerCase();
				var isWeixin = ua.indexOf('micromessenger') != -1;
				if (isWeixin) {
					that.modal = 'show';
				} else {
					let u = navigator.userAgent;
					let isAndroid = u.indexOf('Android') > -1; //安卓终端
					let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
					if (isAndroid) {
						// 安卓的scheme协议跳转
						window.location.href = 'https://woneng-oss.oss-cn-hangzhou.aliyuncs.com/app/woneng.apk';
					}
					// IOS下的scheme协议跳转
					if (isIOS) {
						window.location.href = 'https://apps.apple.com/cn/app/id1508431149';
					}
				}
			}
		}
	}
</script>

<style>
</style>
