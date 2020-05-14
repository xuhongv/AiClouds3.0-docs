

## 1 材料准备

- 首先您必须具备设备端的开发环境，可以是`Linux`或者`Windows`，可参考前面的环境搭建章节！
- NodeMCU ESP8266：https://s.click.taobao.com/u4iYujv ，板载 RGB 三色灯，但具体的按键需要自行搭建；
- 自行搭建MQTT服务器，可在本地搭建或远程主机搭建；
- LED一盏，使用 3~3.3V 即可；

## 2 模板说明

 工程SDK：[AiThinkerProjectForESP](https://gitee.com/xuhongv/AiThinkerProjectForESP)

 源码位于：[Ai-examples/1.SmartConfig_AirKiss_To_MQTT](https://gitee.com/xuhongv/AiThinkerProjectForESP/tree/master/Ai-examples/1.SmartConfig_AirKiss_To_MQTT)

 这是可以用`esp-touch`或微信`airkiss`配网以及近场发现的功能和连接MQTT服务器的的模板demo示范！ 

 并可以远程控制 `ESP8266` 点亮一盏LED；
 
 按键接线 `GPIO0` 引脚下降沿触发，`LED`的正极接 `GPIO12`，负极接 `GND`；

 按键短按 ，改变灯具状态并上报状态到`MQTT`服务器；

 按键长按 ，进去配网模式；

 - 搜索 "安信可科技" 微信公众号点击 `WiFi` 配置；

 - 或者下载一键配网APP点击配置设备；


 ### 联系方式：
 
 有任何技术问题请在这里提出：https://github.com/xuhongv/AiClouds3.0-docs/issues
 
 或邮箱联系我们：support@aithinker.com @team: Ai-Thinker Open Team 安信可开源团队
 

## 上报指令协议

- 主题：/aithinker/${设备mac地址}/devPub
- 设备上报关灯：

```
{
	"header": {
		"type": "aithinker",
		"mac": "60019421dc59"
	},
	"attr": [{
		"name": "powerstate",
		"value": "off"
	}]
}
```

- 主题：/aithinker/${设备mac地址}/devPub
- 设备上报开灯：

```
{
	"header": {
		"type": "aithinker",
		"mac": "60019421dc59"
	},
	"attr": [{
		"name": "powerstate",
		"value": "on"
	}]
}
```
## 控制指令

- 控制开灯
- 主题：/aithinker/${设备mac地址}/devSub

```
{
	"msg": 1
}
```

- 控制关灯
- 主题：/aithinker/${设备mac地址}/devSub

```
{
	"msg": 0
}
```

# 联系方式

- 扫描加钉钉交流群：

![在这里插入图片描述](../_media/quick/dingding_qr.JPG ':size=300x400')