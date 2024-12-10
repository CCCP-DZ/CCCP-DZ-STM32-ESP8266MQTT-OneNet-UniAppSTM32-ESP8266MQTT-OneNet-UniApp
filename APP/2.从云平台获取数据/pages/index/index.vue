<template>
	<view class="content">
		<view class="">温度{{temp}}℃</view>
		<view class="">湿度{{humi}}%</view>	
		<switch :checked="led" @change="" />
	</view>
</template>

<script>
	const {
		createCommonToken
	} = require('@/key.js')
	export default {
		data() {
			return {
				temp:'11',
				humi:'23',
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
		onShow(){
			//向服务器请求数据
			this.fetchDevData();
			
			//定时进行数据请求
			setInterval(()=>{
				this.fetchDevData();
			},10000)
			
		},
		methods: {
			fetchDevData(){
				
				//发起网络请求
				uni.request({
				    url: 'https://iot-api.heclouds.com/thingmodel/query-device-property', //仅为示例，并非真实接口地址。
				    method:'GET',
					data: {
						product_id:'GH98oDk7fb',
						device_name:'DuZhong1'
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

			}

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
