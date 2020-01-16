<template>
	<view class="content">
		<view class="container">
			<view v-if="typeof lib === 'object' && lib.length > 0" class="haveDataContainer">
				<view class="text1">
					请回答:
				</view>
				<view class="text2">
					<text style="width: 100%;word-break: break-word;">{{title}}</text>
				</view>
				<view class="goAnswer">
					<button type="primary" @click="checkAnswer">查看答案</button>
					<button style="margin-top: 24rpx;" type="primary" @click="changeATest" :disabled="lib.length === 1">换一道题</button>
				</view>
			</view>
			<view v-else>
				暂无数据
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '',
				answer: '',
				lib: [],
				testIndex: 0
			}
		},
		onLoad() {
			let self = this
			uni.getStorage({
				key: 'test',
				success: function (res) {
					if(typeof res.data === 'object' && res.data.length > 0){
						self.lib = res.data
						self.testIndex = Math.floor(Math.random() * res.data.length)
						self.title = res.data[self.testIndex].title
						self.answer = res.data[self.testIndex].answer
					} else {
						self.title = ''
						self.answer = ''
					}
				}
			});
		},
		onShow() {
			let self = this
			uni.getStorage({
				key: 'test',
				success: function (res) {
					if(typeof res.data === 'object' && res.data.length > 0){
						self.lib = res.data
					} else {
						self.lib = []
					}
				}
			});
		},
		methods: {
			checkAnswer(){
				uni.navigateTo({
				    url: '/pages/checkAnswer/checkAnswer?index=' + this.testIndex
				});
			},
			changeATest(){
				while(true){
					let temp = Math.floor(Math.random() * this.lib.length)
					if(this.testIndex !== temp) {
						this.testIndex = temp
						this.title = this.lib[this.testIndex].title
						this.answer = this.lib[this.testIndex].answer
						break
					}
				}
			}
		}
	}
</script>

<style>
	.content {
		width: 100%;
		height: 100%;
		box-sizing: border-box;
	}
	.container{
		width: 100%;
		height: 100%;
		box-sizing: border-box;
		padding: 0 48rpx 24rpx 48rpx;
		overflow: auto;
	}
	.haveDataContainer{
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-evenly;
	}
</style>
