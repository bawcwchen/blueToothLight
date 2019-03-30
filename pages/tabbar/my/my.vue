<template>
	<view>
<uni-list>
	<uni-list-item title="关于我们" @click="aboutUs()">
	</uni-list-item>
	<uni-list-item title="公司地址" @click="myLocation()">
	</uni-list-item>
</uni-list>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'

	export default {
		components: {
			uniList,
			uniListItem
		},
		data() {
			return {

			};
		},
		methods:{
			aboutUs(e) {
				uni.navigateTo({
					url: '../../aboutUs/aboutUs' 
				});
			},
			myLocation(){
				uni.navigateTo({
					url: '../../myLocation/myLocation' 
				});
			}
		}
	}
</script>

<style>

</style>
