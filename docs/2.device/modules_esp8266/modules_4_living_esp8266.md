## 材料准备

- 首先您必须具备设备端的开发环境，可以是`Linux`或者`Windows`，可参考前面的环境搭建章节！
- NodeMCU ESP8266：https://s.click.taobao.com/u4iYujv ，板载 RGB 三色灯，但具体的按键需要自行搭建；
- 自行搭建MQTT服务器，可在本地搭建或远程主机搭建；
- LED一盏，使用 3~3.3V 即可；


----------

# 一 前言
&nbsp;&nbsp;&nbsp;&nbsp;很多时候，开发效率不得不说是提高生产力的重要要素。而且，很多客户都不想要 Linux 开发，所以很多这样的人来问我，因此在 `windows` 环境下IDE编译代码，可以上阿里飞燕那种，毋庸置疑，这是可以的；

# 二 下载阿里飞燕直连的SDK

&nbsp;&nbsp;&nbsp;&nbsp;前面文章，已经教会大家如何简单使用 **IDE 1.5**版本，现在我们拉取下SDK，此SDK是源自乐鑫仓库二次整理。


&nbsp;&nbsp;&nbsp;&nbsp;通过 **git** 拉取代码，务必带子模块下载哦！

```C
git clone --recursive https://gitee.com/xuhongv/aithinker-aliyun.git
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519162255937.png)

## 2.1 导入步骤：

1. 点击C/C++分支，选择 `Existing Code as MakeFile Project` 工程;
2. 复制刚刚的下载的文件夹路径，`import -->  Cross Gcc` , 并且去掉对应的 `C++` 勾勾；
3. 导进刚刚下载的文件夹，如图所示：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519172712660.png)

## 2.2 配置环境步骤：

 1. 项目属性设置，鼠标选中项目名称右键点击，在右侧菜单中选择Properties 
 2. 在 `Properties --> C/C++ Build --> Build directory` 选择编译的工程路径，比如 `aithinker-aliyun/esp-aliyun/examples/solutions/smart_light` 工程。
 3. 添加IDF环境变量在 `Properties --> C/C++ Build --> Environment` 点击Add ，路径为刚刚的下载的文件夹路径，变量名字为 `IDF_PATH`;
 4. 然后点击 OK 保存退出！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519174416574.png)
- 指定的依赖 **IDF_PATH** 路径：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519174306961.png)

## 2.3 编译步骤：

&nbsp;&nbsp;&nbsp;&nbsp;与其他版本不一样， rtos3.0或以上支持面板设置参数，即通过 `make menuconfig` 设置，同样地，我们可以利用快捷键去快速设置；

 1. 构建menuconfig菜单，选中项目名称，在右键菜单中选择 `Make Targets --> Create`；或者快捷键 `Alt + F9` ;
 2. 在弹出的对话框中取消勾选`Same as the target name` 与 `User builder settings` 这2个选项，并且在`Build command` 中输入
   ```C
   mintty.exe -e make chip=esp8266 defconfig
   ```
 3. 同样的操作，添加如下指令，表示进去控制面板： :
  ```C
  mintty.exe -e make menuconfig
  ```
 4. 同样的操作，添加如下指令，表示编译阿里云三元组为bin文件；
 ```C
 mintty.exe -e ../../../../components/nvs_flash/nvs_partition_generator/nvs_partition_gen.py --input ../../../config/mass_mfg/single_Mfg_config.csv --output single_mfg.bin --size 0x4000
 ```
 5. 同样的操作，添加如下指令，表示烧录阿里云三元组bin文件进去设备；
 ```C
 mintty.exe -e ../../../../components/nvs_flash/nvs_partition_generator/nvs_partition_gen.py --port /dev/ttyUSB0 write_flash 0x100000 single_mfg.bin
  ```
 6. 如何编译？先 **shift + F9** 出现一个弹窗，依次点击第二点/第三点创建的项目；最后右击工程 **build**  即可！


&nbsp;&nbsp;&nbsp;&nbsp; **如下动图操作所示（忽略文件夹名字）：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212155920148.gif)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519180118283.png)

----------------------
# 三 阿里云IoT生态

&nbsp;&nbsp;&nbsp;&nbsp;也许你会问，想要天猫精灵语音控制，为啥要搞这个阿里飞燕，这里我给大家粗略普及下阿里飞燕是怎么样的平台。

&nbsp;&nbsp;&nbsp;&nbsp;阿里云物联网平台和阿里物联网生活平台（又名阿里飞燕）都是阿里云旗下的产品；天猫精灵 IoT 平台归属天猫旗下，非阿里云旗下产品；但都可以天猫精灵语音控制，其数据已打通；

&nbsp;&nbsp;&nbsp;&nbsp; **很强大的是，我们的IDE都支持这三个平台的接入！！**

## 3.1 阿里云物联网平台：
&nbsp;&nbsp;&nbsp;&nbsp;阿里云物联网平台提供了一站式的设备接入、设备管理、监控运维、数据流转、数据存储等服务，数据按照实例维度隔离，可根据业务规模灵活提升规格，具备高可用性、高并发、高性价比的特性，是企业设备上云的首选。

## 3.2 阿里物联网生活平台：
&nbsp;&nbsp;&nbsp;&nbsp;阿里云 IoT 提供了一款针对消费领域的物联网平台，即生活物联网平台，以解决家电设备快速智能化的问题。平台针对家电智能化的设备连接、移动端控制、设备管理、数据统计等问题，打包阿里云多款产品，提供了一整套配置化方案，大幅减低“设备-云端-App”的开发成本。

**两者的区别：**

&nbsp;&nbsp;&nbsp;&nbsp;生活物联网平台 和 物联网平台 均为阿里云 IoT 提供的云服务平台，两个平台各自优势和使用场景如下。

&nbsp;&nbsp;&nbsp;&nbsp;物联网平台提供原子化的设备接入能力，适用于云开发能力较强的用户，可以在各个行业领域使用。了解更多详情请参见 什么是物联网平台 。
生活物联网平台提供了设备接入能力、移动端的 SDK 以及免开发的公版 App 和界面，更适用于消费级的智能设备开发者，开发门槛较低，可以快速实现消费级设备的智能化，如智能家电、穿戴、家装领域等。

&nbsp;&nbsp;&nbsp;&nbsp;使用同一个阿里云账号登录的用户，在生活物联网平台创建的所有产品和设备，将自动同步到物联网平台中。而在物联网平台中创建的产品，也可以通过手动切换收费模式，将产品转移到生活物联网平台中。

## 3.3 天猫云IoT平台（原赤兔平台）
&nbsp;&nbsp;&nbsp;&nbsp;AliGenie 智能应用开发平台是阿里巴巴人工智能实验室(AI-Labs)面向软硬件厂商和开发者推出的，将人工智能中 ASR（语音识别）、NLP（自然语言处理）、TTS（语音合成）等自然语言处理技术整合、将 AI 能力和设备控制能力对外共享的开放式平台，帮助开发者以最高效率创建智能应用。

&nbsp;&nbsp;&nbsp;&nbsp; AliGenie 平台中的 IOT 接入开放平台，也称 	天猫精灵 IoT 开放平台，	是阿里巴巴人工智能实验室(Alibaba A.I.Labs)面向品牌商、方案商、模组商/芯片商、三方平台商以及个人开发者推出的，将 IoT 物联网技术（蓝牙协议、WiFi 协议、云服务）和 AI（天猫精灵 ASR 语音识别、NLP 自然语言处理、TTS 语音合成）等对外输出的开放式平台。

&nbsp;&nbsp;&nbsp;&nbsp;开发者可按直连接入（WiFi 模组、蓝牙模组）、云云接入（OAuth2.0）2 类方式，接入天猫精灵软硬件生态（天猫精灵音箱、天猫精灵 App、天猫精灵车机及 AliGenie Inside 智能设备）及阿里巴巴集团生态服务，实现语音、触屏、多模态交互，为用户提供控制、查询、播报、场景与主动服务。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520084223657.png)

---------------
# 四 阿里飞燕平台创建产品

#### 1.参考上述的资料进行硬件准备、环境搭建、SDK 准备
#### 2.阿里云平台部署
在阿里云 [生活物联网平台](https://living.aliyun.com/#/) 创建产品, 参考[创建产品文档](https://living.aliyun.com/doc#readygo.html).
> 配置较多, 如果不太懂, 也不用纠结, 后续都可以修改.

部署自己的产品, 可参考如下:
新增 RGB 调色功能:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520085600316.png)

新增测试设备, 此处即可以获得`三元组`, 后续需要烧录到 NVS 分区.
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-7kiRBzaj-1589936094333)(_static/p2.png)\]](https://img-blog.csdnimg.cn/2020052008574337.png)
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-gB654jOA-1589936094335)(_static/p3.png)\]](https://img-blog.csdnimg.cn/20200520085752636.png)

选择面板, 手机 APP 上会显示同样界面; `配网二维码`是贴在产品包装上, 终端客户给设备配网中需扫描此二维码.；
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-6Wh8IPYh-1589936094337)(_static/p4.png)\]](https://img-blog.csdnimg.cn/20200520085803908.png)

选择面板时, 主题面板在手机上仅能显示标准界面, 没有 RGB 调色功能. 可以自定义面板, 增加 RGB 调色.
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-4jw62xc9-1589936094338)(_static/p5.png)\]](https://img-blog.csdnimg.cn/20200520085813496.png)

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-3a9q3vwq-1589936094339)(_static/p6.png)\]](https://img-blog.csdnimg.cn/20200520085821591.png)

配网方案选择:
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-31IYYOzN-1589936094340)(_static/p7.png)\]](https://img-blog.csdnimg.cn/20200520085828439.png)

完成
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-1cWOx2Bd-1589936094341)(_static/p8.png)\]](https://img-blog.csdnimg.cn/2020052008584733.png)

---------------
# 五 编译过程

## 1.按照前面步骤搭建安信可一体化环境


## 2.烧录四元组信息
`mass_mfg` 目录中有一参考配置：`single_mfg_config.csv`，请把里面的内容成自己的四元组配置文件：

使用自己的 ProductKey、ProductSecret、DeviceName、DeviceSecret 对 single_mfg_config.csv 进行修改：

```c
key,type,encoding,value
aliyun-key,namespace,,
DeviceName,data,string,config
DeviceSecret,data,string,dsj3RuY74pgCBJ3zczKz1LaLK7RGApqh
ProductKey,data,string,a10BnLLzGv4
ProductSecret,data,string,pVfLpS1u3A9JM0go
```

将 `config，dsj3RuY74pgCBJ3zczKz1LaLK7RGApqh，a10BnLLzGv4，pVfLpS1u3A9JM0go` 修改为你对应的值。

> 如果执行了 `make erase_flash`, 需要重新烧录三元组.

## 3.配置 `smart light example`
- 在安信可淘bao店阿里云专区购买 `NodeMCU`开发板，自带`RGB` 灯，或自行接线分别接 `ESP8266` 开发板上 `GPIO4`, `GPIO5`, `GPIO2` (可在 `lightbulb.c` 中修改)；

## 4.编译 `smart light` 并烧录运行

- **shift + F9** 出现一个弹窗，依次点击之前创建的四个项目，或者命令格式一次敲：

```
//设置为ESP8266配置
make chip=esp8266 defconfig
//面板设置
make menuconfig
//编译三元组
 mintty.exe -e ../../../../components/nvs_flash/nvs_partition_generator/nvs_partition_gen.py --input ../../../config/mass_mfg/single_Mfg_config.csv --output single_mfg.bin --size 0x4000
 //烧录三元组
 mintty.exe -e ../../../../components/nvs_flash/nvs_partition_generator/nvs_partition_gen.py --port /dev/ttyUSB0 write_flash 0x100000 single_mfg.bin
