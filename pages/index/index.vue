<template>
	<view class="">
		<image src="../../static/loading-white.gif" class="gif-white response" style="height:480rpx"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				sendimg: '',
				token: '',
				articleId: '',
				type: ''
			};
		},
		methods: {
			getQueryVariable(variable) {
				var query = window.location.search.substring(1);
				// console.log(query)
				var vars = query.split('&');
				for (var i = 0; i < vars.length; i++) {
					var pair = vars[i].split('=');
					if (pair[0] == variable) {
						return pair[1];
					}
				}
				return false;
			}
		},
		onLoad() {
			let that = this;
			that.type = that.getQueryVariable('type');
			if (that.getQueryVariable('type') == 'article') { //文章
				uni.redirectTo({
					url: '../article/article?token=' + that.getQueryVariable('token') + '&articleId=' + that.getQueryVariable(
						'articleId')
				});
			} else if (that.getQueryVariable('type') == 'material') { //材料
				uni.redirectTo({
					url: '../material/material?token=' + that.getQueryVariable('token') + '&materialId=' + that.getQueryVariable(
						'materialId')
				});
			} else if (that.getQueryVariable('type') == 'summary') { //纪要
				uni.redirectTo({
					url: '../summary/summary?token=' + that.getQueryVariable('token') + '&summaryId=' + that.getQueryVariable(
						'summaryId')
				});
			} else if (that.getQueryVariable('type') == 'service') { //客服
				uni.redirectTo({
					url: '../service/service?token=' + that.getQueryVariable('token') + '&imgurl=' + that.getQueryVariable('imgurl')
				});
			} else if (that.getQueryVariable('type') == 'agreement') { //组件宝服务协议
				uni.redirectTo({
					url: '../agreement/agreement'
				});
			} else if (that.getQueryVariable('type') == 'agreementEnergySaving') { //智慧节能宝服务协议
				uni.redirectTo({
					url: '../agreementEnergySaving/agreementEnergySaving'
				});
			} else if (that.getQueryVariable('type') == 'privacy') { //组件宝隐私政策
				uni.redirectTo({
					url: '../privacy/privacy'
				});
			} else if (that.getQueryVariable('type') == 'privacyEnergySaving') { //智慧节能宝隐私政策
				uni.redirectTo({
					url: '../privacyEnergySaving/privacyEnergySaving'
				});
			} else if (that.getQueryVariable('type') == 'strategy') { //挖贝攻略
				uni.redirectTo({
					url: '../strategy/strategy'
				});
			} else if (that.getQueryVariable('type') == 'canerShare') { //用户分享
				uni.redirectTo({
					url: '../canerShare/canerShare?id=' + that.getQueryVariable('id')
				});
			} else if (that.getQueryVariable('type') == 'soShare') { //组织分享
				uni.redirectTo({
					url: '../soShare/soShare?id=' + that.getQueryVariable('id')
				});
			} else if (that.getQueryVariable('type') == 'productShare') { //产品分享
				uni.redirectTo({
					url: '../productShare/productShare?id=' + that.getQueryVariable('id')
				});
			} else if (that.getQueryVariable('type') == 'taskShare') { //任务分享
				uni.redirectTo({
					url: '../taskShare/taskShare?id=' + that.getQueryVariable('id')
				});
			} else if (that.getQueryVariable('type') == 'creatMaterial') { //创建材料
				uni.redirectTo({
					url: '../creatMaterial/creatMaterial?token=' + that.getQueryVariable('token')
				});
			} else if (that.getQueryVariable('type') == 'editMaterial') { //编辑材料
				uni.redirectTo({
					url: '../editMaterial/editMaterial?token=' + that.getQueryVariable('token') + '&materialId=' + that.getQueryVariable(
						'materialId')
				});
			} else if (that.getQueryVariable('type') == 'activityDetail') { //活动详情
				uni.redirectTo({
					url: '../Richtext/Richtext?token=' + that.getQueryVariable('token') + '&activityId=' + that.getQueryVariable(
						'activityId') + '&type=activityDetail'
				});
			} else if (that.getQueryVariable('type') == 'aboutIcan') { //关于组件宝
				uni.redirectTo({
					url: '../aboutIcan/aboutIcan'
				});
			} else if (that.getQueryVariable('type') == 'rules') { //关于规则
				uni.redirectTo({
					url: '../rules/rules'
				});
			} else if (that.getQueryVariable('type') == 'registration') { //注册
				uni.redirectTo({
					url: '../registration/account?inviter=' + that.getQueryVariable('inviteID')
				});
			} else if (that.getQueryVariable('type') == 'download') { //下载
				uni.redirectTo({
					url: '../registration/download'
				});
			} else if (that.getQueryVariable('type') == 'userFaith') { //关于组件宝信用值
				uni.redirectTo({
					url: '../userFaith/userFaith'
				});
			} else if (that.getQueryVariable('type') == 'userFaithEnergySaving') { //关于智慧节能宝信用值
				uni.redirectTo({
					url: '../userFaithEnergySaving/userFaithEnergySaving'
				});
			}
			else if (that.getQueryVariable('type') == 'preview') { //组件宝红度规则
				uni.redirectTo({
					url: '../preview/preview'
				});
			} else if (that.getQueryVariable('type') == 'previewEnergySaving') { //智慧节能宝红度规则
				uni.redirectTo({
					url: '../previewEnergySaving/previewEnergySaving'
				});
			}
			else if (that.getQueryVariable('type') == 'valuation') { //组件宝估值规则
				uni.redirectTo({
					url: '../valuation/valuation'
				});
			}
			else if(that.getQueryVariable('type') == 'getActivityCode') { //获取活动核销码
				uni.redirectTo({
					url: '../getActivityCode/getActivityCode?id=' + that.getQueryVariable('id')
				})
			}
			else if(that.getQueryVariable('type') == 'examination') { //证书查询
				uni.redirectTo({
					url: '../examination/examination?id=' + that.getQueryVariable('id')
				})
			}
			else if(that.getQueryVariable('type') == 'partnerApply') { //合伙人申请
				uni.redirectTo({
					url: '../partnerApply/partnerApply'
				})
			}
			else if(that.getQueryVariable('type') == 'energyCompany') { //公司介绍专题页
				uni.redirectTo({
					url: '../energyCompany/energyCompany'
				})
			}
			else if(that.getQueryVariable('type') == 'energyAPP') { //智慧节能宝APP专题页
				uni.redirectTo({
					url: '../energyAPP/energyAPP'
				})
			}
			else if(that.getQueryVariable('type') == 'energySkill') { //节能技术专题页
				uni.redirectTo({
					url: '../energySkill/energySkill'
				})
			}
			else if(that.getQueryVariable('type') == 'energyOfficer') { //首席节能官专题页
				uni.redirectTo({
					url: '../energyOfficer/energyOfficer'
				})
			}
		}
	};
</script>

<style>
	.cu-avatar {
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	page {
		padding-top: 100rpx;
		background-color: #fff;
		padding-bottom: 100rpx;
	}
</style>
