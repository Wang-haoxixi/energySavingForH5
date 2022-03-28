<template>
	<view>
		<view class="bg-red nav" style="padding: 20rpx;height: 108rpx;" @tap="goApp">
			<view class="" style="float: right;margin-top: 16rpx;">
				打开APP
			</view>
			<image src="../../static/icon.png" style="width: 68rpx;height: 68rpx;float: left;"></image>
			<view style="margin-left: 90rpx;">
				<view>
					组件宝
				</view>
				<view class="text-sm">
					成就数字人生
				</view>
			</view>
		</view>
		<view class="bg-img" :style="[{ backgroundImage:'url(' + data.imageUrl + ')' }]" style="height: 750rpx;"></view>
		<view class="padding">
			<view class='cu-tag round text-red' style="float: right;background-color: #FEF0F4;">更多优惠></view>
			<view class="text-red text-bold text-lg">
				{{data.price}}贝 <text class="margin-left-xs margin-right-xs">/</text><text>{{data.cash}}元</text>
			</view>
			<view class="text-xs padding-top-xs" style="color: #999;">
				价格：{{data.price}}贝
			</view>
			<view class="padding-top padding-bottom-xs">
				{{data.name}}
			</view>
		</view>
		<view class="padding-xs" style="background-color: #F8F8F8;">

		</view>
		<view class="padding-left padding-top-xs padding-bottom-xs" style="color: #FF782C;background-color: #FFEFDF;border-top: 5px;">
			<text class="cuIcon-notice" /><text class="text-xs margin-left-xs">软件类和报告类产品不支持退款</text>
		</view>
		<view class="padding-xs" style="background-color: #F8F8F8;">

		</view>
		<view class="padding">
			<view class="">
				组织推荐
				<view class="padding-top">
					<view class="grid margin-bottom text-center col-3">
						<view class="padding-xs" v-for="(item,index) in data.orgProducts" :key="index" @tap="newProduct(item.id)">
							<view class="bg-img" :style="[{ backgroundImage:'url(' + item.imageUrl + ')' }]" style="height: 200rpx;width: 200rpx;"></view>
							<view class="singleline margin-top">{{item.name}}</view>
							<view class="text-red">{{item.price}}贝</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="padding-xs" style="background-color: #F8F8F8;">

		</view>
		<view class="flex padding align-center">
			<view class="bg-img text-xs" :style="[{ backgroundImage:'url(' + data.orgImg + ')' }]" style="height: 80rpx;width: 80rpx;"></view><text
			 class="margin-left">{{data.orgName}}</text>
		</view>
		<view class="text-center padding-xs text-xs" style="color: #666;background-color: #F8F8F8;">
			-- 图文详情 --
		</view>
		<parser class="padding" :html="data.description" :tag-style="tagStyle" img-mode="widthFix"></parser>
		<view class="padding-xs" style="background-color: #F8F8F8;">

		</view>
		<view class="padding">
			<view class="text-center">
				你可能还喜欢
				<view class="padding-top">
					<view class="grid margin-bottom text-center col-3">
						<view class="padding-xs" v-for="(item,index) in data.recommendProducts" :key="index" @tap="newProduct(item.id)">
							<view class="bg-img" :style="[{ backgroundImage:'url(' + item.imageUrl + ')' }]" style="height: 200rpx;width: 200rpx;"></view>
							<view class="singleline margin-top">{{item.name}}</view>
							<view class="text-red">{{item.price}}贝</view>
						</view>
					</view>
				</view>
			</view>
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
				id: '',
				data: '',
				tagStyle: {
					body: 'line-height: 1.8;',
					img: 'background-size: contain|cover;width:100%;height:auto;'
				}
			}
		},
		methods: {
			newProduct(id) {
				uni.navigateTo({
					url: 'productShare?id=' + id
				})
				console.log(id)
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
						window.location.href = "productstyle://?id=" + that.id;
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
						window.location.href = "productstyle://?id=" + that.id;
						setTimeout(function() {
							let hidden =
								window.document.hidden ||
								window.document.mozHidden ||
								window.document.msHidden ||
								window.document.webkitHidden;
							if (typeof hidden == "undefined" || hidden == false) {
								window.location.href = "https://apps.apple.com/cn/app/id1508431149";
							}
						}, 2000);
					}
				}
			}
		},
		onLoad(option) {
			let that = this;
			that.id = option.id
			uni.request({
				url: '/api/dms/product/open/' + option.id,
				success: (res) => {
					that.data = res.data.data
				}
			})
		}
	}
</script>

<style>
	page {
		background-color: #fff;
	}

	.singleline {
		display: inline-block;
		white-space: nowrap;
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>
