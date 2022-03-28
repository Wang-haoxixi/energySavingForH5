<template>
	<view>
		<!-- <cu-custom bgColor="bg-gradual-pink" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">聊天</block>
		</cu-custom> -->
		<view style="display: flex;background:linear-gradient(180deg,rgba(239,249,255,1) 0%,rgba(247,251,255,1) 100%);height: 216rpx;align-items: flex-end;">
			<image src="../../static/bg_service.png" mode="aspectFit" style="margin-left: 66rpx;height: 170rpx;width: 186rpx;"></image>
			<view style="margin-bottom: 60rpx;margin-left: -20rpx;color: #303031;font-size: 28rpx;font-weight: 600;">
				<view>亲爱的{{realName}}</view>
				<view>我是智能客服小能，很高兴为您服务</view>
			</view>
		</view>
		<view class="cu-chat">
			<view class="cu-item">
				<view class="cu-avatar radius" style="background-image:url(../../static/icon_service.png);"></view>
				<view class="main">
					<view class="content" style="background-color: #F4F5F6;border-radius: 20rpx;width: 486rpx;">
						<view style="font-size: 28rpx;width: 100%;">
							<view class="shadow" style="padding: 15rpx 0 15rpx 0;font-weight: 600;font-size: 32rpx;">我猜你想问</view>
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('我要加入组件宝')">
								我要加入组件宝<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<!-- <view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('拓展人脉相关问题')">
								拓展人脉相关问题<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>s
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('我要发布生态说说')">
								我要发布生态说说<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view> -->
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('怎么创建组织')">
								怎么创建组织<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<!-- <view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('如何邀请他人加组织')">
								如何邀请他人加组织<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #E25055;" @tap="senddefault('怎么在组织内群聊')">
								怎么在组织内群聊<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('如何发布组织公告')">
								如何发布组织公告<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('如何成为星级合伙人')">
								如何成为星级合伙人<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('怎么发宝贝红包')">
								怎么发宝贝红包<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view>
							<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #F74437;" @tap="senddefault('组件宝平台价值如何产生')">
								组件宝平台价值如何产生<text class="text-gray cuIcon-right" style="float: right;"></text>
							</view> -->
							<!-- <view class="text-grey" style="padding: 15rpx 0 15rpx 0;">
								都不是，请一句话完整描述你的问题？
							</view> -->
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="cu-chat" v-for="(item,index) in sendArray" :key="index">
			<view class="cu-item self">
				<view class="main">
					<view class="content shadow" style="background-color: #F94337;color: #FFFFFF;font-size: 28rpx;padding: 18rpx 30rpx;border-radius: 10rpx;">
						<text>{{item.send}}</text>
					</view>
				</view>
				<image :src="sendimg" style="width: 80rpx;height: 80rpx;border-radius: 50%;" @error="imageError"></image>
				<!-- <view class="cu-avatar radius" :style="{backgroundImage:'url(' + sendimg + ')'}"></view> -->
			</view>
			<view class="cu-item">
				<view class="cu-avatar radius" style="background-image:url(../../static/icon_service.png);"></view>
				<view class="main">
					<view class="content shadow" style="background-color: #F4F5F6;color: #303031;font-size: 28rpx;padding: 18rpx 30rpx;border-radius: 10rpx;">
						<view class="">
							<parser :html="item.answer.desc" :tag-style="tagStyle" img-mode="widthFix"></parser>
							<view v-for="(optionsItem,optionsIndex) in item.options" :key="optionsIndex">
								<view class="solids-bottom" style="padding: 15rpx 0 15rpx 0;color: #E25055;" @tap="senddefault(optionsItem.desc)">
									{{ optionsItem.desc }}
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="cu-bar foot input" :style="[{bottom:InputBottom+'px'}]" style="background-color: #FFFFFF;">
			<input placeholder-class="custom-placeholder-class" placeholder="请完整描述你的问题" class="input" :adjust-position="false"
			 :focus="false" maxlength="300" v-model="sendvalue"></input>
			<view class="action">
				<!-- <text class="cuIcon-emojifill text-grey"></text> -->
			</view>
			<button :disabled="sendvalue.length==0" class="cu-btn bg-red shadow" @tap="send()" style="font-weight: 600;">发送</button>
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
				InputBottom: 0,
				sendvalue: "",
				sendArray: [],
				index: 0,
				optionsIndex: 0,
				sendimg: "",
				token: "",
				realName: "",
				tagStyle: {
					img: 'background-size: contain|cover;width:100%;height:auto;'
				}
			};
		},
		methods: {
			imageError() {
				console.log("1111");
				let that = this;
				if (that.sendimg.substring(0, 2) == "//") {
					that.sendimg = "http:" + that.sendimg;
				} else {
					that.sendimg = "../../static/ic_launcher.png";
				}
			},
			senddefault(value) {
				let that = this;
				let sendvalue = value;
				that.sendvalue = "";
				that.sendArray.push({
					send: sendvalue,
					answer: {
						desc: "小能思考中……"
					},
					options: ""
				});
				uni.request({
					url: '/api/ims/serviceDialog/query/' + sendvalue,
					header: {
						'Authorization': "Bearer " + that.token,
					},
					success: (res) => {
						let data = res.data.data
						let list = [];
						let fn = (data, name) => {
							let index = list[list.length - 1] || 0;
							// 第一个 ="
							let index1 = data.indexOf(name);
							// 若存在 src= ，截至第一个双引号
							if (index1 >= 0) {
								list.push(index1 + index + 1);
								let string1 = data.substr(index1 + 2);
								// eslint-disable-next-line quotes
								let index2 = string1.indexOf('"');
								list.push(index1 + 2 + index2 + index);
								// 判断后续的文字中是否有双引号
								let string2 = string1.substr(index2);
								if (string2.indexOf(name) >= 0) {
									fn(string2, name);
								}
							}
						};
						fn(data, '="');
						for (let item of list) {
							data = data.substr(0, item) + "'" + data.substr(item + 1);
						}
						// 正常解析返回值
						let row = JSON.parse(data);
						that.sendArray[that.index].answer = row.answer;
						that.sendArray[that.index].options = row.options;
						console.log(that.sendArray);
						that.index++;
					}
				});

			},
			send() {
				let that = this;
				//控制用户不能输入全空格
				if (that.sendvalue.replace(/(^\s*)|(\s*$)/g, "") == "") {
					that.sendvalue = ""
					return
				}
				let sendvalue = that.sendvalue;
				that.sendvalue = "";
				that.sendArray.push({
					send: sendvalue,
					answer: {
						desc: "小能思考中……"
					},
					options: ""
				});
				uni.request({
					url: '/api/ims/serviceDialog/app/query/' + sendvalue,
					header: {
						'Authorization': "Bearer " + that.token,
					},
					success: (res) => {
						that.sendArray[that.index].answer = res.data.data.answer;
						that.sendArray[that.index].options = res.data.data.options;
						console.log(that.sendArray);
						that.index++;
					}
				});
			}
		},
		updated() {
			uni.pageScrollTo({
				scrollTop: document.getElementsByTagName("body")[0].scrollHeight,
				duration: 0
			});
		},
		onLoad(option) {
			let that = this;
			that.token = option.token
			// that.sendimg = option.imgurl
			uni.request({
				url: '/api/admin/user/information',
				header: {
					'Authorization': "Bearer " + that.token,
				},
				success: (res) => {
					that.realName = res.data.data.userInfo.realName;
					that.sendimg = res.data.data.userInfo.avatar
				}
			})
		}
	}
</script>

<style>
	.cu-avatar {
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	page {
		padding-bottom: 100upx;
		background-color: #FFFFFF;
	}

	.solids-bottom::after {
		border-bottom: 1px solid #E3E4E5;
	}

	.radius {
		border-radius: 50%;
	}

	.bg-red {
		background-color: #F74437;
	}

	.input {
		background-color: #F4F5F6;
		padding: 0 30rpx;
		border-radius: 10rpx;
		font-size: 24rpx;
	}

	.input>>>.custom-placeholder-class {
		color: #BABEC0;
		font-size: 24rpx;
	}

	.cu-chat .cu-item>.main .content::after {
		top: 42rpx;
	}

	.shadow[class*="-red"] {
		box-shadow: 3px 3px 4px rgba(204, 69, 59, 0);
	}
</style>
