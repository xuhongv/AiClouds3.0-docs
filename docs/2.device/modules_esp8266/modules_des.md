## 关于模板的说明

&nbsp;&nbsp;&nbsp;&nbsp;我们安信可的 AiClouds3.0 的设计初衷是：

- **跨平台、开发快、全开源**的宗旨；
- 微信公众号内 `airkiss` 配网和 `MQTT` 控制；
- 微信小程序内 `smartConfig` 配网和 `MQTT` 控制；
- 设备端要求：支持 `airkiss` 配网和 `MQTT` 协议即可；
- 服务器端：有微信公众号业务和各云平台对接的业务，支持但不限于 天猫精灵、小爱同学、小度音箱、Alexa音箱等；
- 扩展性强，支持用户二次开发，可私定义协议；
- 三端开源：设备端、服务器端、前端；


&nbsp;&nbsp;&nbsp;&nbsp;但，在设备端就一个工程以及少量的代码，满足不了大家的好学之心。所以，我们决定整理上国内各种云的代码模板给大家；

&nbsp;&nbsp;&nbsp;&nbsp;<font color="red" size="4">AiClouds3.0架构的出厂固件是 模板2，设备端的模块代码（模板2除外）与AiClouds3.0 架构无直接关系，均来自我司的日常整理；</font>

# FAQ常见疑问

- Q1：这些模板是否可以商用？

&nbsp;&nbsp;&nbsp;&nbsp;首先说明这些模板都是我们自行测试完毕后放上去的，代码工程是在原厂的基础上二次开发，具体对应原厂哪个工程，请详情见 README 文档，可商用，具体的各种云平台上架流程还需与其云平台具体协议有关，如有整体上架云平台的各种测试需求，可联系我司商务：support@aithinker.com

- Q2：这些模板那么多，有对应的开发板吗？

&nbsp;&nbsp;&nbsp;&nbsp;我们有提供开发板 NodeMCU ESP8266：https://s.click.taobao.com/u4iYujv ，板载 RGB 三色灯，但具体的按键需要自行搭建；

- Q3：这些模板都是`C`语言`SDK`开发的，是否有 `Arduino` 开发的版本？

&nbsp;&nbsp;&nbsp;&nbsp;我们不做 `Arduino` 开发的方式，也没有这个意向做`Arduino`版本；

- Q4：是否有开发者交流群？ 如何咨询解决开发遇到的问题？

&nbsp;&nbsp;&nbsp;&nbsp;因咨询技术过多，购买我司开发板后，咨询淘宝客服加群；

- Q5：以上模板满足不了我的需求，或者我如何共享我的代码？ 

&nbsp;&nbsp;&nbsp;&nbsp;联系安信可开源团队: xuhongv@aithinker.com