```


## 5.设备第一次运行时, 会进入配网
- - 用安信可串口工具打开，有下面的打印，则说明进去配网模式；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520091430314.png)

## 6. 下载APP
- 手机从[阿里云官网](https://living.aliyun.com/doc#muti-app.html) 下载 `云智能` 公版 APP, 国内用户版.

## 7.注册好账号后,进入 APP, 右上角扫描, 扫描第二步的二维码配网.
- 设备端配网成功后会保存 `ssid` 和 `password` :

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520091746660.png)

- 设备与手机绑定成功后, APP 上会弹出灯的配置页面. 返回主页显示灯 `在线`.  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520092002956.png)

## 8.控制智能灯

在 APP 上打开灯, 设备端收到消息:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520093420171.png)

- 在 APP 上设置 RGB 调色;
- 设备端即解析 RGB 颜色, 并设置到具体的灯产品上，解析文件在. `linkkit_solo.c`:

```c
static int user_property_set_event_handler(const int devid, const char *request, const int request_len)
{
    int res = 0;
    cJSON *root = NULL, *LightSwitch = NULL, *LightColorRed = NULL, *LightColorGreen = NULL, *LightColorBlue = NULL;
    ESP_LOGI(TAG, "Property Set Received, Devid: %d, Request: %s", devid, request);
    if (!request)
    {
        return NULL_VALUE_ERROR;
    }
    /* Parse Root */
    root = cJSON_Parse(request);
    if (!root)
    {
        ESP_LOGI(TAG, "JSON Parse Error");
        return FAIL_RETURN;
    }
    /** Switch Lightbulb On/Off   */
    LightSwitch = cJSON_GetObjectItem(root, "LightSwitch");
    if (LightSwitch)
    {
        if (LightSwitch->valueint)
        {
            light_driver_set_rgb(0, 0, 0);
        }
        else
        {
            light_driver_set_rgb(0, 255, 0);
        }
    }
    /** Switch Lightbulb Hue */
    LightSwitch = cJSON_GetObjectItem(root, "RGBColor");
    if (LightSwitch)
    {
        LightColorRed = cJSON_GetObjectItem(LightSwitch, "Red");
        LightColorGreen = cJSON_GetObjectItem(LightSwitch, "Green");
        LightColorBlue = cJSON_GetObjectItem(LightSwitch, "Blue");
        light_driver_set_rgb(LightColorRed->valueint, LightColorGreen->valueint, LightColorBlue->valueint);
    }
    cJSON_Delete(root);
    res = IOT_Linkkit_Report(EXAMPLE_MASTER_DEVID, ITM_MSG_POST_PROPERTY, (unsigned char *)request, request_len);
    ESP_LOGI(TAG, "Post Property Message ID: %d", res);

    return SUCCESS_RETURN;
}
```

## 9.重新配网
快速重启设备 5 次, 设备会擦除配置信息, 会出现呼吸闪烁， 重新进入配网状态.


## 10.OTA 支持
可参考安信可开源团队徐工的文档：[https://blog.csdn.net/xh870189248/article/details/103779637](https://blog.csdn.net/xh870189248/article/details/103779637)

## 11.天猫精灵
### 11.1 天猫精灵控制设备
针对使用公版 APP 的产品，用户可以一键开通天猫精灵，实现天猫精灵音箱对设备的控制. 使用步骤参照[阿里云文档](https://living.aliyun.com/doc#TmallGenie.html).
- 在阿里云 [生活物联网平台](https://living.aliyun.com/#/)上一键开通天猫精灵, 查看功能映射.
- 在 `云智能` [公版 APP]((https://living.aliyun.com/doc#muti-app.html))上绑定天猫精灵账号(即淘宝账号).  

注意最后步骤, 否则天猫精灵无法找到设备:
> 在天猫精灵 APP 找到 "阿里智能" 技能, 手动进行 "尝试" 或 "设备同步"(后期会进行自动同步)  
> 即可在 "我家" 的设备列表中看到您的设备

完成以上步骤后，您可以通过天猫精灵音箱控制您的设备了. 您可以对天猫精灵说 "天猫精灵,开灯", "天猫精灵, 关灯", "天猫精灵, 把灯调成红色" 或者其他您希望设置的颜色, 设备即响应相应的命令.

### 11.2 天猫精灵配网并控制设备
阿里云设备支持 `零配` 的配网方式.  
使设备进入配网状态, 对天猫精灵说 "天猫精灵,发现设备"  
天猫精灵回复 "正在为您扫描, 发现了智能灯, 现在连接吗"  
对天猫精灵说 "连接" 或者 "是的"  
天猫精灵回复 "好的, 设备连接中, 稍等一下下哦"  
设备收到天猫精灵发送的管理帧配网信息, 进行联网:  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520093741376.png)

等待联网成功, 天猫精灵说 "智能设备联网成功, 现在用语音控制它试试", 这时您可以通过天猫精灵音箱控制您的设备了.

如果您之前通过云智能 APP 配网, 天猫精灵配网成功后, 云智能 APP 将不再显示设备. 如果继续通过云智能 APP 配网, APP 会配网失败, 显示 "设备添加失败, 设备已被管理员绑定, 请联系管理员解绑或将设备分享给您". 
> 在天猫精灵 APP 删除设备, 云智能 APP 再进行配网可以配置成功并显示设备.

## 12.国际站设备开发
### 12.1 创建国际站产品
在阿里云 [生活物联网平台-国际站](https://living-global.aliyun.com/#/) 创建产品, 参考[全球化服务文档](https://g.alicdn.com/aic/ilop-docs/2018.10.35/international.html)和[创建产品文档](https://living.aliyun.com/doc#readygo.html).  
同上述阿里云平台部署, 新增测试设备后获取设备的`三元组`, 参考 [量产说明](../../../config/mass_mfg/README.md) 文档烧录三元组到模组 NVS 分区.
### 12.2 海外版 APP
手机从[阿里云官网](https://living.aliyun.com/doc#muti-app.html) 下载 `云智能` 公版 APP, 海外用户版.  
如果已下载过公版 APP(国内用户版), 退出登录, 重新注册.
注册时选择其他非“中国大陆”的地区，都认为是海外的 APP 账号，会默认连接国际站的服务器。  
目前在中国内地之外的国家和地区（包括港澳台地区）支持3个region：新加坡、美国和德国。中国内地之外的设备激活联网时，将统一连接到新加坡激活中心。在设备绑定时，平台将根据 App 用户所在区域，自动将设备切换到相应的region（示例如下）。

 - 示例一：App用户注册时选择的国家为美国，平台会将该用户待绑定的设备切换到美国的服务器。
 - 示例二：App用户注册时选择的国家为欧洲国家，平台会将该用户待绑定的设备切换到德国的服务器。
 - 示例三：App用户注册时选择的国家为东南亚国家，设备连接新加坡服务器，此时无需切换region。
### 12.3 国际版固件设置
参考[国际站设备开发](https://help.aliyun.com/document_detail/138889.html).设备端固件需要修改以下配置来支持统一连接到新加坡激活中心。  
执行
```
 make menuconfig
