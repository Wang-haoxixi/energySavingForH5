<template>
	<view>
		<view style="display: flex;justify-content: space-between;align-items: center; padding: 30rpx;background-color: #FFFFFF;">
			<view style="display: flex; align-items: center;">
				<view class="_img" style="margin-right: 20rpx;">
					<image src="../../static/logo192x192.png" mode="" style="width:76rpx;height:76rpx;border-radius:20rpx"></image>
				</view>
				<view style="display: flex;flex-direction: column;padding-bottom: 14rpx;">
					<view style="font-size: 28rpx;padding-bottom: 12rpx;font-weight: 600;">组件宝</view>
					<view style="font-size: 20rpx;color:rgba(145,147,151,1);font-weight: 400;">成就数字人生</view>
				</view>
			</view>
			<view style="width:160rpx;height:50rpx;background:#46B28B;border-radius:30rpx;display:flex;justify-content: center;align-items: center;">
				<view style="font-size: 24rpx;color: #FFFFFF;font-weight: 600;" @tap="goApp()">APP内打开</view>
			</view>
		</view>
		<view class="baseContainer">
			<view class="title">
				<view>
					<view class="titlename">
						<view class="name">{{isEmpty(data.realName)?'暂无':data.realName}}</view>
						<image v-if="data.redness==1 && oss" class="levelImg" :src="oss+'icon_ican_newLevelOne.png'"></image>
						<image v-if="data.redness==2 && oss" class="levelImg" :src="oss+'icon_ican_newLevelTwo.png'"></image>
						<image v-if="data.redness==3 && oss" class="levelImg" :src="oss+'icon_ican_newLevelThree.png'"></image>
						<image v-if="data.redness==4 && oss" class="levelImg" :src="oss+'icon_ican_newLevelFour.png'"></image>
						<image v-if="data.redness==5 && oss" class="levelImg" :src="oss+'icon_ican_newLevelFive.png'"></image>
					</view>
					<view class="profession">
						{{isEmpty(profession)?'暂无':profession}}
					</view>
				</view>
				<image v-if="oss && data.avatar" class="avatar" mode="aspectFill" :src="data.avatar">
				</image>
			</view>
			<view class="content">
				<image v-if="oss" class="content-topimg" :src="oss + 'bg_find_card_tab.png'"></image>
				<view class="content-text">
					<view class="company">{{isEmpty(data.company)?'':data.company}}</view>
					<view class="tag"><text v-for="(item,index) in data.abilityTag" :key="index" v-if="index<3">{{item}} <text class="tag-point"
							 v-if="index<data.abilityTag.length-1 && index<2">·</text></text></view>
				</view>
			</view>
			<view class="end">
				<view class="location">
					<image v-if="oss" class="icon" :src="oss + 'icon_find_canner_location.png'"></image>
					<view class="text">{{isEmpty(data.workPlace)?'暂无':data.workPlace}}</view>
				</view>
				<view class="button-list">
					<view v-if="data.isFollowed == 0" class="attention" @tap="goApp()">+关注</view>
					<view v-if="data.isFollowed == 1" class="attentioned button" @tap="goApp()">已关注</view>
				</view>
			</view>
		</view>
		<view @tap="goApp" style="width: 670rpx;height:88rpx;background:#46B28B;border-radius:20rpx;display:flex;justify-content: center;align-items: center;margin: 0 auto;">
			<view style="font-size: 32rpx;color: #FFFFFF;" @tap="goApp()">APP内查看更多</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				modal: '',
				oss: 'https://woneng-oss.oss-cn-hangzhou.aliyuncs.com/wxapp/',
				data: '',
				profession: '',
			};
		},
		methods: {
			isEmpty(item) {
				if (item == undefined || item == '' || item == null) {
					return true;
				} else {
					return false;
				}
			},
			imageError(e) {
				let that = this;
				// console.log(that.card)
				if (that.avatar.substring(0, 2) == '//') {
					that.avatar = 'http:' + that.avatar;
				} else {
					that.avatar = '../../static/ic_launcher.png';
				}
			},
			modalhidden() {
				this.modal = '';
			},
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
			},
			goApp() {
				let that = this;
				var ua = navigator.userAgent.toLowerCase();
				var isWeixin = ua.indexOf('micromessenger') != -1;
				if (isWeixin) {
					this.modal = 'show';
				} else {
					let u = navigator.userAgent;
					let isAndroid = u.indexOf('Android') > -1; //安卓终端
					let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
					if (isAndroid) {
						// 安卓的scheme协议跳转
						window.location.href = 'personalshare://?id=' + that.data.userId;
						setTimeout(function() {
							let hidden = window.document.hidden || window.document.mozHidden || window.document.msHidden || window.document
								.webkitHidden;
							if (typeof hidden == 'undefined' || hidden == false) {
								window.location.href = 'https://android.myapp.com/myapp/detail.htm?apkName=com.govmade.ican';
							}
						}, 2000);
					}
					// IOS下的scheme协议跳转
					if (isIOS) {
						window.location.href = 'personalshare://?id=' + that.data.userId;
						setTimeout(function() {
							let hidden = window.document.hidden || window.document.mozHidden || window.document.msHidden || window.document
								.webkitHidden;
							if (typeof hidden == 'undefined' || hidden == false) {
								window.location.href = 'https://apps.apple.com/cn/app/id1508431149';
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
			// this.$http.get('admin/app/user/style/' + this.id).then(res => {
			// 	this.data = res.data.data
			// 	let industry = this.$isEmpty(this.data.industry) ? '' : this.data.industry.split('-')[0];
			// 	let position = this.$isEmpty(this.data.position) ? '' : ' · ' + this.data.position.split('-')[(this.data.position
			// 		.split('-').length - 1)];
			// 	this.profession = industry + position;
			// 	// this.data = res.data.data.card;
			// 	console.log(this.profession)
			// });
			uni.request({
				url: '/api/admin/app/user/style/' + option.id,
				success: res => {
					that.data = res.data.data;
					that.avatar = res.data.data.avatar;
					let industry = that.isEmpty(that.data.industry) ? '' : that.data.industry.split('-')[0];
					let position = that.isEmpty(that.data.position) ? '' : ' · ' + that.data.position.split('-')[(that.data.position
						.split('-').length - 1)];
					that.profession = industry + position;
				}
			});
		}
	};
</script>

<style lang="scss">
	page {
		background-color: #F4F5F6;
	}

	.baseContainer {
		margin: 40rpx;
		padding: 30rpx;
		background-color: #FFFFFF;
		border-radius: 30rpx;

		.icon {
			height: 40rpx;
			width: 40rpx;
		}

		.title {
			display: flex;
			justify-content: space-between;
			align-items: center;

			.titlename {
				display: flex;

				.name {
					font-size: 32rpx;
					line-height: 44rpx;
					color: #303031;
					font-weight: bold;
				}

				.levelImg {
					margin-left: 10rpx;
					width: 40rpx;
					height: 44rpx;
				}
			}

			.profession {
				margin-top: 10rpx;
				color: #666769;
				font-size: 24rpx;
				line-height: 34rpx;
			}

			.avatar {
				width: 96rpx;
				height: 96rpx;
				border-radius: 20rpx;
			}
		}

		.content {
			border-radius: 0 0 20rpx 20rpx;
			width: 610rpx;
			margin-top: 30rpx;
			background-color: #F4F5F6;

			.content-topimg {
				width: 610rpx;
				height: 40rpx;
			}

			.content-text {
				padding: 0 30rpx 30rpx;

				.company {
					color: #666769;
					font-size: 28rpx;
					line-height: 40rpx;
				}

				.tag {
					margin-top: 20rpx;
					color: #919397;
					font-size: 24rpx;
					line-height: 34rpx;

					.tag-point {
						margin: 0 12rpx;
					}
				}
			}
		}

		.end {
			margin-top: 40rpx;
			display: flex;
			justify-content: space-between;

			.location {
				display: flex;
				align-items: center;

				.text {
					margin-left: 10rpx;
					font-size: 28rpx;
					line-height: 40rpx;
					color: #666769;
				}
			}

			.button-list {
				display: flex;
				align-items: center;
				font-size: 28rpx;
				line-height: 40rpx;

				.share {
					color: #919397;
					font-weight: bold;
					width: 136rpx;
					background-color: #F4F5F6;
					border-radius: 33rpx;
					text-align: center;
					padding: 8rpx 0;
				}

				.attention {
					display: flex;
					align-items: center;
					justify-content: center;
					margin-left: 20rpx;
					color: #FFFFFF;
					font-weight: bold;
					width: 136rpx;
					height: 56rpx;
					background-color: #46B28B;
					border-radius: 33rpx;
					text-align: center;
				}

				.attentioned {
					margin-left: 20rpx;
					color: #919397;
					font-weight: bold;
					width: 136rpx;
					background-color: #F4F5F6;
					border-radius: 33rpx;
					text-align: center;
					padding: 8rpx 0;
				}
			}
		}
	}

	.singleline {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>
