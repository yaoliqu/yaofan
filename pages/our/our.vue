<template>
	<view class="container">
		<view class="wrapper">
			<navigator url="/pages/photoAlbum/photoAlbum" class="wrapper-item item-bg-color1">
				<view class="text">
					相册
				</view>
				<view class="icon">
					<image src="../../static/our/xianche.png" mode="aspectFill"></image>
				</view>

			</navigator>
			<navigator url="/pages/what/what" class="wrapper-item item-bg-color7">
				<view class="text">
					不知道
				</view>
				<view class="icon">
					<image src="../../static/our/what.png" mode="aspectFill"></image>
				</view>
			</navigator>
			<view class="wrapper-item item-bg-color3">
				<view class="text">
					Together
				</view>
				<view class="icon">
					<image src="../../static/our/together.png" mode="aspectFill"></image>
				</view>
			</view>
			<view class="wrapper-item item-bg-color4">
				<view class="text">
					Plan
				</view>
				<view class="icon">
					<image src="../../static/our/plan.png" mode="aspectFill"></image>
				</view>
			</view>
			<view class="wrapper-item item-bg-color6">
				<view class="text">
					纪念日
				</view>
				<view class="icon">
					<image src="../../static/our/jinianri.png" mode="aspectFill"></image>
				</view>
			</view>
			<view class="wrapper-item item-bg-color2">
				<view class="text">
					存钱罐
				</view>
				<view class="icon">
					<image src="../../static/our/chunqian.png" mode="aspectFill"></image>
				</view>
			</view>
			<navigator url="../family/family" class="wrapper-item item-bg-color5">
				<view class="text">
					家园
				</view>
				<view class="icon">
					<image src="../../static/our/family.png" mode="aspectFill"></image>
				</view>
			</navigator>
			<view class="wrapper-item item-bg-color8">
				<view class="text1">
					退出登录
				</view>
				<view class="icon">
					<image src="../../static/our/out.png" mode="aspectFill"></image>
				</view>
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
		onLoad() {

		},
		methods: {

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
		background-image: linear-gradient(to right, #e1ffdd, #ffcfda);
		display: flex;
		align-items: center;
	}

	.wrapper {
		height: 80%;
		width: 100%;
		display: flex;
		justify-content: space-around;
		flex-wrap: wrap;
		align-items: center;
	}

	.wrapper-item {
		width: 42%;
		height: 150upx;
		border-radius: 50upx;
		box-shadow: 10px 10px 5px #888888;
		display: flex;
		justify-content: space-around;
		align-items: center;
	}

	.text {
		width: 50%;
		color: #fff;
		padding-left: 20upx;
		letter-spacing: 3upx;
	}

	.text1 {
		width: 55%;
		color: #fff;
		padding-left: 20upx;
		letter-spacing: 3upx;
	}

	.icon {
		width: 35%;
		height: 80%;
	}

	.icon image {
		width: 100%;
		height: 100%;
	}








	.item-bg-color1 {
		background-image: linear-gradient(to right, #F7971E, #FFD200);
	}

	.item-bg-color2 {
		background-image: linear-gradient(to right, #b92b27, #1565C0);
	}

	.item-bg-color3 {
		background-image: linear-gradient(to right, #2980B9, #6DD5FA);
	}

	.item-bg-color4 {
		background-image: linear-gradient(to right, #45B649, #DCE35B);
	}

	.item-bg-color5 {
		background-image: linear-gradient(to right, #f85032, #e73827);
	}

	.item-bg-color6 {
		background-image: linear-gradient(to right, #8E2DE2, #4A00E0);
	}

	.item-bg-color7 {
		background-image: linear-gradient(to right, #f12711, #f5af19);
	}

	.item-bg-color8 {
		background-image: linear-gradient(to right, #1e130c, #9a8478);
	}
</style>
