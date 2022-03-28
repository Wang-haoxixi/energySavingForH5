<template>
	<view class="container">
		<view id="navigation" :style="getNavigationStyle()">
			<!-- <image class="icon-back" src="../../static/icon_back.png" @tap="goBack"></image> -->
			<view class="icon-back">
				<image class="icon-back-inner" :style="{opacity: 1- transparency}" src="../../static/icon_back.png" @tap="goBack"></image>
				<image class="icon-back-inner" :style="{opacity: transparency}" src="../../static/icon_back_black.png" @tap="goBack"></image>
			</view>
			<view class="title">组件宝合伙人</view>
			<view class="icon-back"></view>
		</view>
		<scroll-view ref="scroll" id="content" scroll-y :style="{height: scrollHeight + 'px', paddingBottom: bottomHeight + 'px'}" @scroll="pageScroll">
			<view >
				<view id="description" :class="{limit: limitShow}">
					<p class="description-text">组件宝，以数据治理架构师为使命，以定义未来为愿景，聚焦“数字生态系统+数字技能提升+百万合伙人+智慧群组+行业运营”五位一体的数据资产运营，打造智慧城市、数字政府、智慧园区、智慧企业等项目创新标杆，引领组织变革、数据治理、流程再造新浪潮。</p>
					<image class="description-image" src="../../static/bg_recruit_description.png"></image>
					<view class="show-limit-button" @tap="limitToggle">
						<image v-show="limitShow" class="show-limit-icon" src="../../static/icon_show_toggle_up.png"></image>
						<image v-show="!limitShow" class="show-limit-icon" src="../../static/icon_show_toggle_down.png"></image>
					</view>
				</view>
				<view id="detail">
					<h3 class="detail-title">申请合伙人</h3>
					<view id="apply" class="detail-content">
						<view v-for="apply in applyList" :key="apply.id" class="apply-item">
							<image class="apply-item-image" :src="getApplyIcon(apply.value)"></image>
							<view class="apply-item-text">
								<p class="text-name">{{apply.label}}</p>
								<p class="text-desc">{{getApplyDescript(apply.value)}}</p>
							</view>
							<view class="apply-item-button" @tap="handleApply(apply)">立即申请</view>
						</view>
					</view>
					<h3 class="detail-title">您将获得</h3>
					<view id="harvest" class="detail-content">
						<view id="harvest-title">
							<view class="harvest-title-item" :class="{checked: harvestChecked == 0}" @tap="harvestChange(0)">
								<image class="harvest-title-item-image" src="../../static/icon_harvest_support.png"></image>
								<view class="harvest-title-item-text">强大支撑体系</view>
							</view>
							<view class="harvest-title-item" :class="{checked: harvestChecked == 1}" @tap="harvestChange(1)">
								<image class="harvest-title-item-image" src="../../static/icon_harvest_guarantee.png"></image>
								<view class="harvest-title-item-text">多重权益保障</view>
							</view>
							<view class="harvest-title-item" :class="{checked: harvestChecked == 2}" @tap="harvestChange(2)">
								<image class="harvest-title-item-image" src="../../static/icon_harvest_overall.png"></image>
								<view class="harvest-title-item-text">多种合作方向</view>
							</view>
						</view>
						<view id="harvest-content">
							<image class="harvest-content-image" style="height: 350rpx" v-show="harvestChecked == 0" src="../../static/bg_harvest_support.png"></image>
							<image class="harvest-content-image" style="height: 252rpx" v-show="harvestChecked == 1" src="../../static/bg_harvest_guarantee.png"></image>
							<image class="harvest-content-image" style="height: 476rpx" v-show="harvestChecked == 2" src="../../static/bg_harvest_overall.png"></image>
						</view>
					</view>

					<h3 class="detdail-title">多重产品代理</h3>
					<view id="agent" class="detail-content">
						<view class="agent-item" style="background-image: url('../../static/bg_dna.png')">数据基因DNA</view>
						<view class="agent-item" style="background-image: url('../../static/bg_ods.png')">组织数据体系ODS</view>
						<view class="agent-item" style="background-image: url('../../static/bg_iep.png')">智慧赋能平台IEP</view>
						<view class="agent-item" style="background-image: url('../../static/bg_dips.png')">水巢DIPS</view>
					</view>

					<h3 class="detail-title">组件宝合作伙伴</h3>
					<view id="partner" class="detail-content">
						<view class="partner-item">
							<image class="partner-item-image" src="../../static/pic_partner_1.png"></image>
							<view class="partner-item-name">仙桃大数据谷</view>
						</view>
						<view class="partner-item">
							<image class="partner-item-image" src="../../static/pic_partner_2.png"></image>
							<view class="partner-item-name">瑞麟天下</view>
						</view>
						<view class="partner-item">
							<image class="partner-item-image" src="../../static/pic_partner_3.png"></image>
							<view class="partner-item-name">组件宝衡阳公司</view>
						</view>
						<view class="partner-item">
							<image class="partner-item-image" src="../../static/pic_partner_4.png"></image>
							<view class="partner-item-name">重庆关爱儿童基金会</view>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
		<view class="bottom-button-area" :style="{paddingBottom: safeBottom + 'px'}">
			<view class="bottom-button-list">
				<view class="bottom-button-item" @tap="handleApplyValidate">立即申请</view>
			</view>
		</view>
	</view>
