<template>
	<view>
		<map style="width: 100%; height: 300px;"  :latitude="latitude" :longitude="longitude" :markers="covers">
		</map>
		<view style="text-align: center;">美的集团股份有限公司</view>
		<view style="text-align: center;">广东省佛山市顺德区北滘镇美的大道6号美的总部大楼</view>
		<view style="text-align: center;">电话：0757-26338888</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				latitude: 23.05,
				longitude: 113.112,
				/*latitude: 23.05,
				longitude: 113.11  ,
				covers: [{
                latitude: 23.05,
                longitude: 113.11 ,
                iconPath: '../../static/logo.png'
            }, {
                latitude: 23.05,
                longitude: 113.11 ,
                iconPath: '../../static/logo.png'
            }] */
			};
		},
		onLoad() {
			uni.setNavigationBarTitle({
				title: '公司地址'
			});
		}
	}
</script>

<style>

</style>
