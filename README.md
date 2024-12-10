# STM32+ESP8266/MQTT+OneNet+UniApp



​                  

## 项目概述：

##### 硬件模块：

<img src="https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241019222418826.png" alt="image-20241019222418826" style="zoom: 67%;" />

##### PCB设计：

<img src="https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241021120848977.png" alt="image-20241021120848977" style="zoom:60%;" />

##### 功能设计：

**硬件功能一览：**

PA0：key按键。PA1：DHT11数据端口。PA2、PA3：ESP-01S的数据端口，RX-TX。PA9、PA10：串口通信的数据端口，RX-TX。

PB0、PB1：OLED的SCL、SDA数据端口。

PC13：指示灯。

**软件功能一览：**

| B站选集                       | 软件功能                                            | 面包板 | 开发板 |
| ----------------------------- | --------------------------------------------------- | ------ | ------ |
| 4 点亮LED                     | 上电PC13接口的LED，点亮2.5S，后熄灭                 | Y      | Y      |
| 5 按键控制LED                 | PA1按键，进行控制PC13的LED，每次按下，LED状态翻转   | Y      | **N**  |
| 6 获取DHT11温湿度数据         | PA0端口作为DH11的数据接口，从串口接收其收发数据     | Y      | Y      |
| 7 ESP8266连接WiFi             | ESP-01S连接WIFI设置                                 | Y      | **N**  |
| 8 连接MQTT服务器              | ESP-01S连接MQTT平台                                 | Y      | **N**  |
| 11设备登录                    | ESP-01S连接到MQTT平台的具体设备、ID，密钥，设备名称 | Y      | **N**  |
| 13 上传数据到云平台           |                                                     |        |        |
| 15 解析下发消息控制LED        | OneNET平台可以下发数据控制单片机的小灯亮灭          | Y      | **N**  |
| 16 APP-生成token秘钥          |                                                     | Y      |        |
| 17 APP-从云平台获取传感器数据 | 定义方法从云平台获取相关数据                        | Y      |        |
| 18 APP-下发命令消息控制LED    | 向云平台发送数据请求，实现按钮控制单片机LED的开关   | Y      |        |
| 19 APP-界面美化               | Vue优化页面，详情见Vue开发                          | Y      |        |
| 20 APP-图标替换               | 更换Vue界面的图标                                   |        |        |
| 21 OLED显示传感器数值         |                                                     |        |        |
|                               |                                                     |        |        |





原先不合理：

1. **U3引脚错误，2,3,4脚都需要接地**

2. **USART接口：**



## 项目设计：

### 1.MQTT

> **MQTT** (Message Queuing Telemetry Transport) 是一种轻量级的 **发布/订阅**（Publish/Subscribe）消息传输协议，广泛应用于物联网（IoT）等需要低带宽、低功耗、实时传输的场景中。它设计初衷是为了解决设备与服务器之间的可靠通信问题，尤其是那些网络资源有限或不稳定的环境。

##### MQTT的核心概念

1. **发布/订阅模型**

- **发布者（Publisher）**：向某个主题（Topic）发送消息的设备或应用。发布者不需要知道谁订阅了它的消息。
- **订阅者（Subscriber）**：订阅特定主题的设备或应用，一旦有发布者向该主题发布消息，订阅者将收到该消息。订阅者不需要知道谁是发布者。
- **消息代理（Broker）**：负责接收发布者发送的消息并将其转发给订阅了相关主题的订阅者。Broker 是 MQTT 通信的核心，它充当了发布者和订阅者之间的中间人。
- **主题（Topic）**：消息发布和订阅的分类通道，通常以层次结构表示，如 `home/livingroom/temperature`。

##### MQTT的工作流程

1. **连接**：客户端（发布者或订阅者）通过 TCP/IP 或其他传输协议与 Broker 建立连接。
2. **发布消息**：发布者发布消息到一个指定的主题（Topic）。
3. **订阅主题**：订阅者向 Broker 订阅某些主题，Broker 将转发相应主题的消息给订阅者。
4. **消息路由**：Broker 收到发布者的消息后，将其转发给所有订阅了该主题的订阅者。
5. **断开连接**：当通信完成或发生错误时，客户端断开与 Broker 的连接。

