<template>
	<view>
		<div class="test-component">
			<hm-cover-card :options="options"></hm-cover-card>
			<hm-cover-card :options="options1"></hm-cover-card>
		</div>
	</view>
</template>

<script>
	import HmCoverCard from '@/components/hm-cover-card/index.vue'
	export default {
		data() {
			return {
				options: {
					entryPic: '/static/hm-cover-card/images/img_25361_0_1.png',
					title: '愿望清单',
					text: '写下你的小计划',
					shoucang: '/static/hm-cover-card/images/img_25361_0_0.png'
				},
				options1: {
					entryPic: '/static/hm-cover-card/images/img_25361_0_1.png',
					title: '年度计划',
					text: '写下你的小计划',
					shoucang: '/static/hm-cover-card/images/img_25361_0_0.png'
				}
			};
		},
		components: {
			HmCoverCard
		},
		methods: {
		
		}
	};
</script>

<style>

</style>
