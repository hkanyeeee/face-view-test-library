<template>
	<view class="container">
		<!-- <view class="deleteCard">
			<view class="titleArea">
				我是标题我是标题
			</view>
			<view class="deleteArea">
				X
			</view>
		</view> -->
		<view v-if="lib !== [] && lib.length > 0">
			<view class="deleteCard" v-for="(item,index) in lib" :key="index">
				<view class="titleArea">
					{{item.title}}
				</view>
				<view class="deleteArea" @click="deleATest(index)">
					X
				</view>
			</view>
		</view>
		<view v-else>
			暂无数据
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				lib: []
			}
		},
		methods: {
			deleATest(index) {
				this.lib.splice(index, 1)
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
		},
		onShow() {
			let self = this
			uni.getStorage({
				key: 'test',
				success: function (res) {
					if(typeof res.data === 'object' && res.data.length > 0) {
						self.lib = res.data
						console.log(self.lib[0])
					}
				}
			});
		}
	}
</script>

<style>
	.container{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
		align-items: center;
	}
	.deleteCard{
		width: 360rpx;
		height: 80rpx;
		display: flex;
		margin-top: 24rpx;
		justify-content: space-between;
		align-items: center;
		background-color: #3498db;
		border-radius: 16rpx;
		box-sizing: border-box;
		box-shadow: 0 2rpx 4rpx 0 rgba(0,0,0,0.15), 0px 2px 4px 0px rgba(0,0,0,0.10);
		border: 2rpx solid rgba(165,170,184,0.10);
	}
	.titleArea{
		flex: 1;
		line-height: 80rpx;
		text-align: center;
		font-size: 24rpx;
		word-break: break-all;
		color: #FFFFFF;
	}
	.deleteArea{
		width: 100rpx;
		line-height: 80rpx;
		text-align: center;
		color: #FFFFFF;
	}
</style>