##### MQTT与HTTP的区别

- **连接模型**：MQTT 是长连接，而 HTTP 是短连接，每次请求都会断开连接。MQTT 适合实时通信，而 HTTP 适合基于请求/响应的通信。
- **效率和开销**：MQTT 消息头小，带宽开销小，而 HTTP 请求的头部较大。
- **通信模式**：MQTT 使用发布/订阅模式，通信是异步的；HTTP 使用请求/响应模式，通信是同步的。



### 2. 搭建项目工程文件：

MQTT工程文件模板、





移植STM32F03相关文件、







时钟频率调节、







串口调试：波特率







精简工程代码：

如何移除工程中的驱动代码、项目，文件、头文件引用等！！！

Beep--->LED，全工程文件替换。





### 3. 外设驱动：

##### 1. LED：

> 最小系统板的PC13，LED初始化点亮2.5s，然后熄灭。

蜂鸣器的驱动代码，更改成LED的驱动代码。

最小开发板：PC13



##### 2.按键：

led.c、led.h文件改编成key 

实现PA1外接按键，对PC13进行控制，按键按下，LED灯状态翻转。





##### 3.DHT11：

**DHT11** 是一种常见的温湿度传感器，用于测量环境中的温度和湿度。它广泛应用于家庭自动化、物联网（IoT）设备、气象监测等需要实时温湿度数据的场景。DHT11 是一种低成本、易于使用的温湿度传感器，适合初学者和简单的物联网应用。虽然其测量范围和精度不如 DHT22，但对于许多普通应用来说，它已经足够实用。

**DHT11的特点**

1. **温度和湿度测量**：**湿度范围**：20-90% RH（相对湿度），精度为 ±5% RH。**温度范围**：0-50°C，精度为 ±2°C。
2. **单线通信**：DHT11 使用 **单线数字信号传输**，即通过一根信号线与主控设备（如微控制器、单片机）进行通信，减少了引脚的使用。使用一个简单的协议进行数据交换，每次发送 40 位的二进制数据，包含湿度和温度的信息。
3. **集成的传感器模块**：DHT11 内部集成了湿敏电阻和热敏电阻，用于分别测量空气中的湿度和温度。内部带有信号处理芯片，能够将传感器数据处理后转换为标准的数字信号输出。
4. **低成本和易用性**：DHT11 是一种非常经济实惠的传感器，适合各种低成本的嵌入式项目。易于使用，常见于 Arduino、STM32、ESP8266 等开发板的温湿度监测应用中。

**DHT11的工作原理**

DHT11 使用**电阻式湿度传感器**和 **NTC（负温度系数）温度传感器**来测量空气中的湿度和温度。具体流程如下：

1. **湿度测量**：DHT11 内部的湿度传感器是一种电阻式湿度传感器，其**电阻值会随空气中湿度的变化而变化**。内部芯片通过测量电阻的变化来计算当前的相对湿度。
2. **温度测量**：该传感器通过内部的 NTC 热敏电阻测量温度。**热敏电阻的阻值随温度的变化而改变**，芯片根据阻值变化来计算温度。
3. **数据传输**：
   - DHT11 每次通过单线通信发送 40 位的数据：
     - 前 16 位表示湿度（8 位整数部分，8 位小数部分）。
     - 接下来的 16 位表示温度（8 位整数部分，8 位小数部分）。
     - 最后的 8 位是校验码，用于确认数据是否正确传输。

<img src="https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241021221303600.png" alt="image-20241021221303600" style="zoom:50%;" />



移植DHT11的配置文件：`dht11.c`

```c
uint8_t DHT11_Init(void)
{	 
 	GPIO_InitTypeDef  GPIO_InitStructure;	
 	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	 //使能PG端口时钟
 	GPIO_InitStructure.GPIO_Pin = DT;				 //PA0端口配置
 	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; 		 //推挽输出
 	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
 	GPIO_Init(GPIOA, &GPIO_InitStructure);				 //初始化IO口
 	GPIO_SetBits(GPIOA,DT);						 //PG11 输出高
			    
	DHT11_Rst();  //复位DHT11
	return DHT11_Check();//等待DHT11的回应
}
```



##### 4.ESP-01S：

**AT指令**：