```
选择 `Componnet config -> iotkit embedded -> Aliyun linkkit mqtt config` -> 取消选择` MQTT DIRECT`.  
配置生效后会修改以下两个配置.
- 关闭 MQTT 直连功能, 打开 MQTT 预认证功能.
- 设置 `linkkit_solo.c` 中的 `domain_type = IOTX_CLOUD_REGION_SINGAPORE`

### 12.4 国际站设备运行
编译下载固件后, 使设备重新进入配网状态, APP 扫描 14.1 步骤中生成的配网二维码. 如果设备是第一次配网, 连网成功后连接新加坡服务器:  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520093915773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hoODcwMTg5MjQ4,size_16,color_FFFFFF,t_70)

重启设备, 由于本示例中 APP 注册时选择的地区是美国, 设备连接美国服务器:  
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-pFGq0cpJ-1589936094356)(_static/p-us.png)\]](https://img-blog.csdnimg.cn/20200520093931871.png)  
手机 APP 端显示产品状态和控制和国内站是一样的哦！



#  xClouds 地址

# 感谢：

- PHP微信对接：https://github.com/zoujingli/WeChatDeveloper
- PHP Oauth2.0：https://github.com/bshaffer/oauth2-server-php
- PHP 框架：http://www.thinkphp.cn
- 乐鑫物联网操作系统：https://github.com/espressif/esp-idf

# 地址：

- **xClouds服务器端开源地址**：[https://github.com/xuhongv/xClouds-php](https://github.com/xuhongv/xClouds-php)
- **xClouds设备端开源地址**：[https://github.com/xuhongv/xClouds-device](https://github.com/xuhongv/xClouds-device)
- **项目遵循协议**： [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)

**1、额外说明，架构中提到的对公司或组织的观点，如有争议，请联系我；**
**2、架构中涉及到的技术点，我会一一公布出来以表感谢；**
**3、同时，也欢迎大家支持我，或一起壮大这个框架，奉献你代码项目；**

- 钉钉扫描二维码，加入钉钉群一起联系，干货多多，第一时间推送！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200512152613637.png)