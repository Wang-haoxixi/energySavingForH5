<template>
	<view style="background-color: #fff;">
		<parser class="padding" :html="html" :tag-style="tagStyle"></parser>
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
				html: '',
				tagStyle: {
					body: 'line-height: 1.8;',
					img: 'background-size: contain|cover;width:100%;height:auto;'
				}
			}
		},
		onLoad(option) {
			console.log(option);
			uni.showLoading({
				title: '加载中'
			});
			let that = this;
			if (option.type == "activityDetail") {
				uni.request({
					url: '/api/qms/conference_publish/' + option.activityId,
					header: {
						'Authorization': "Bearer " + option.token,
					},
					success: (res) => {
						if (res.data.code != 0) {
							uni.redirectTo({
								url: '../404/404'
							});
						}
						that.html = res.data.data.details;
						uni.hideLoading();
					}
				});
			}
		}
	}
</script>

<style>

</style>
