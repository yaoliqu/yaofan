<template>
	<view class="index">
		<block v-for="list in lists" :key="list.id">
			<view class="row">
				<view class="card card-list2" v-for="item in list.data" @click="goDetail(item)" :key="item.keys">
					<image class="card-img card-list2-img" :src="item.img_src"></image>
					<text class="card-num-view card-list2-num-view">{{ item.img_num }}P</text>
					<view class="card-bottm row">
						<view class="car-title-view row">
							<text class="card-title card-list2-title">{{ item.title }}</text>
						</view>
						<!-- 分享 -->
						<!-- <view @click.stop="share(item)" class="card-share-view"></view> -->
					</view>
				</view>
			</view>
		</block>
		<text class="loadMore">加载中...</text>
	</view>
</template>

<script>
let keys = 0;
export default {
	data() {
		return {
			refreshing: false,
			lists: [],
			fetchPageNum: 1
		};
	},
	created()  {
		this.getData();
		uni.getProvider({
			service: 'share',
			success: e => {
				let data = [];
				for (let i = 0; i < e.provider.length; i++) {
					switch (e.provider[i]) {
						case 'weixin':
							data.push({
								name: '分享到微信好友',
								id: 'weixin'
							});
							data.push({
								name: '分享到微信朋友圈',
								id: 'weixin',
								type: 'WXSenceTimeline'
							});
							break;
						case 'qq':
							data.push({
								name: '分享到QQ',
								id: 'qq'
							});
							break;
						default:
							break;
					}
				}
				this.providerList = data;
			},
			fail: e => {
				console.log('获取登录通道失败', e);
			}
		});
	},
	onPullDownRefresh() {
		console.log('下拉刷新');
		this.refreshing = true;
		this.getData();
	},
	/* onReachBottom() {
		this.getData();
	}, */
	 mounted() {
	    window.addEventListener("scroll", this.onScroll);
	  },
	methods: {
		    onScroll() {
		      //可滚动容器的高度
		      let innerHeight = document.querySelector('#app').clientHeight;
					console.log(innerHeight)
		      //屏幕尺寸高度
		      let outerHeight = document.documentElement.clientHeight;
					console.log(outerHeight)
		      //可滚动容器超出当前窗口显示范围的高度
		      let scrollTop = document.documentElement.scrollTop;
					console.log(scrollTop)
		      //scrollTop在页面为滚动时为0，开始滚动后，慢慢增加，滚动到页面底部时，出现innerHeight < (outerHeight + scrollTop)的情况，严格来讲，是接近底部。
		      //console.log(innerHeight + " " + outerHeight + " " + scrollTop);
		      if (innerHeight <= outerHeight + scrollTop) {
		        //加载更多操作
						this.getData();
		      }
		    },
		getData() {
			uni.request({
				url: this.$serverUrl + '/api/picture/posts.php?page=' + (this.refreshing ? 1 : this.fetchPageNum) + '&per_page=10',
				success: ret => {
					if (ret.statusCode !== 200) {
						console.log('请求失败:', ret);
					} else {
						if (this.refreshing && ret.data[0].id === this.lists[0].data[0].id) {
							uni.showToast({
								title: '已经最新',
								icon: 'none'
							});
							this.refreshing = false;
							uni.stopPullDownRefresh();
							return;
						}

						let list = {
								id: '',
								data: []
							},
							lists = [],
							data = ret.data;
						for (let i = 0, length = data.length; i < length; i++) {
							let index = Math.floor(i / 2);
							list.id = 'list' + keys;
							data[i].keys = keys++;
							list.data.push(data[i]);
							if (i % 2 == 1) {
								lists.push(list);
								list = {
									id: '',
									data: []
								};
							}
						}
						console.log('得到lists', lists);
						if (this.refreshing) {
							this.refreshing = false;
							uni.stopPullDownRefresh();
							this.lists = lists;
							this.fetchPageNum = 2;
						} else {
							this.lists = this.lists.concat(lists);
							this.fetchPageNum += 1;
						}
					}
				}
			});
		},
		goDetail(e) {
			uni.showToast({
			    title: '待开发',
			    duration: 2000,
				icon:'none'
			});
			uni.navigateTo({
				url:'./../index/photoDetail/photoDetail?data=' + encodeURIComponent(JSON.stringify(e))
				//url:'../photoDetail/photoDetail?data=' + encodeURIComponent(JSON.stringify(e))
			});
		},
		share(e) {
			if (this.providerList.length === 0) {
				uni.showModal({
					title: '当前环境无分享渠道!',
					showCancel: false
				});
				return;
			}
			let itemList = this.providerList.map(function(value) {
				return value.name;
			});
			uni.showActionSheet({
				itemList: itemList,
				success: res => {
					uni.share({
						provider: this.providerList[res.tapIndex].id,
						scene: this.providerList[res.tapIndex].type && this.providerList[res.tapIndex].type === 'WXSenceTimeline' ? 'WXSenceTimeline' : 'WXSceneSession',
						type: 0,
						title: 'uni-app模版',
						summary: e.title,
						imageUrl: e.img_src,
						href: 'https://uniapp.dcloud.io',
						success: res => {
							console.log('success:' + JSON.stringify(res));
						},
						fail: e => {
							uni.showModal({
								content: e.errMsg,
								showCancel: false
							});
						}
					});
				}
			});
		}
	}
};
</script>

<style scoped>
	@import '/common/common.css';
	view{
		display: flex;
	}
	.card-bottm{
		height: 100rpx;
	}
</style>
