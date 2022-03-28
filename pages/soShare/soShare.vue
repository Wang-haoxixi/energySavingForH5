<template>
	<view style="padding: 40rpx;">
		<view style="display: flex;align-items: center;">
			<view class="cu-avatar" style="background-image: url(../../static/logo192x192.png);height: 76rpx;width: 76rpx;border-radius: 20rpx;"></view>
			<view style="margin-left:20rpx;flex: 1;">
				<view style="color: #303031;font-size: 28rpx;font-weight: 600;">智慧节能宝</view>
				<view style="color: #919397;font-size: 20rpx;margin-top: 12rpx;">全域节能，科技臻善！</view>
			</view>
			<view style="background: #46B28B;color: #FFFFFF;font-size: 24rpx;padding: 8rpx 20rpx;font-weight: 600;border-radius: 25rpx;"
			 @tap="goApp">APP内打开</view>
		</view>
		<view style="background: linear-gradient(343deg, #47B48D 0%, #4FC9B3 100%);padding: 30rpx;margin-top: 60rpx;width: 670rpx;height: 432rpx;border-radius: 20rpx;">
			<view style="display: flex;align-items: center;">
				<image :src="avatar" style="height: 140rpx;width: 140rpx;border-radius: 20rpx;" @error="imageError"></image>
				<view style="margin-left:30rpx;">
					<view class="singleline" style="color: #FFFFFF;font-size: 28rpx;font-weight: 600;">{{ data.name }}</view>
					<view class="singleline" style="color: #FFFFFF;font-size: 24rpx;margin-top: 12rpx;opacity: 0.8;">用户：{{ data.creator }}</view>
					<view style="color: #FFFFFF;font-size: 22rpx;margin-top: 12rpx;opacity: 0.8;display: flex;">
						<view v-if="index<3" v-for="(item,index) in data.abilityTag" :key="index">
							{{item}}<text v-if="index < abilityTagSize-1" style="margin: 0 6rpx;">/</text>
						</view>
					</view>
				</view>
			</view>
			<view style="margin-top: 60rpx;display: flex;justify-content: space-between;padding: 0 18rpx;">
				<view style="color: #FFFFFF;display: flex;flex-direction: column;align-items: center;">
					<view style="font-weight: 600;font-size: 36rpx;">{{ data.creditWorth }}</view>
					<view style="font-size: 24rpx;margin-top: 10rpx;opacity: 0.8;">信用值</view>
				</view>
				<view style="color: #FFFFFF;display: flex;flex-direction: column;align-items: center;">
					<view style="font-weight: 600;font-size: 36rpx;">{{ data.bellAssets }}</view>
					<view style="font-size: 24rpx;margin-top: 10rpx;opacity: 0.8;">宝贝</view>
				</view>
				<view style="color: #FFFFFF;display: flex;flex-direction: column;align-items: center;">
					<view style="font-weight: 600;font-size: 36rpx;">{{ data.memberNum }}</view>
					<view style="font-size: 24rpx;margin-top: 10rpx;opacity: 0.8;">用户</view>
				</view>
				<view style="color: #FFFFFF;display: flex;flex-direction: column;align-items: center;">
					<view style="font-weight: 600;font-size: 36rpx;">{{ data.taskCount }}</view>
					<view style="font-size: 24rpx;margin-top: 10rpx;opacity: 0.8;">任务数</view>
				</view>
			</view>
			<view class="singleline" style="margin-top: 40rpx;font-size: 24rpx;color: #FFFFFF;opacity: 0.8;width: 100%;">
				{{ data.slogan}}
			</view>
		</view>
		<view class="cu-modal" :class="modal" @tap="modalhidden">
			<view class="cu-dialog" style="float: right;background-color: transparent;height:240rpx;width: 530rpx;margin-right: 30rpx;">
				<view class="bg-img" style="background-image: url('../../static/wxshare.png');height:240rpx;width: 530rpx;margin-right: 30rpx;">
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data: "",
				modal: "",
				avatar: "",
				abilityTagSize: "",
			}
		},
		methods: {
			modalhidden() {
				this.modal = "";
			},
			imageError(e) {
				let that = this;
				if (that.avatar.substring(0, 2) == "//"){
					that.avatar = "http:" + that.data.avatar;
				}
				else{
					that.avatar = "../../static/ic_launcher.png";
				}
			},
			goApp() {
				let that = this;
				var ua = navigator.userAgent.toLowerCase();
				var isWeixin = ua.indexOf('micromessenger') != -1;
				if (isWeixin) {
					this.modal = "show";
				} else {
					let u = navigator.userAgent;
					let isAndroid = u.indexOf("Android") > -1; //安卓终端
					let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
					if (isAndroid) {
						// 安卓的scheme协议跳转
						window.location.href = "organization://?id=" + that.data.orgId;
						setTimeout(function() {
							let hidden =
								window.document.hidden ||
								window.document.mozHidden ||
								window.document.msHidden ||
								window.document.webkitHidden;
							if (typeof hidden == "undefined" || hidden == false) {
								window.location.href = "https://android.myapp.com/myapp/detail.htm?apkName=com.govmade.ican";
							}
						}, 2000);
					}
					// IOS下的scheme协议跳转
					if (isIOS) {
						window.location.href = "organization://?id=" + that.data.orgId;
						setTimeout(function() {
							let hidden =
								window.document.hidden ||
								window.document.mozHidden ||
								window.document.msHidden ||
								window.document.webkitHidden;
							if (typeof hidden == "undefined" || hidden == false) {
								window.location.href = "https://apps.apple.com/cn/app/id1508431149"; // 此处跳转到苹果应用市场
							}
						}, 2000);
					}
				}
			}
		},
		onLoad(option) {
			let that = this;
			var ua = navigator.userAgent.toLowerCase();
			var isWeixin = ua.indexOf('micromessenger') != -1;
			if (isWeixin) {
				that.modal = 'show';
			}
			uni.request({
				url: '/api/admin/app/open/org/so/' + option.id,
				success: (res) => {
					console.log('res',res)
					// if (res.data.code != 0) {
					// 	uni.redirectTo({
					// 		url: '../404/404'
					// 	});
					// }
					that.data = res.data.data;
					that.avatar = res.data.data.avatar;
					that.abilityTagSize = res.data.data.abilityTag.length;
					if(that.abilityTagSize > 3){
						that.abilityTagSize = 3;
					}
				},
				fail: () => {
					uni.redirectTo({
						url: '../404/404'
					});
				}
			});
		}
	}
</script>

<style>
	page {
		background-color: white;
	}

	.singleline {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		width: 440rpx;
	}
</style>
