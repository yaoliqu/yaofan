<template>
	<view>
		<uni-list>
			<uni-list-item title="把你骗进来了" note="想跟你说的都在这里"></uni-list-item>
		</uni-list>
		<uni-list v-for="(item,index) in diaryList" :key=index >
		    <uni-list-item title="标题文字" note="描述信息" clickable @tap="goDetail"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
import uniList from '@/components/uni-list/uni-list.vue';
import uniListItem from '@/components/uni-list-item/uni-list-item.vue';
export default {
	components: { uniList, uniListItem },
	data() {
		return {
			diaryList:5 
		}
	},
	methods:{
		goDetail(){
			console.log(55)
			uni.navigateTo({
				url:'../family/diary/diaryDetail',
			})
			
		}
	}
};

</script>

<style></style>
