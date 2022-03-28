<template>
	<view>
		<view class="solid-bottom title padding">标题</view>
		<view class="padding">
			<input placeholder="请输入标题(1-30字)" class="text-df" v-model="materialName" maxlength="30"></input>
		</view>
		<view style="background-color: #F8F8F8;height: 10px;">
		</view>
		<view class="solid-bottom title padding">正文</view>
		<view class="padding-xs" style="height: auto;">
			<iep-jodit-editor v-model="html" :height="400"></iep-jodit-editor>
		</view>
		<view class="padding-xs">
			<view style="float: left;padding-right: 20rpx;vertical-align: middle;position: relative;display: inline-flex;align-items: center;justify-content: center;">
				<image src="../../static/tag.png" style="width: 20px;height: 20px;margin-right: 5px;"></image>添加标签
			</view>
			<view style="display: flex;overflow-x: auto;width: 550rpx;">
				<text class='cu-tag text-red round' style="flex: none;background-color: #FEF0F4;" v-for="(item,index) in tag" :key="index">
					<view class="cu-form-group" style="background-color: #FEF0F4;padding: 0;">
						<text :id="index" contenteditable="true" style="background-color: #FEF0F4;padding-right: 5px;min-width: 40px;font-size: 12px;color: red;outline:none;"
						 @blur="tagedit(index)"></text>
						<!-- <input style="padding-right: 5px;width: 60px;font-size: 12px;color: red;" v-model="tag[index]"/> -->
						<text class='cuIcon-close text-red' @tap="deltag(index)" style="font-size: 12px;"></text>
					</view>
				</text>
				<text v-show="tag.length<5" @tap="addtag" class="cu-tag text-red round" style="flex: none;background-color: #FEF0F4;font-size: 16px;width: 24px;"><text
					 class='cuIcon-add text-red'></text></text>
			</view>
		</view>
		<view class="padding-xs">
			<button class="cu-btn bg-red margin-tb-sm lg" style="width: 100%;" @tap="tijiao">修改</button>
		</view>
	</view>
</template>

<script>
	import IepJoditEditor from "@/components/IepJoditEditor/index.vue";
	export default {
		components: {
			IepJoditEditor
		},
		data() {
			return {
				materialName: '',
				token: '',
				html: '',
				tag: [],
				materialId: '',
			}
		},
		methods: {
			tagedit(index) {
				this.tag[index] = document.getElementById(index).innerText;
			},
			deltag(index) {
				this.tag.splice(index, 1)
				for (let i = 0; i < this.tag.length; i++) {
					document.getElementById(i).innerText = this.tag[i];
				}
			},
			addtag() {
				this.tag.push("");
			},
			tijiao() {
				let that = this;
				if (that.materialName == "" || that.html == "") {
					uni.showToast({
						title: '请填写标题和正文',
						icon: 'none'
					});
				} else {
					uni.request({
						url: '/api/dms/material/update',
						method: 'POST',
						header: {
							'Authorization': "Bearer " + that.token,
						},
						data: {
							id: that.materialId,
							materialName: that.materialName,
							materialContent: that.html,
							tag: that.tag
						},
						success: (res) => {
							if(res.data.code == 0){
								uni.showToast({
									title: '文章修改成功',
									icon: 'success'
								});
							}
							else{
								uni.showToast({
									title:res.data.message,
									icon:'none'
								})
							}
						}
					})
				}
			}
		},
		onLoad(option) {
			uni.showLoading();
			this.token = option.token
			this.materialId = option.materialId
			uni.request({
				url: '/api/dms/material/' + option.materialId,
				header: {
					'Authorization': "Bearer " + option.token,
				},
				success: (res) => {
					console.log(res.data.data)
					this.materialName = res.data.data.materialName
					this.html = res.data.data.materialContent
					this.tag = res.data.data.tag
					this.materialId = res.data.data.id
					uni.hideLoading();
				}
			});
		},
		updated() {
			for (let i = 0; i < this.tag.length; i++) {
				console.log(document.getElementById(i));
				document.getElementById(i).innerText = this.tag[i];
			}
		}
	}
</script>

<style>
	page {
		background-color: #fff;
	}
</style>
