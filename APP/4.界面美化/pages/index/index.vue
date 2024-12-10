<template>
	<view class="wrap">
		<view class="dev-area">
			<view class="dev-cart">
				<view class="">
					<view class="dev-name">温度</view>
					<img class="dev-logo" src="../../static/logo.png" mode=""></image>
				</view>
				<view class="dev-data">{{temp}}℃</view>
			</view>
			<view class="dev-cart">
				<view class="">
					<view class="dev-name">湿度</view>
					<img class="dev-logo" src="../../static/logo.png" mode=""></image>
				</view>
				<view class="dev-data">{{humi}}℃</view>
			</view>
			<view class="dev-cart">
				<view class="">
					<view class="dev-name">指示灯</view>
					<img class="dev-logo" src="../../static/logo.png" mode=""></image>		
				</view>
				<switch :checked="led" @change="onLedSwitch" color="#8f8f94" />
			</view>
		</view>
	</view>
</template>

<script>
	const {
		createCommonToken
	} = require('@/key.js')
	export default {
		data() {
			return {
				temp: '11',
				humi: '23',
				led: true,
				token: '',
			}
		},
		onLoad() {
			const params = {
				author_key: 'cjKZ0SZdKEiSRbQhknuvnYHhx95cF+QMucPOPYQCKePaGiZAthS45l7I3VEh7lsr8Q5x9vHY62ZK6SUFdzQjzQ==',
				version: '2022-05-01',
				user_id: '408733',
			}

			this.token = createCommonToken(params);
		},
		onShow() {
			//向服务器请求数据
			this.fetchDevData();

			//定时进行数据请求
			// setInterval(()=>{
			// 	this.fetchDevData();
			// },10000)

		},
		methods: {
			fetchDevData() {
				//发起网络请求
				uni.request({
					url: 'https://iot-api.heclouds.com/thingmodel/query-device-property', //仅为示例，并非真实接口地址。
					method: 'GET',
					data: {
						product_id: 'GH98oDk7fb',
						device_name: 'DuZhong1'
					},
					header: {
						'authorization': this.token //自定义请求头信息
					},
					success: (res) => {
						console.log(res.data);
						this.humi = res.data.data[0].value;
						this.led = res.data.data[1].value === 'true';
						this.temp = res.data.data[2].value;
					}
				});
			},
			onLedSwitch(event) {
				console.log(event.detail.value);
				let value = event.detail.value;
				//发起网络请求
				uni.request({
					url: 'https://iot-api.heclouds.com/thingmodel/set-device-property', //仅为示例，并非真实接口地址。
					method: 'POST',
					data: {
						product_id: 'GH98oDk7fb',
						device_name: 'DuZhong1',
						params: {
							"led": value
						}
					},
					header: {
						'authorization': this.token //自定义请求头信息
					},
					success: (res) => {
						console.log('LED' + (value ? 'ON' : 'OFF') + '!');
					}
				});
			}

		}
	}
</script>

<style>
	.wrap {
		padding: 30rpx;
	}

	.dev-area {
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
		gap: 30rpx;
	}

	.dev-cart {
		height: 190rpx;
		width: 330rpx;
		border-radius: 30rpx;
		margin-top: 30rpx;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
		box-shadow: 0 0 15rpx #ccc;
	}

	.dev-name {
		font-size: 20prx;
		text-align: center;
		color: #6d6d6d;
	}

	.dev-logo {
		width: 70rpx;
		height: 70rpx;
		margin-top: 10rpx;
	}

	.dev-data {
		font-size: 30rpx;
		color: #6d6d6d;
	}
</style>
