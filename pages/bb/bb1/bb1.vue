<template>
	<view>
		<view class="tmp-box">
			<view class='filterBox'>
				<view class='filter-input'>
					<image class='filterImg' src='../../../static/our/chunqian.png'></image>
					<input class="text" type='text' v-model="inputVal" confirm-type="搜索" @confirm='handleFllter' placeholder='记仇小本本'></input>
				</view>
			</view>
		</view>
		<button type="primary" class="btn">记下，你死定了</button>
		<!-- 单行内容显示 -->
		<uni-list>
			<uni-list-item title="列表文字ccccccccccc啛啛喳喳错错错" clickable   @click="onClick"></uni-list-item>
		</uni-list>
	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputVal: "",
			}
		},

		methods: {
			onClick(){
				uni.showModal({
				    title: '提示',
				    content: '这是一个模态弹窗',
				    success: function (res) {
				        if (res.confirm) {
				            console.log('用户点击确定');
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			},
			handleFllter(e) {
				// console.log(e.detail.value)
				this.$emit("confirm", e.detail.value)
			},
			clears() {
				console.log(this.inputVal)
				this.inputVal = ""
				console.log('清除了内容')
			}
		}
	}
</script>

<style lang="scss" scoped>
	.filterBox {
		padding: 8px 16px;
		background-color: #fff;

		.filter-input {
			height: 40px;
			background-color: #eeeff0;
			border-radius: 20px;
			display: flex;
			align-items: center;
			padding-left: 20px;

			.filterImg {
				width: 16px;
				height: 16px;
				margin-right: 10px;
			}

			.filterImgs {
				width: 16px;
				height: 16px;

			}

			.text {
				width: 84%;
				background-color: #eeeff0;
				font-size: 16px;
				color: #000;
			}
		}
	}

	.btn {
		width: 500upx;
		margin: 40upx auto;
		border-radius: 20upx;
		background-color: #fc87dd;
	}
</style>
