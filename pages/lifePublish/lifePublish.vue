<template>
	<view>
		<view class="wrapper">
			ui我不会呀，好丑啊~<br/>
			等之后再优化了，凑合先
			<navigator url="./xinqin/xinqin" class="item">
				发个心情
			</navigator>
			<view class="item" @click="dkf">
				写个小作文
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
		dkf(){
			uni.showToast({
				title:'加班开发中...',
				duration:2000,
				icon:'none'
			})
		}
		}
	}
</script>

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		display: flex;
		justify-content: space-around;
		align-items: center;
		flex-direction: column;
	}

	.item {
		width: 60%;
		height: 240upx;
		background-color: #007AFF;
		border-radius: 30upx;
		color: #FFFFFF;
		font-size: 60upx;
		text-align: center;
		line-height: 240upx;
	    background-image: linear-gradient(to right,#FDC830, #F37335);
	}
</style>
