## 一、前言

&nbsp;&nbsp;&nbsp;&nbsp;看了这么丰富的应用场景之后，如何快速体验呢？

&nbsp;&nbsp;&nbsp;&nbsp;目睹为快，效果演示视频正在筹划中；


# 二、快速体验

&nbsp;&nbsp;&nbsp;&nbsp;跟着步骤，不要问为什么，这步这步是干嘛的？原理是什么？后续，慢慢给大家讲解下原理，请不要心急！

## 2.0 编译烧录固件

&nbsp;&nbsp;&nbsp;&nbsp;已为大家准备好了设备的 [源码](https://github.com/xuhongv/xClouds-device/tree/master/Ai-examples/2.xClouds-template)，但需自行烧录，后续会提供固件自行烧录；

&nbsp;&nbsp;&nbsp;&nbsp;基于 esp-idf esp8266芯片 rtos3.0 sdk 开发，配合 xClouds-php 可实现微信配网绑定控制 + 天猫精灵语音控制 + 小爱同学控制；

&nbsp;&nbsp;&nbsp;&nbsp;这是微信`airkiss`配网以及近场发现的功能和连接MQTT服务器的的demo示范！

&nbsp;&nbsp;&nbsp;&nbsp;LED接线参考 `XPWM.h` 头文件定义，按键接线 `GPIO0` 下降沿有效；

&nbsp;&nbsp;&nbsp;&nbsp;按键长按 ，进去配网模式，微信扫码下面微信公众号二维码点击添加设备；


### 上报指令

- 主题：/rgbLight/${设备mac地址}/devPub
- 设备上报格式：

```
{
	"header": {
		"type": "rgbLight",
		"fw": "12.5",
		"mac": "6001947a70a7"
	},
	"attr": [{
			"name": "powerstate",
			"value": "on"
		},
		{
			"name": "colorTemperature",
			"value": "4000"
		},
		{
			"name": "mode",
			"value": "nightLight"
		},
		{
			"name": "brightness",
			"value": "100"
		},
		{
			"name": "color",
			"value": "Yellow"
		}
	]
}
```
### 控制指令

- 控制开灯
- 主题：/aithinker/${设备mac地址}/devSub


paylaod：

```
// 开灯
{
	"header": {
		"name": "TurnOn",
		"namespace": "AliGenie.Iot.DeviceCenter.Control",
		"payLoadVersion ": 1
	},
	"payload": {
		"attribute": "powerstate",
		"deviceId": "9",
		"deviceType": "light",
		"value": "0"
	}
}
```

------------------

```
// 关灯
{
	"header": {
		"name": "TurnOff",
		"namespace": "AliGenie.Iot.DeviceCenter.Control",
		"payLoadVersion ": 1
	},
	"payload": {
		"attribute": "powerstate",
		"deviceId": "9",
		"deviceType": "light",
		"value": "0"
	}
}
```

--------------

```
// 设置颜色为拉蓝色
{
	"header": {
		"name": "SetColor",
		"namespace": "AliGenie.Iot.DeviceCenter.Control",
		"payLoadVersion ": 1
	},
	"payload": {
		"attribute": "color",
		"deviceId": "9",
		"deviceType": "light",
		"value": "Blue"
	}
}
```


## 2.1 微信公众号绑定设备

&nbsp;&nbsp;&nbsp;&nbsp;想要体验语音控制怎么可以没有真实设备，以安信可 ESP8266 NodeMCU 开发板为例，下载烧录工具 ；自行某宝淘一个；

&nbsp;&nbsp;&nbsp;&nbsp;第一步：我们先让设备进去微信airkiss配网模式，按键长按三秒以上，待设备会呼吸闪烁，说明进去配网模式；

&nbsp;&nbsp;&nbsp;&nbsp;第二步：微信扫描以下二维码;

![](https://img-blog.csdnimg.cn/2020050709361963.png ':size=200')


&nbsp;&nbsp;&nbsp;&nbsp;如果添加失败或超时提示，排除以下原因：

1. 路由器Wi-Fi信道是否为5G频段？
2. 手机是否开启定位功能？微信是否被授权定位权限；
3. 尝试换个路由器，或者手机开启热点；

&nbsp;&nbsp;&nbsp;&nbsp;添加成功之后，返回个人列表界面，打开设备列表界面，此刻会显示您刚刚添加的设备，这时候，您可打开它在里面控制它啦！这里特别说明：**因个人的服务器资源有限，我只给普通用户仅能绑定三个设备的权限，还望谅解。** 

&nbsp;&nbsp;&nbsp;&nbsp;如果上面完全没问题，恭喜，成功了第一步！下面，我们开始进行天猫精灵 控制设置；

## 2.2 天猫精灵配置（平台支持任何组织包括个人）

与各大服务器的对接是采用 云云对接方式，而未上架是不可以对所有人所见的，所以，大家跟着我步骤，在天猫精灵云后台设置下我目前的环境参数；

准备材料：
- 应用商店下载天猫精灵APP；
- 自行购买天猫精灵音箱，连接天猫精灵智能音箱并完成配网绑定；

- 1、进去天猫精灵云后台，淘宝账号登录：[https://open.aligenie.com/console/skill/list](https://open.aligenie.com/console/skill/list)
- 2、添加新技能，类型务必为 **智能家居**，名字随便起；
- 3、按照如下截图配置，每项认认真真填好完毕！

```
账户授权连接：https://aligenie.xuhongv.com/oauth/aligenie
Client ID：aithinker
Client Secret：xuhong2020
跳转 URL：https://open.bot.tmall.com/oauth/callback

Access Token URL：https://aligenie.xuhongv.com/oauth/token
开发者网关地址：https://aligenie.xuhongv.com/oauth/AliGenieGateWay
```

![](https://img-blog.csdnimg.cn/20200507093826652.png)


- 4、开始测试同步验证，确保您已经添加了设备，然后再微信公众号个人中心点击 “获取授权码”，注意 大小写，输入授权界面，如下界面：

![upl-image-preview ](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9kb2NzLmFpLXRoaW5rZXIuY29tL19tZWRpYS9haWNsb3VkL2FsaWdlaWUtY29uaWZpZy5wbmc?x-oss-process=image/format,png)

- 5、然后在天猫精灵APP上面找到此设备，修改此设备名，后续将可以通过此设备名字，来语音控制设备啦；

## 2.3 小米IoT平台配置（平台仅支持企业，不支持个人开发者）

**平台仅支持企业，不支持个人开发者**，如若您没有经过小米认证的企业账号，请跳过此小节；

第一步：在小米开放平台注册账号：[点击进去。](https://iot.mi.com/new/doc/introduction)

第二步：点击 “**已上市非连接小米IoT的产品接入小爱同学**” 方式接入，以 云对云接入；新建产品如下图所示；PS：不够清晰的请右击图片在新的标签页打开放大查看；

![upl-image-preview url=](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9kb2NzLmFpLXRoaW5rZXIuY29tL19tZWRpYS9haWNsb3VkL3Nkay8lRTclQjElQjMlRTUlQUUlQjYlRTUlOTAlOEUlRTUlOEYlQjAlRTYlOTYlQjAlRTUlQkIlQkElRTYlODglQUElRTUlOUIlQkUucG5n?x-oss-process=image/format,png)

第三步：然后，我们在后台找到 [云云对接的参数链接界面设置](https://iot.mi.com/fe-op/appCenter/auth)，如下：

```
1. 账号授权URL：https://aligenie.xuhongv.com/oauth/miot
2. Client ID：miot
3. Client Secret：xuhong2020
4. Access Token URL：https://aligenie.xuhongv.com/oauth/token
5. Refresh URL：https://aligenie.xuhongv.com/oauth/token
6. 设备指令接受URL：https://aligenie.xuhongv.com/oauth/MiotGateWay
```
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9kb2NzLmFpLXRoaW5rZXIuY29tL19tZWRpYS9haWNsb3VkL3Nkay8lRTclQjElQjMlRTUlQUUlQjYlRTUlOTAlOEUlRTUlOEYlQjAlRTYlOTYlQjAlRTUlQkIlQkElRTYlODglQUElRTUlOUIlQkUucG5n?x-oss-process=image/format,png)



第五步：最后设置如下图所示；PS：不够清晰的请右击图片在新的标签页打开放大查看；
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9kb2NzLmFpLXRoaW5rZXIuY29tL19tZWRpYS9haWNsb3VkL3NwZWMvJUU3JUIxJUIzJUU1JUFFJUI2b2F1dGgyLjAucG5n?x-oss-process=image/format,png)

第六步：请确保把你的账号添加进去 后台的白名单，然后打开米家APP登录您账号，如下找到第三方设备添加；
             **PS：不够清晰的请右击图片在新的标签页打开放大查看；**

![upl-image-preview url=http://cdn.cnbj0.fds.api.mi-img.com/miio.files/commonfile_png_df7f2073d34d9b3c5a289ebb68c0e13d.png](https://imgconvert.csdnimg.cn/aHR0cDovL2Nkbi5jbmJqMC5mZHMuYXBpLm1pLWltZy5jb20vbWlpby5maWxlcy9jb21tb25maWxlX3BuZ19kZjdmMjA3M2QzNGQ5YjNjNWEyODllYmI2OGMwZTEzZC5wbmc?x-oss-process=image/format,png)

第七步：确保您已经添加了设备，然后再微信公众号个人中心点击 “获取授权码”，注意 大小写，输入授权界面，如下界面：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9kb2NzLmFpLXRoaW5rZXIuY29tL19tZWRpYS8lRTYlOEUlODglRTYlOUQlODMlRTclQjElQjMlRTUlQUUlQjYlRTYlOTMlOEQlRTQlQkQlOUMucG5n?x-oss-process=image/format,png)

第八步：点击完成同步设备，就会出现你的设备列表啦！就可以小爱同学语音控制啦！注意不支持米家APP控制哈！

第九步：同时，还支持米家后台控制：

![upl-image-preview url=https://docs.ai-thinker.com/_media/aicloud/spec/%E7%B1%B3%E5%AE%B6%E5%90%8E%E5%8F%B0%E8%B0%83%E8%AF%95.jpg](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9kb2NzLmFpLXRoaW5rZXIuY29tL19tZWRpYS9haWNsb3VkL3NwZWMvJUU3JUIxJUIzJUU1JUFFJUI2JUU1JTkwJThFJUU1JThGJUIwJUU4JUIwJTgzJUU4JUFGJTk1LmpwZw?x-oss-process=image/format,png)

- end

# 联系方式：

- 技术疑问请发疑问到邮箱： xuhongv@aithinker.com