```c
void ESP8266_Init(void)
{
	ESP8266_Clear();
	
	UsartPrintf(USART_DEBUG, "1. AT\r\n");
	while(ESP8266_SendCmd("AT\r\n", "OK"))
		DelayXms(500);
	
	UsartPrintf(USART_DEBUG, "2. CWMODE\r\n");
	while(ESP8266_SendCmd("AT+CWMODE=1\r\n", "OK"))		//连接WIFI模式
		DelayXms(500);
	
	UsartPrintf(USART_DEBUG, "3. AT+CWDHCP\r\n");
	while(ESP8266_SendCmd("AT+CWDHCP=1,1\r\n", "OK"))	//获取IP地址
		DelayXms(500);
	
	UsartPrintf(USART_DEBUG, "4. CWJAP\r\n");
	while(ESP8266_SendCmd(ESP8266_WIFI_INFO, "GOT IP"))		//WIFI连接设置
		DelayXms(500);
	
	UsartPrintf(USART_DEBUG, "5. ESP8266 Init OK\r\n");

}
```

* 设置WiFi密码和名字



ESP-01：连接效果

![image-20241026213349543](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241026213349543.png)

手机WIFI：显示已连接设备` ESP-6EFA90`



##### 5.连接MQTT服务器：

```c
while(ESP8266_SendCmd(ESP8266_ONENET_INFO, "CONNECT"))
    
#define ESP8266_ONENET_INFO		"AT+CIPSTART=\"TCP\",\"183.230.40.96\",1883\r\n"
```

![image-20241026221202681](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241026221202681.png)

测试连接是否成功：

```c
Hardware init OK
1. AT
2. CWMODE
3. AT+CWDHCP
4. CWJAP
5. ESP8266 Init OK
    
Connect MQTTs Server...
Connect MQTT Server Success
```





### 4.ONENET服务器：

> ONENET 服务器的主要作用是提供一个全面的物联网解决方案，简化设备管理、数据处理和应用开发过程，帮助企业快速实现物联网应用的落地和运营。

产品开发->设备介入->设备开发->MQTT协议介入->最佳实践->物模型数据交互



1. **利用云服务平台，创建产品服务器。**

接入协议：MQTT

数据协议：OneJson

联网方式：Wi-Fi

开发方案：自定义方案

<img src="https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241022200247958.png" alt="image-20241022200247958" style="zoom: 50%;" />



##### 1.调试器模拟设备登陆：

MQTT.fx客户端配置，使用MQTT.fx连接设备服务器，设备状态会显示在线。

![image-20241022214123330](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241022214123330.png)



##### 2.设备登陆：



**注意栈溢出问题**：

设置数组长度过大，超过栈的大小，导致栈溢出。不同设备移植过程中，考虑栈空间适配问题。

```c
unsigned char step5data[MAX_MESSAGE_LENGTH + 128];
```



设备登陆成功：对应的OneNET平台，设备状态也会更改为在线状态。

![image-20241026224247081](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241026224247081.png)





##### 3.调试器模拟上传数据

属性上报的topic为：***$sys/{pid}/{device-name}/thing/property/post***

OneNET订阅主题，设备向OneNET上传数据，OneNET解析上传数据，保存到平台相应位置。

OneJSON请求数据格式:

```json
{
	"id": "123",
	"version": "1.0",
	"params": {
		"Power": {
			"value": "12345",
			"time": 1706673129818
		},
		"temp": {
			"value": 23.6,
			"time": 1706673129818
		}
	}
}
```



设备模拟上传格式：

```json
{
	"id": "123",
	"version": "1.0",
	"params": {
		"humi": {
			"value": 66
		},
		"temp": {
			"value": 23
		},
		"led": {
			"value": true
		}
	}
}
```



**上传结果：**

![image-20241027100326853](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241027100326853.png)





##### 4.上传数据到云平台(单片机->云平台)

```c
		if(++timeCount >= 500)									//发送间隔5s
		{
			DHT11_Read_Data(&temp,&humi);
			
			UsartPrintf(USART_DEBUG, "OneNet_SendData\r\n");
			OneNet_SendData();									//发送数据                                                                                                                                                                                                                                                                                                                  b'v'n'b
			timeCount = 0;
			ESP8266_Clear();
		}
```



