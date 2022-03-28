<template>
	<view style="background-color: #fff;">
		<view class="solid-bottom text-xl padding">
			<text class="text-black text-bold">{{articleDetail.title}}</text>
		</view>
		<view class="text-xl padding" style="padding-bottom: 0;">
			<view class="cu-avatar round" :style="{backgroundImage:'url(' + articleDetail.creatorAvatar + ')'}"></view>
			<text class="text-lg padding">{{articleDetail.creatorName}}</text>
			<view class="text-df text-grey">{{articleDetail.createTime}}
				<text class="padding"><text class="cuIcon-attention"></text> <text class="text-df">{{articleDetail.views}}人浏览
					</text></text></view>
		</view>
		<parser class="padding" :html="articleDetail.content" :tag-style="tagStyle" img-mode="widthFix"></parser>
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
				articleDetail: '',
				tagStyle: {
					body: 'line-height: 1.8;',
					img: 'background-size: contain|cover;width:100%;height:auto;'
				}
			}
		},
		methods: {},
		onLoad(option) {
			uni.showLoading({
				title: '文章加载中'
			});
			let that = this;
			uni.request({
				url: '/api/cms/info_article/index/' + option.articleId,
				header: {
					'Authorization': "Bearer " + option.token,
				},
				success: (res) => {
					that.articleDetail = res.data.data;
					if(that.articleDetail.creatorAvatar == ''){
						that.articleDetail.creatorAvatar = '../../static/icon_def_avatar.png'
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
