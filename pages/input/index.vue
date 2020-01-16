<template>
	<view class="container">
		<view class="flex">
			标题：<input style="border-radius: 8rpx;border: 2rpx #C0C0C0 solid;width: 500rpx;height: 80rpx;" v-model="title" placeholder="请输入标题"/>
		</view>
		<view class="flex">
			答案：<textarea style="border-radius: 8rpx;border: 2rpx #C0C0C0 solid;width: 500rpx;" v-model="answer" placeholder="请输入答案"/>
		</view>
		<button type="primary" @click="submit">提交</button>
		<button type="warn" @click="goDelete">选择删除</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '',
				answer: '',
				lib: []
			}
		},
		methods:{
			submit(){
				this.lib.push({'title':this.title,'answer':this.answer})
				uni.setStorage({
				    key: 'test',
				    data: this.lib,
				    success: function () {
				        uni.showToast({
				            title: 'success',
				            duration: 500
				        });
				    }
				});
			},
			goDelete(){
				uni.navigateTo({
				    url: '/pages/delete/delete'
				});
			}
		},
		onShow() {
			let self = this
			uni.getStorage({
				key: 'test',
				success: function (res) {
					if(typeof res.data === 'object' && res.data.length > 0){
						self.lib = res.data
					}
				}
			});
		}
	}
</script>

<style>
	.container{
		padding: 24rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-evenly;
		width: 100%;
		height: 100%;
		box-sizing: border-box;
	}
	.flex{
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
