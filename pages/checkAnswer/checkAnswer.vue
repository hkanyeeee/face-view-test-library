<template>
	<view class="container">
		<view class="container" v-if="!isXiugai">
			<view>
				答案是：
			</view>
			<view style="word-break: break-word;padding: 0 48rpx;">
				{{answerText}}
			</view>
			<view>
				<button type="primary" @click="changeAnswer">修改答案</button>
			</view>
		</view>
		<view class="container" v-else>
			<view>
				请修改：
			</view>
			<textarea style="border-radius: 8rpx;border: 2rpx #C0C0C0 solid;width: 500rpx;" v-model="answerTestArea" placeholder="请输入答案"/>
			<button type="primary" @click="changeFinish">修改完成</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				lib: '',
				answerText: '',
				isXiugai: false,
				answerTestArea: '',
				changeIndex: 0
			}
		},
		onLoad(option) {
			let self = this
			uni.getStorage({
				key: 'test',
				success: function (res) {
					if(typeof res.data === 'object' && res.data.length > 0){
						self.lib = res.data
						self.answerText = res.data[parseInt(option.index)].answer
						self.answerTestArea = res.data[parseInt(option.index)].answer
						self.changeIndex = parseInt(option.index)
					}
				}
			});
		},
		methods: {
			changeAnswer() {
				this.isXiugai = true
			},
			changeFinish() {
				this.isXiugai = false
				this.answerText = this.answerTestArea
				this.lib[this.changeIndex].answer = this.answerTestArea
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
			}
		}
	}
</script>

<style>
	.container{
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-evenly;
		align-items: center;
	}
</style>
