<template>
	<view class="container">
		<swiper indicator-dots=true circular=true class="swiper">
			<swiper-item>
				<navigator url="../bb/bb" class="item">
					<view class="item-img">
						<image src="../../static/image/bb.jpg" mode="aspectFill"></image>
					</view>
					<hr />
					<view class="item-text">
						小声逼逼
					</view>
				</navigator>
			</swiper-item>
			<swiper-item>
				<navigator url="../tally/tally" class="item">
					<view class="item-img">
						<image src="../../static/image/jizhang.jpg" mode="aspectFill"></image>
					</view>
					<hr />
					<view class="item-text">
						记账本
					</view>
				</navigator>
			</swiper-item>
			<swiper-item> 
				<navigator url="../hit/hit" class="item">
					<view class="item-img">
						<image src="../../static/image/baoda.jpg" mode="aspectFill"></image>
					</view>
					<hr />
					<view class="item-text">
						暴打姚立渠
					</view>
				</navigator>
			</swiper-item>
			<swiper-item>
				<navigator url="../lifePublish/lifePublish" class="item">
					<view class="item-img">
						<image src="../../static/image/xinqin.jpg" mode="aspectFill"></image>
					</view>
					<hr />
					<view class="item-text">
						心情/日记
					</view>
				</navigator>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		onLoad() {

		},
		methods: {
		tip(){
			uni.showToast({
			    title: '时间不够，我抓紧做',
			    duration: 2000,
				icon:"none"
			});
		}
		}
	}
</script>

<style>
	page {
		margin: 0;
		padding: 0;
		font-family: 'Open Sans Light';
		letter-spacing: 10upx;
		height: 100%;
	}

	.container {
		height: 100%;
		background-image: linear-gradient(to right, #ffd0ed, #fffcdc);
	}

	.swiper {
		width: 100%;
		height: 100%;
	}

	.item {
		width: 80%;
		height: 75%;
		margin: 120upx auto;
		box-sizing: border-box;
		border-radius: 30upx;
		background-color: #fffbfa;
	}

	.item-img {
		width: 100%;
		height: 75%;
		box-sizing: border-box;
	}

	.item-img image {
		width: 100%;
		height: 100%;
		border-radius: 30upx 30upx 0 0;
		box-sizing: border-box;
	}

	hr {
		width: 85%;
		margin: 40upx auto 0;
		border: 1px dashed #5a5a5a;
	}

	.item-text {
		margin: 20px auto 0;
		text-align: center;
		font-size: 46upx;
		color: #999999;
	}
</style>
