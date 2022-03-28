<template>
	<view style="padding: 40rpx;">
		<view class="text-black text-bold" style="font-size: 32rpx;line-height: 44rpx;">{{materialDetail.title}}</view>
		<view style="color: #919397;margin-top: 40rpx;font-size: 24rpx;line-height: 34rpx;">{{materialDetail.creatorRealName}}
			{{materialDetail.meetingTime.substring(0, 16)}}</view>
		<view style="display: flex;overflow-x: auto;margin-top: 30rpx;">
			<text class='cu-tag' style="flex: none;color: #919397;background-color: #F4F5F6;border-radius: 10rpx;font-size: 22rpx;padding: 6rpx 16rpx;"
			 v-for="(item,index) in materialDetail.tagKeyWords" :key="index">{{ item }}</text>
		</view>
		<view class="text-bold text-black" style="font-size: 32rpx;margin-top: 40rpx;line-height: 44rpx;">会议内容</view>
		<parser :html="materialDetail.meetingContent" :tag-style="tagStyle" img-mode="widthFix" style="margin-top: 30rpx;"></parser>
		<view class="text-bold text-black" style="font-size: 32rpx;margin-top: 40rpx;line-height: 44rpx;">会议总结</view>
		<parser :html="materialDetail.meetingCon" :tag-style="tagStyle" img-mode="widthFix" style="margin-top: 30rpx;"></parser>
		<view class="text-bold text-black" style="font-size: 32rpx;margin-top: 40rpx;line-height: 44rpx;">感想和困惑</view>
		<parser :html="materialDetail.thoughtsProblem" :tag-style="tagStyle" img-mode="widthFix" style="margin-top: 30rpx;"></parser>
		<view class="text-bold text-black" style="font-size: 32rpx;margin-top: 40rpx;line-height: 44rpx;">基本信息</view>
		<view style="font-size: 28rpx;line-height: 40rpx;color: #666769;margin-top: 30rpx;">
			<view>主持人：<text v-if="isEmpty(materialDetail.host.name)">无</text>{{materialDetail.host.name}}</view>
			<view>参会人：<text v-for="(item,index) in materialDetail.attendee.users" :key="index" style="padding-right: 10rpx;"> {{ item.name }}
				</text></view>
			<view>抄送人：<text v-if="isEmpty(materialDetail.receiver.users)"  style="padding-right: 10rpx;">无</text><text v-for="(item,index) in materialDetail.receiver.users" :key="index"
				 style="padding-right: 10rpx;"> {{ item.name }}
				</text></view>
			<view>会议地点：{{materialDetail.meetingLocation}}</view>
		</view>
	</view>
</template>

<script>
	import parser from "@/components/jyf-Parser/index"
	export default {
		components: {
			parser
		},
		data() {
			return {
				materialDetail: '',
				tagStyle: {
					body: 'line-height: 1.8;',
					img: 'background-size: contain|cover;width:100%;height:auto;'
				}
			}
		},
		methods: {
			isEmpty(item) {
				if (item == undefined || item == '' || item == null) {
					return true;
				} else {
					return false;
				}
			}
		},
		onLoad(option) {
			uni.showLoading({
				title: '纪要加载中'
			});
			let that = this;
			uni.request({
				url: '/api/dms/meeting_summary/' + option.summaryId,
				header: {
					'Authorization': "Bearer " + option.token,
				},
				success: (res) => {
					if (res.data.code != 0) {
						uni.redirectTo({
							url: '../404/404'
						});
					}
					that.materialDetail = res.data.data;
					if (that.materialDetail.creatorAvatar == '') {
						that.materialDetail.creatorAvatar = '../../static/icon_def_avatar.png'
					}
					uni.hideLoading();
				}
			});
		}
	}
</script>

<style>
	page {
		background-color: #fff;
	}

	parser {
		line-height: 1.8;
	}
</style>