```c
body_len = OneNet_FillBuf(buf);									//获取当前需要发送的数据流的总长度
```



**上传数据流设置：**

格式设置存在坑！！！

```c
extern uint8_t temp,humi;
unsigned char OneNet_FillBuf(char *buf)
{
	
	char text[48];
	
	memset(text, 0, sizeof(text));
	
	strcpy(buf, "{\"id\":\"123\",\"params\":{");
	
	memset(text, 0, sizeof(text));
	sprintf(text, "\"temp\":{\"value\":%d},", temp);
	strcat(buf, text);
	
	memset(text, 0, sizeof(text));
	sprintf(text, "\"humi\":{\"value\":%d},", humi);
	strcat(buf, text);
	
	memset(text, 0, sizeof(text));
	sprintf(text, "\"led\":{\"value\":%s}", led_info.Led_Status ? "true":"false");
	strcat(buf, text);
	
	strcat(buf, "}}");
	
	return strlen(buf);

}
```



<font color='red'>topic_buf？？？</font>



##### 5.调试器接收下发控制消息



设备侧需要收到平台下发的数据，需要订阅：
`$sys/{pid}/{device-name}/thing/property/set`

调试器订阅平台信息，设备调试时，会收到相应的调试信息。



![image-20241027125640793](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241027125640793.png)



##### 6.解析下发消息控制LED

CJSON？？？



* 订阅发送相关主题：

* ESP8266_GetIPD：OneNET发送数据到ESP-01S，ESP-01S再通过串口把数据发送给单片机



```c
result = MQTT_UnPacketPublish(cmd, &cmdid_topic, &topic_len, &req_payload, &req_len, &qos, &pkt_id);
```

* req_payload



* CJSON文件，解析文件代码

```c
	cJSON *raw_json = cJSON_Parse(req_payload);	
	cJSON *params_json = cJSON_GetObjectItem(raw_json,"params");
	cJSON *led_json = cJSON_GetObjectItem(params_json,"led");
	if(led_json->type ==  cJSON_True) Led_Set(LED_ON);
	else Led_Set(LED_OFF);

	cJSON_Delete(raw_json);
```





### 5.APP界面设计

HBuilder X开发软件：

* Uin-app的开发模板，进行app/小程序的配置。





##### 1.获取token：安全鉴权

