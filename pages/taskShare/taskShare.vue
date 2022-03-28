<template>
	<view style="padding: 40rpx;">
		<view style="display: flex;align-items: center;">
			<image :src="avatar" style="height: 76rpx;width: 76rpx;border-radius: 20rpx;" @error="imageError"></image>
			<view style="margin-left: 20rpx;flex: 1;">
				<view style="color: #303031;display: flex;align-items: flex-end;">
					<view style="font-weight: 600;font-size: 28rpx;">{{data.realName}}</view>
					<view v-if="data.orgIdentity==2" style="margin-left: 10rpx;border-radius: 4rpx;color: #F74437;background-color: #FFEDEA;font-size: 20rpx;line-height: 28rpx;padding: 0 10rpx;">用户</view>
					<view v-if="data.orgIdentity==3" style="margin-left: 10rpx;border-radius: 4rpx;background-color: #EBF2FF;color: #2F80FF;font-size: 20rpx;line-height: 28rpx;padding: 0 10rpx;">知事</view>
				</view>
				<view style="color: #919397;font-size: 20rpx;margin-top: 12rpx;">{{ data.cooperationTask.createTime}}</view>
			</view>
			<view style="font-size: 24rpx;background-color: #F74437;color: #FFFFFF;font-weight: 600;border-radius: 25rpx;height: 50rpx;width: 160rpx;line-height: 50rpx;text-align: center;"
			 @tap="goApp">
				APP内打开
			</view>
		</view>
		<view style="margin-top: 40rpx;background-color: #EBF2FF;color: #2F80FF;font-size: 24rpx;padding: 8rpx 20rpx;border-radius: 10rpx;">
			来自：{{ data.org.name }}
		</view>
		<view style="margin-top: 40rpx;color: #303031;font-size: 28rpx;">
			{{ data.cooperationTask.description }}
		</view>
		<view style="margin-top: 40rpx;display: flex;font-size: 20rpx;color: #919397;">
			<!-- <view style="display: flex;align-items: center;" @tap="goApp">
				<image src="../../static/icon_pinglun.png" style="height: 32rpx;width: 32rpx;"></image>
				<view style="margin-left: 8rpx;line-height: 32rpx;">{{ data.commentNum}}</view>
			</view>
			<view style="display: flex;align-items: center;margin-left: 56rpx;" @tap="goApp">
				<image src="../../static/icon_dianzan.png" style="height: 32rpx;width: 32rpx;"></image>
				<view style="margin-left: 8rpx;line-height: 32rpx;">{{ data.likeNum}}</view>
			</view> -->
		</view>
		<view style="margin-top: 40rpx;">
			<view style="color: #303031;font-size: 32rpx;font-weight: 600;">
				评论
			</view>
			<view style="padding: 20rpx;margin-top: 20rpx;background-color: #F4F5F6;color: #919397;border-radius: 10rpx;font-size: 28rpx;"
			 @tap="goApp">
				对任务进行点评…
			</view>
		</view>
		<view style="padding: 30rpx;margin-top: 60rpx;background-color: #F4F5F6;border-radius: 30rpx;">
			<view>
				<view style="display: flex;align-items: center;">
					<image :src="avatar" style="height: 76rpx;width: 76rpx;border-radius: 20rpx;"></image>
					<text style="color: #303031;font-weight: 600;line-height: 76rpx;margin-left: 20rpx;">{{ data.realName }}</text>
				</view>
				<!-- <view style="width: 54rpx;height: 12rpx;margin-top: 28rpx;margin-left: 12rpx;">
					<image src="../../static/icon_up.png" mode="aspectFit" style="width: 54rpx;height: 12rpx;"></image>
				</view> -->
				<view style="padding: 20rpx;background-image:url(../../static/bg_task.png);background-size: cover;border-radius: 10rpx;color: #919397;font-size: 28rpx;padding: 32rpx 20rpx; width: 610rpx;height: 92rpx;margin-top: 28rpx;">
					我在组件宝APP邀请您参与任务协作
				</view>
			</view>
			<view style="margin-top: 40rpx;display: flex;align-items: center;">
				<image :src="orgavatar" style="height: 48rpx;width: 48rpx;border-radius: 10rpx;" @error="imageOrgError"></image>
				<!-- <view class="cu-avatar" :style="{backgroundImage: 'url('+ data.org.avatar +')'}" style=";height: 48rpx;width: 48rpx;border-radius: 10rpx;"></view> -->
				<view style="margin-left: 10rpx;color: #919397;font-size: 24rpx;flex: 1;">{{ data.org.name }}</view>
				<view style="padding:8rpx 20rpx;background-color: #2F80FF;line-height: 34rpx;border-radius: 25rpx;color: #FFFFFF;font-size: 24rpx;"
				 @tap="goApp">了解一下</view>
			</view>
		</view>
		<view style="margin-top: 40rpx;color: #919397;font-size: 24rpx;">关注微信公众号「组件宝VIP」，让我们一起成功吧。</view>
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
				avatar: "",
				modal: "",
				orgavatar: "",
			}
		},
		methods: {
			modalhidden() {
				this.modal = "";
			},
			imageError(e) {
				let that = this;
				if (that.avatar.substring(0, 2) == "//") {
					that.avatar = "http:" + that.avatar;
				} else {
					that.avatar = "../../static/ic_launcher.png";
				}
			},
			imageOrgError(e) {
				let that = this;
				if (that.orgavatar.substring(0, 2) == "//") {
					that.orgavatar = "http:" + that.orgavatar;
				} else {
					that.orgavatar = "../../static/ic_launcher.png";
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
						window.location.href = "organizationstyle://?taskId=" + that.data.cooperationTask.id + "&taskName=" + that.data.cooperationTask
							.title + "&id=" + that.data.cooperationTask.orgId;
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
						window.location.href = "organizationstyle://?taskId=" + that.data.cooperationTask.id + "&taskName=" + that.data.cooperationTask
							.title + "&id=" + that.data.cooperationTask.orgId;
						setTimeout(function() {
							let hidden =
								window.document.hidden ||
								window.document.mozHidden ||
								window.document.msHidden ||
								window.document.webkitHidden;
							if (typeof hidden == "undefined" || hidden == false) {
								window.location.href = "https://apps.apple.com/cn/app/id1508431149";
							}
							console.log("https://apps.apple.com/cn/app/id1508431149");
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
				url: '/api/qms/cooperation_list/select_share?taskId=' + option.id,
				success: (res) => {
					if (res.data.code != 0) {
						uni.redirectTo({
							url: '../404/404'
						});
					}
					that.data = res.data.data;
					that.avatar = res.data.data.avatar;
					that.orgavatar = res.data.data.org.avatar;
				},
				fail: () => {
					uni.redirectTo({
						url: '../404/404'
					});
				}
			})
		}
	}
</script>

<style>
	page {
		background-color: #FFFFFF;
	}
</style>