</template>

<script>
	import axios from 'axios'

	function setupWebViewJavascriptBridge(callback) {
		if (window.WebViewJavascriptBridge) {
			return callback(WebViewJavascriptBridge);
		}
		if (window.WVJBCallbacks) {
			return window.WVJBCallbacks.push(callback);
		}
		window.WVJBCallbacks = [callback];
		var WVJBIframe = document.createElement('iframe');
		WVJBIframe.style.display = 'none';
		WVJBIframe.src = 'wvjbscheme://__BRIDGE_LOADED__';
		document.documentElement.appendChild(WVJBIframe);
		setTimeout(function() {
			document.documentElement.removeChild(WVJBIframe)
		}, 0)
	}

	function getBridge(funName, dataJson, callback) {
		setupWebViewJavascriptBridge(function(bridge) {
			bridge.callHandler(funName, dataJson, function(response) {
				callback && callback(response);
			});
		});
	}
	export default {
		data() {
			return {
				applyList: [],
				applyInfoList: [{
						value: 'strategy',
						icon: '../../static/icon_partner_strategy.png',
						description: '组件宝平台城市超级APP运营中心'
					},
					{
						value: 'city',
						icon: '../../static/icon_partner_city.png',
						description: '组件宝地区中心代表'
					},
					{
						value: 'agent',
						icon: '../../static/icon_partner_agent.png',
						description: '产品服务代理'
					},
					{
						value: 'cause',
						icon: '../../static/icon_partner_cause.png',
						description: '以组件宝平台发展为共同事业目标'
					},
					{
						value: 'ordinary',
						icon: '../../static/icon_partner_ordinary.png',
						description: '组件宝用户即合伙人'
					},
				],
				harvestChecked: 0,
				limitShow: true,
				safeTop: 0,
				safeBottom: 0,
				bottomHeight: 0,
				scrollHeight: 0,
				changeHeight: 0,
				transparency: 0,
				init: false,
				promise: null,
				
			}
		},
		onReady() {
			if (this.init) {
				this.$nextTick(() =>{
					this.setHeight()
					this.setBottomHeight()
				})
			} else {
				this.setHeight()
				this.setBottomHeight()
				this.promise.then(res => {
					this.$nextTick(() =>{
						this.setHeight()
						this.setBottomHeight()
					})
				})
			}
			axios.get('https://api.woneng.net/admin/dict/type/recruit_partners_type').then(({
				data
			}) => {
				if (data.code === 0) {
					this.applyList = data.data
				}
			})
		},
		onLoad () {
			this.promise = new Promise((resolve, reject) => {
				getBridge('getSafeDistance', null, res => {
					this.init = true
					this.safeTop = JSON.parse(res).top
					this.safeBottom = JSON.parse(res).bottom
					resolve(res)
				})
			})
		},
		methods: {
			setHeight () {
				uni.createSelectorQuery()
					.in(this)
					.select('#navigation')
					.boundingClientRect()
					.select('#detail')
					.boundingClientRect()
					.exec(ret => {
						this.changeHeight = ret[1].top - ret[0].height + 20
						let height = uni.getSystemInfoSync().windowHeight;
						this.scrollHeight = height - ret[0].height
					});
			},
			setBottomHeight () {
				uni.createSelectorQuery()
					.in(this)
					.select('.bottom-button-area')
					.boundingClientRect()
					.exec(ret => {
						this.bottomHeight = ret[0].height
					});
			},
			getNavigationStyle () {
				let color = 255 * (1 - this.transparency)
				return {
					paddingTop: this.safeTop + 'px',
					backgroundColor: 'rgba(255, 255, 255, ' + this.transparency + ')',
					color: `rgb(${color}, ${color}, ${color})`
				}
			},
			pageScroll ({detail}) {
				if (this.changeHeight - 50 < detail.scrollTop) {
					this.transparency = (50 + detail.scrollTop - this.changeHeight) / 50
				} else {
					this.transparency = 0
				}
			},
			limitToggle() {
				this.limitShow = !this.limitShow
				this.$nextTick(() => {
					this.setHeight()
				})
			},
			getApplyIcon(value) {
				for (let i = 0; i < this.applyInfoList.length; i++) {
					if (this.applyInfoList[i].value == value) {
						return this.applyInfoList[i].icon
					}
				}
			},
			getApplyDescript(value) {
				for (let i = 0; i < this.applyInfoList.length; i++) {
					if (this.applyInfoList[i].value == value) {
						return this.applyInfoList[i].description
					}
				}
			},
			handleApply(apply) {
				getBridge('partnerApply', apply)
			},
			handleApplyValidate () {
				getBridge('partnerApplyValidate')
			},
			harvestChange(index) {
				this.harvestChecked = index
			},
			goBack () {
				getBridge('goBack')
			},
			share () {
				getBridge('share')
			}
		}
	}
</script>

