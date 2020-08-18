<template>
	<view>
		<view class="nav">
			<view class="nav_top">
				<scroll-view scroll-x scroll-with-animation class="tab-view" :scroll-left="scrollLeft">
					<view v-for="(item, index) in tabList" :key="index" class="tab-bar-item" :style="{ 'min-width': tabList.length > 3 ? '18%' : '25%' }"
					 :data-current="index" v-on:click="tabBtn(index, item)">
						<text class="tab-bar-title" :style="{ color: tabIndex == index ? '#ffaaff' : '' }">{{ item.name }}</text>
						<view class="slideItem-line" v-if="tabIndex == index"></view>
					</view>
				</scroll-view>
			</view>
		</view>
		<view class="access"  >
			<bb1 v-show="tabIndex==0"></bb1>
			<bb2 v-show="tabIndex==1"></bb2>
		</view>
	</view>
</template>

<script>
	import bb1 from '@/pages/bb/bb1/bb1'
	import bb2 from '@/pages/bb/bb2/bb2'
	export default {
		data() {
			return {
				//吸顶导航
				tabList: [{
						name: '记仇',
						index: 0
					},
					{
						name: '秘密基地',
						index: 1
					}
				],
				scrollLeft: 0, //tab标题的滚动条位置
				tabIndex: 0,
			};
		},
		components:{
			bb1,
			bb2,
		},
		methods: {
			//点击顶部导航
			tabBtn(index, item) {
				//切换tab
				this.tabIndex = index;
				/* uni.showLoading({
					title: '加载中'
				});
				uni.hideLoading(); */
			},
		}
	};
</script>

<style lang="scss" scoped>
	.nav_top {
		position: relative;
	}

	.tab-view::before {
		content: '';
		position: absolute;
		top: 0;
		right: 0;
		left: 0;
		border-bottom: 2px solid #dadadaff;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
	}

	.tab-view {
		width: 100%;
		height: 100upx;
		overflow: hidden;
		box-sizing: border-box;
		background: #fff;
		white-space: nowrap;
		position: fixed;
		z-index: 999;

		.active {
			color: #ffaaff;
			border-bottom: 2px solid #ffaaff;
		}

		/* position: absolute; */
		top: 0;
		right: 0;
		left: 0;
	}

	.tab-bar-item {
		padding: 0;
		height: 100upx;
		min-width: 18%;
		line-height: 100upx;
		margin: 0 3.5%;
		display: inline-block;
		text-align: center;
		box-sizing: border-box;
		position: relative;
	}

	.tab-bar-title {
		height: 100upx;
		line-height: 100upx;
	}

	.slideItem-line {
		width: 100upx;
		height: 3upx;
		background-color: #ffaaff;
		position: absolute;
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
	}
	.access{
		margin-top: 100upx;
	}
</style>
