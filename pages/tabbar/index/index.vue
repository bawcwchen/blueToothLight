<template>
	<view>
		<view class="uni-padding-wrap uni-common-pb">
			<!-- <view class="uni-card" v-for="(list,index) in lists" :key="index">
			<view class="uni-list">
				<view class="uni-list-cell uni-collapse">
					<view class="uni-list-cell-navigate uni-navigate-bottom" hover-class="uni-list-cell-hover" :class="list.open ? 'uni-active' : ''"
					 >
						{{list.name}}
					</view>
					<view class="uni-list uni-collapse" :class="list.open ? 'uni-active' : ''">
						<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,key) in list.pages" :key="key" :deviceId="item.deviceId"
						 @click="goDetailPage(item.deviceId)">
							<view class="uni-list-cell-navigate uni-navigate-right"> {{item.name}} </view>
						</view>
					</view>
				</view>
			</view>
		</view> -->
			<view class="uni-card">
				<view class="uni-list">
					<view class="uni-list-cell uni-collapse">
						<view class="uni-list-cell-navigate uni-navigate-bottom" hover-class="uni-list-cell-hover" :class="'uni-active'">
							已连接设备
						</view>
						<view class="uni-list uni-collapse" :class="'uni-active'">
							<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,key) in connectDeviceList" :key="key"
							 :deviceId="item.deviceId" @click="goDetailPage(item.deviceId)">
								<view class="uni-list-cell-navigate uni-navigate-right"> {{item.name}} </view>
								<view class="uni-list-cell-navigate uni-navigate-right">{{item.deviceId}}</view>
								<view style="margin-right: -200px;">
									<image style="width: 15px;height: 15px;" src="/static/img/signal/1.png"></image>
									{{item.RSSI}}
								</view>

							</view>
						</view>
					</view>
				</view>

				<view class="uni-list">
					<view class="uni-list-cell uni-collapse">
						<view class="uni-list-cell-navigate uni-navigate-bottom" hover-class="uni-list-cell-hover" :class="'uni-active'">
							未连接设备
						</view>
						<view class="uni-list uni-collapse" :class="'uni-active'">
							<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,key) in unConnectDeviceList" :key="key"
							 :deviceId="item.deviceId" @click="goDetailPage(item.deviceId)">
								<view class="uni-list-cell-navigate uni-navigate-right"> {{item.name}} </view>
							</view>
						</view>
					</view>
				</view>

				<view class="uni-list">
					<view class="uni-list-cell uni-collapse">
						<view class="uni-list-cell-navigate uni-navigate-bottom" hover-class="uni-list-cell-hover" :class="'uni-active'">
							不可用设备
						</view>
						<view class="uni-list uni-collapse" :class="'uni-active'">
							<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,key) in unableDeviceList" :key="key"
							 :deviceId="item.deviceId">
								<view class="uni-list-cell-navigate uni-navigate-right"> {{item.name}} </view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'



	export default {
		components: {
			uniCollapse,
			uniCollapseItem,
			uniList,
			uniListItem
		},
		data() {
			const connectDeviceList = [{
					name: '灯25',
					deviceId: 'dkfjdfkdkf-99jkdfjdkjf-jdkfjdkfj9993232',
					RSSI: 98
				}, {
					name: '灯18',
					deviceId: 'dkfjdfkdkf-99jkdfjdkjf-jdkfjdkfj9993232',
					RSSI: 66
				},

				{
					name: '灯19',
					deviceId: 'dkfjdfkdkf-99jkdfjdkjf-jdkfjdkfj9993232',
					RSSI: 34
				},

				{
					name: '灯10',
					deviceId: 'dkfjdfkdkf-99jkdfjdkjf-jdkfjdkfj9993232',
					RSSI: 56
				},
				{
					name: '灯21',
					deviceId: 'dkfjdfkdkf-99jkdfjdkjf-jdkfjdkfj9993232',
					RSSI: 78
				}


			];

			const unConnectDeviceList = [{
					name: '灯11',
					deviceId: '11'
				}, {
					name: '灯12',
					deviceId: '12'
				}, {
					name: '灯13',
					deviceId: '13'
				},
				{
					name: '灯14',
					deviceId: '14'
				}, {
					name: '灯15',
					deviceId: '15'
				}

			];

			const unableDeviceList = [{
				name: '灯1',
				deviceId: '1'
			}, {
				name: '灯2',
				deviceId: '2'
			}, {
				name: '灯3',
				deviceId: '3'
			}];
			console.log("初始化值");

			return {
				connectDeviceList: connectDeviceList,
				unConnectDeviceList: unConnectDeviceList,
				unableDeviceList: unableDeviceList
			}
		},
		onLoad() {
			this.searchBleDevices();
			console.log("搜索蓝牙设备");
		},
		methods: {
			//搜索蓝牙设备
			searchBleDevices: function() {
				//初始化蓝牙
				uni.openBluetoothAdapter({
					success(res) {
						setTimeout(() => {
							getBluetoothAdapterState()
						}, 1000)
					},
					fail: function(err) {
						// 初始化失败
					}
				})

			},
			//获取蓝牙状态
			getBluetoothAdapterState: function() {
				uni.getBluetoothAdapterState({
					success(res) {
						startBluetoothDevicesDiscovery()
					},
					fail(res) {
						console.log(res)
					}
				})

			},
			//开始搜索蓝牙设备
			startBluetoothDevicesDiscovery: function() {
				setTimeout(() => {
					uni.startBluetoothDevicesDiscovery({
						success: function(res) {
							/* 获取蓝牙设备列表 */
							that.getBluetoothDevices()
						},
						fail(res) {}
					})
				}, 1000)

			},
			//获取蓝牙设备列表
			getBluetoothDevices: function() {
				var that = this;
				setTimeout(() => {
					uni.getBluetoothDevices({
						// services: [],
						// allowDuplicatesKey: false,
						// interval: 0,
						success: function(res) {
							// 							if (res.devices.length > 0) {
							// 								if (JSON.stringify(res.devices).indexOf(that.deviceName) !== -1) {
							// 									for (let i = 0; i < res.devices.length; i++) {
							// 										if (that.deviceName === res.devices[i].name) {
							// 											/* 根据指定的蓝牙设备名称匹配到deviceId */
							// 											that.deviceId = that.devices[i].deviceId;
							// 											setTimeout(() => {
							// 												that.createBLEConnection();
							// 											}, 2000);
							// 										};
							// 									};
							// 								} else {}
							// 							} else {}
							this.connectDeviceList = res.devices;
						},
						fail(res) {
							console.log(res, '获取蓝牙设备列表失败=====')
						}
					})
				}, 2000)

			},
			goDetailPage(e) {
				uni.navigateTo({
					url: '../../equipmentDetail/equipmentDetail?deviceId=' + e
				});

			}

		}
	}
</script>

<style>
	page {
		height: auto;
		min-height: 100%;
	}

	.uni-card {
		box-shadow: none;
	}

	.uni-list:after {
		height: 0;
	}

	.uni-list:before {
		height: 0;
	}
</style>
