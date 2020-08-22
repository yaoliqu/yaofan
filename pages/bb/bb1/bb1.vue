<template>
	<view>
		<view class="tmp-box">
			<view class='filterBox'>
				<view class='filter-input'>
					<image class='filterImg' src='../../../static/image/search.png'></image>
					<input class="text" type='text' v-model="inputVal" confirm-type="搜索" placeholder='记仇小本本'></input>
				</view>
			</view>
		</view>
		<button type="primary" class="btn" @tap="pushContent">记下，你死定了</button>
		<!-- 单行内容显示 -->
		<uni-list v-for="(item,index) in list" :key=(item,index)>
			<uni-list-item :title=item clickable   @tap="onClick(index)"></uni-list-item>
		</uni-list>
	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputVal: "",
				list:[]
			}
		},

		methods: {
			onClick(index){
				uni.showModal({
				    title: '提示',
				    content: '要删除吗？',
				    success:res=> {
				        if (res.confirm) {
				            this.list.splice(index,1)
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			},
			
			
			
		pushContent(){
			this.list.push(this.inputVal)
			this.inputVal = ""
		},
		
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
