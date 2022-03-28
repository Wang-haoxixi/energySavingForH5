<template>
	<view>
		<view class="title">
			{{materialDetail.materialName}}
		</view>
		<view class="subtitle">
			<view>{{materialDetail.createUser}} | {{materialDetail.createTime}}</view>
			<view><text class="cuIcon-attention"></text> <text class="text-df" style="margin-left: 10rpx;">{{materialDetail.views}}
				</text></view>
		</view>
		<parser class="parser" :html="materialDetail.materialContent" :tag-style="tagStyle" img-mode="widthFix"></parser>
		<view class="tips">
			<text class='cu-tag round' style="flex: none;color: #F74437;background-color: #F744371A;border-radius: 10rpx;" v-for="(item,index) in materialDetail.tag"
			 :key="index">#{{ item }}</text>
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
		methods: {},
		onLoad(option) {
			uni.showLoading({
				title: '材料加载中'
			});
			let that = this;
			uni.request({
				url: '/api/dms/material/' + option.materialId,
				header: {
					'Authorization': "Bearer " + option.token,
				},
				success: (res) => {
					console.log(res)
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

	.parser {
		padding: 0 40rpx;
		margin-top: 40rpx;
	}

	.title {
		margin-top: 20rpx;
		font-weight: bold;
		line-height: 48rpx;
		font-size: 32rpx;
		color: #303031;
		padding: 0 40rpx;
	}

	.subtitle {
		margin-top: 20rpx;
		line-height: 48rpx;
		color: #919397;
		font-size: 24rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 40rpx;
	}

	.tips {
		display: flex;
		overflow-x: auto;
		padding: 40rpx;
		
	}
	.cu-tag+.cu-tag{
		margin-left: 20rpx;
	}
</style>