* API接口-SDK：`onenet-studio-api-node-sdk-master.zip`

  * | Node | V1.0.0 | [onenet-studio-api-node-sdk](https://github.com/cm-heclouds/onenet-studio-api-node-sdk) |
    | ---- | ------ | ------------------------------------------------------------ |

  * `onenet-studio-api-node-sdk-master\onenet-studio-api-node-sdk-master\src\Key.js`

**Token** 在计算机科学和网络通信中通常指代一种用于身份验证和授权的安全凭证。它在多种场景中有不同的含义和应用，主要包括：

1. **身份验证与授权** ：Token 常用于用户登录后，作为访问权限的凭证。用户在身份验证后会收到一个 Token，用于后续的 API 调用，而不必每次都提供用户名和密码。

2. **JWT（JSON Web Token）**：JWT 是一种常见的 Token 格式，包含了用户身份和权限信息。它通常由三部分组成：头部（Header）、有效载荷（Payload）和签名（Signature）。JWT 可以用于单点登录（SSO）等场景。

3. **API 访问**：在 RESTful API 中，Token 常用于控制对资源的访问。客户端在每次请求时附带 Token，服务器根据 Token 验证请求的合法性。

4. **安全性**：Token 提供了一种较为安全的方式来管理会话和权限，减少了明文传输敏感信息的风险。Token 通常是短期有效的，可以设置过期时间，提高安全性。

5. **刷新 Token**：为了提高安全性，系统通常会使用一个短期有效的访问 Token 和一个长期有效的刷新 Token。访问 Token 用于实际的 API 调用，而刷新 Token 用于获取新的访问 Token。

总结：Token 是现代应用程序中身份验证和授权的核心组件，能有效地管理用户会话和权限，同时提高系统的安全性和灵活性。

```json
const {
		createCommonToken
	} = require('@/key.js')
	export default {
		data() {
			return {
				title: 'Hello',
				token: '',
			}
		},
		onLoad() {
			const params = {
				author_key: 'cjKZ0SZdKEiSRbQhknuvnYHhx95cF+QMucPOPYQCKePaGiZAthS45l7I3VEh7lsr8Q5x9vHY62ZK6SUFdzQjzQ==',
				version: '2022-05-01',
				user_id: '408733',
			}
			this.token = createCommonToken(params)
			console.log(this.token)
		},methods: {
		}
    }
```



##### 2.云平台获取传感器数据：

初始化页面、请求服务器数据、刷新页面内容

* 定义组件，温度、湿度、led状态，在APP内进行显示，变量形式。
* 设计方法：获取数据变量
* 平 台API->接口详情->物模型使用->设备属性最新数据查询

> 安全鉴权authorization由多个参数构成，每个参数均采用key = value的形式表示，并用&作为分隔符。

```js
	methods: {
			fetchDevData(){
				//发起网络请求
				uni.request({
			url: 'https://iot-api.heclouds.com/thingmodel/query-device-property', //仅为示例，并非真实接口地址。
				    method:'GET',
					data: {
						product_id:'GH98oDk7fb',			//设备ID
						device_name:'DuZhong1'				//产品名称
				    },
				    header: {
				        'authorization': this.token //自定义请求头信息
				    },
				    success: (res) => {
				        console.log(res.data);
						console.log(res.data.data[0].value);
						this.humi = res.data.data[0].value;
						this.led = res.data.data[1].value === 'true';
						this.temp = res.data.data[2].value;
				    }
				});

			}

		}
	}
```

返回的数据：

```js
data: Array(3)
0:
	access_mode: "只读"
	data_type: "int32"
	identifier: "humi"
	name: "湿度"
	__proto__: Object
1:
    access_mode: "读写"
    data_type: "bool"
    identifier: "led"
    name: "状态指示灯"
    __proto__: Object
```

* onShow：定时调用获取数据函数





##### 3.下发命令消息控制LED



* 向云服务平台下发命令的请求
* LED开关变化的动作绑定函数

> detail: {value: false}

```json
onLedSwitch(event){
				console.log(event.detail.value);
				let value = event.detail.value;
				//发起网络请求
		 	uni.request({
			url: 'https://iot-api.heclouds.com/thingmodel/set-device-property', //下发数据所用的url地址
				    method:'POST',
					data: {
						product_id:'GH98oDk7fb',
						device_name:'DuZhong1',
						params:{"led":value}
				    },
				    header: {
				        'authorization': this.token //自定义请求头信息
				    },
				    success: (res) => {
				        console.log();
				    }
				});
			}
```

* 时间流的控制
  * 覆盖命令，同一个缓存接收命令，数据上传太快，会导致命令消息还未处理，就被下一个命令覆盖了。

##### 4.界面美化+图标替换

JS美化界面，Vue开发，英文单词的记忆。

前端页面美化：

![image-20241027164131983](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241027164131983.png)





BUG：信息传输不流畅，单片机数据发生变化，而Web端并没有变化，需要不断刷新才能变化状态。

请求数据的时间设置的不合理。



### 6.OLED显示设计



<font color='red'>心得：CRUD确实香，还是现成驱动好用啊！！！</font>



##### 1.OLED显示传感器数值

* 移植OLED驱动
* PB0、PB1，作为重定义的II2端口



* 重新排版OLED屏幕显示，中景电子的相关插件-----**生成字模**，具体效果如下

![image-20241027205259747](https://haiyang-l.oss-cn-beijing.aliyuncs.com/img/image-20241027205259747.png)

* 移植到源文件



* 温度、湿度、指示灯，需要重定位显示字符，根据接收到的数据进行刷新显示。

```c
void Refresh_Data(void)			//不断刷新屏幕显示值
{
	char buf[10];
	sprintf(buf, "%2d", temp);
	OLED_ShowString(54,0,buf,16);		//温度值
	
	sprintf(buf, "%2d", humi);
	OLED_ShowString(54,3,buf,16);		//湿度值
	

	if(led_info.Led_Status) OLED_ShowCHinese(72,6,9);
	else OLED_ShowCHinese(72,6,10);	
}
```





##### 2.OLED显示初始化信息



* 串口打印的信息转为OLED打印
