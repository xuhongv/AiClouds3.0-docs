## 1 材料准备

- 首先您必须具备设备端的开发环境，可以是`Linux`或者`Windows`，可参考前面的环境搭建章节！
- NodeMCU ESP8266：https://s.click.taobao.com/u4iYujv ，板载 RGB 三色灯，但具体的按键需要自行搭建；
- 自行搭建MQTT服务器，可在本地搭建或远程主机搭建；
- LED一盏，使用 3~3.3V 即可；


## 2 模板说明

 工程SDK：[AiThinkerProjectForESP](https://gitee.com/xuhongv/AiThinkerProjectForESP)

 源码位于：[Ai-examples/2.xClouds-template](https://gitee.com/xuhongv/AiThinkerProjectForESP/tree/master/Ai-examples/2.xClouds-template)

 @功能：MQTT 协议连接 AiClouds3.0 系统，可实现天猫精灵/小爱等第三方音平台控制；


 ## 3 原理

 天猫精灵/小爱等第三方等第三方平台控制的协议适配是在 云端服务器已经完成，设备端无需关心细节，只需要按照私有协议的上报和下发即可；


```
                             ----->
 设备端出厂 ----> 用户配网绑定 ----->
                           ∟_----->


```

# 联系方式：

- 技术疑问请发疑问到邮箱： xuhongv@aithinker.com