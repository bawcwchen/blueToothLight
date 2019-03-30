<template>
	<view>

		<!-- <view class="uni-list">
			<view class="uni-list-cell uni-list-cell-pd">
				<view class="uni-list-cell-db">开/关</view>
				<switch color="#F76260" checked @change="switchChange" />
			</view>

		</view> -->
		<view class="uni-padding-wrap uni-common-mt">
			<view class="uni-title">开灯/关灯</view>
			<view>
				<switch color="#F76260" checked @change="switchChange" />
			</view>
		</view>

		<view class="uni-padding-wrap uni-common-mt">
			<view class="uni-title">亮度</view>
			<view>
				<slider block-color="#FFB400" activeColor="#F76260" value="50" @change="sliderChange" min="0" max="100" show-value />
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		onLoad: function(option) {
			uni.setNavigationBarTitle({
				title: '设备控制'
			});
			this.deviceId = option.deviceId + '';
		},
		data() {
			return {
				deviceId: '',
				notifyServiceId:'',
				notifyCharacteristicsId:''
			};
		},
		methods: {
			sliderChange: function(e) {
				console.log('value 发生变化：' + e.detail.value);
				sendData(e.detail.value);
			},
			switchChange: function(e) {
				console.log('携带值为', e.target.value);
				if (e.target.value) { //连接蓝牙
					this.createBLEConnection();
				} else { //关闭蓝牙
					this.closeBLEConnection();
				}
			},
			//向蓝牙设备发送数据
			sendData: function(str) {
				let that = this;
				let dataBuffer = new ArrayBuffer(str.length)
				let dataView = new DataView(dataBuffer)
				for (var i = 0; i < str.length; i++) {
					dataView.setUint8(i, str.charAt(i).charCodeAt())
				}
				let dataHex = that.ab2hex(dataBuffer);
				this.writeDatas = that.hexCharCodeToStr(dataHex);
				uni.writeBLECharacteristicValue({
					deviceId: that.deviceId,
					serviceId: that.notifyServiceId,
					characteristicId: that.notifyCharacteristicsId,
					value: dataBuffer,
					success: function(res) {
						console.log('发送的数据：' + that.writeDatas)
						console.log('message发送成功')
					},
					fail: function(res) {},
					complete: function(res) {}
				})
			},
			/*转成二进制*/
			ab2hex: function(buffer) {
				var hexArr = Array.prototype.map.call(
					new Uint8Array(buffer),
					function(bit) {
						return ('00' + bit.toString(16)).slice(-2)
					}
				)
				return hexArr.join('')
			},
			/*转成可展会的文字*/
			hexCharCodeToStr: function(hexCharCodeStr) {
				var trimedStr = hexCharCodeStr.trim();
				var rawStr = trimedStr.substr(0, 2).toLowerCase() === '0x' ? trimedStr.substr(2) : trimedStr;
				var len = rawStr.length;
				var curCharCode;
				var resultStr = [];
				for (var i = 0; i < len; i = i + 2) {
					curCharCode = parseInt(rawStr.substr(i, 2), 16);
					resultStr.push(String.fromCharCode(curCharCode));
				}
				return resultStr.join('');
			},
			/**
			 * 连接低功耗蓝牙
			 */
			createBLEConnection: function() {
				uni.showToast({
					title: '连接蓝牙...',
					icon: 'loading',
					duration: 99999
				});
				let deviceId = this.deviceId;
				uni.createBLEConnection({
					// 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接
					deviceId,
					success: res => {
						console.log(res);
						console.log('连接蓝牙成功:' + res.errMsg);
						// 连接设备后断开搜索 并且不能搜索设备
						this.stopBluetoothDevicesDiscovery();
						uni.hideToast();
						uni.showToast({
							title: '连接成功',
							icon: 'success',
							duration: 2000
						});
					},
					fail: e => {
						console.log('连接低功耗蓝牙失败，错误吗：' + e.errCode);

					}
				});
			},
			/**
			 * 停止搜索蓝牙设备
			 */
			stopBluetoothDevicesDiscovery: function() {
				uni.stopBluetoothDevicesDiscovery({
					success: e => {
						console.log('停止搜索蓝牙设备:' + e.errMsg);
					},
					fail: e => {
						console.log('停止搜索蓝牙设备失败，错误吗：' + e.errCode);
					}
				});
			},
			/**
			 * 断开与低功耗蓝牙设备的连接
			 */
			closeBLEConnection: function() {
				let deviceId = this.deviceId;
				uni.closeBLEConnection({
					deviceId,
					success: res => {
						console.log(res);
						console.log('断开低功耗蓝牙成功:' + res.errMsg);
					},
					fail: e => {
						console.log('断开低功耗蓝牙成功，错误吗：' + e.errCode);
					}
				});
			},
		}
	}
</script>

<style>

</style>