<style lang="scss">
	.container {
		color: #303031;
		background-image: linear-gradient(to bottom right, #FF7152, #F74437);
	}
	
	#navigation {
		box-sizing: content-box;
		height: 88rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		.icon-back {
			position: relative;
			margin-left: 26rpx;
			height: 44rpx;
			width: 44rpx;
			.icon-back-inner {
				position: absolute;
				left: 0;
				top: 0;
				height: 100%;
			}
		}
		.icon-share {
			margin-right: 26rpx;
			height: 44rpx;
			width: 44rpx;
		}
		.title {
			
			height: 48rpx;
			line-height: 48rpx;
			font-size: 34rpx;
			
		}
	}

	#description {
		padding: 40rpx;
		font-size: 28rpx;
		color: #FFFFFF;
		
		>.description-text {
			line-height: 48rpx;
		}

		>.description-image {
			display: block;
			margin: 30rpx auto 0;
			height: 580rpx;
			width: 670rpx;
		}

		&.limit {
			.description-text {
				display: -webkit-box;
				height: 144rpx;
				overflow: hidden;
				text-overflow: ellipsis;
				-webkit-line-clamp: 3;
				-webkit-box-orient: vertical;
			}

			.description-image {
				display: none;
			}
		}

		.show-limit-button {
			margin-top: 8rpx;
			text-align: center;
			height: 40rpx;
			line-height: 40rpx;

			.show-limit-icon {
				margin: 0;
				height: 40rpx;
				width: 64rpx;

				&.show {
					transform: rotateX(180deg);
				}
			}
		}
	}

	#detail {
		background-color: #FFFFFF;
		border-radius: 40rpx 40rpx 0 0;
		padding: 40rpx 40rpx 0;

		.detail-title {
			font-size: 28rpx;
			height: 44rpx;
			line-height: 44rpx;
		}

		.detail-content {
			padding-bottom: 60rpx;
		}

		#apply {
			.apply-item {
				height: 180rpx;
				padding: 40rpx 0;
				display: flex;
				justify-content: space-between;
				align-items: center;
				border-bottom: solid 1rpx #EFF0F1;

				.apply-item-image {
					height: 100rpx;
					width: 100rpx;
					flex: 0 0 100rpx;
				}

				.apply-item-text {
					height: 100rpx;
					flex: 1;
					margin: 0 20rpx;

					.text-name {
						height: 44rpx;
						line-height: 44rpx;
						font-size: 32rpx;
					}

					.text-desc {
						margin-top: 14rpx;
						height: 34rpx;
						line-height: 34rpx;
						color: #666769;
						font-size: 24rpx;
					}
				}

				.apply-item-button {
					height: 50rpx;
					line-height: 50rpx;
					flex: 0 0 136rpx;
					border-radius: 25rpx;
					background-color: #FEECEA;
					color: #F74437;
					font-size: 24rpx;
					text-align: center;
				}
			}
		}

		#harvest {
			margin-top: 40rpx;

			#harvest-title {
				display: flex;
				// margin: 0 -40rpx;
				height: 120rpx;
				justify-content: space-around;
				align-items: center;

				.harvest-title-item {
					height: 120rpx;
					text-align: center;

					&.checked {
						.harvest-title-item-text {
							color: #F74437;
						}
					}

					.harvest-title-item-image {
						height: 76rpx;
						width: 76rpx;
					}

					.harvest-title-item-text {
						margin-top: 10rpx;
						height: 34rpx;
						line-height: 34rpx;
						font-size: 24rpx;
						color: #919397;
					}
				}
			}

			#harvest-content {
				.harvest-content-image {
					margin: 40rpx 0 0 0;
					width: 100%;
				}
			}
		}

		#agent {
			margin-top: 40rpx;
			display: grid;
			grid-template-columns: 1fr 1fr;
			grid-template-rows: 128rpx 128rpx;
			grid-gap: 30rpx 30rpx;

			.agent-item {
				line-height: 128rpx;
				padding-left: 30rpx;
				background-size: 100%;
				font-size: 28rpx;
			}
		}

		#partner {
			margin-top: 40rpx;
			display: grid;
			grid-template-columns: 1fr 1fr;
			grid-template-rows: 128rpx 128rpx;
			grid-gap: 30rpx 30rpx;

			.partner-item {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				font-size: 28rpx;
				border-radius: 20rpx;
				background: #FFFFFF;
				box-shadow: 0 0 8rpx 0 rgba(0, 0, 0, 0.12);

				.partner-item-image {
					margin-left: 30rpx;
					height: 76rpx;
					width: 76rpx;
					flex: 0 0 76rpx;
				}

				.partner-item-name {
					margin-left: 20rpx;
					flex: 1 1 20rpx;
					line-height: 40rpx;
				}
			}
		}
	}
	
	.bottom-button-area {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		background-color: #FFFFFF;
		.bottom-button-list {
			padding: 20rpx 40rpx;
			.bottom-button-item {
				height: 72rpx;
				line-height: 72rpx;
				background-color: #F74437;
				border-radius: 20rpx;
				text-align: center;
				color: #FFFFFF;
			}
		}
	}
</style>
