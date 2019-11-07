title: GoKit3二次开发-代码自动生成工具介绍(旧)
---

**2017年10月30日起已使用新版MCU代码生成，[查看新版](http://docs.gizwits.com/zh-cn/deviceDev/DevSDK/代码自动生成工具.html)代码自动生成工具介绍**

# 前文需知
## 1.什么是“代码自动生成工具”
为了降低开发者的开发门槛，缩短开发周期，降低开发资源投入，机智云推出了代码自动生成服务。云端会根据产品定义的数据点生成对应产品的设备端代码。

自动生成的代码实现了机智云通信协议的解析与封包、传感器数据与通信数据的转换逻辑，并封装成了简单的API，且提供了多种平台的实例代码。当设备收到云端或APP端的数据后，程序会将数据转换成对应的事件并通知到应用层，**开发者只需要在对应的事件处理逻辑中添加传感器的控制函数，就可以完成产品的开发。**

使用自动生成的代码开发产品，就不必再处理协议相关的部分了，开发者可以将节省出来的精力集中在产品的核心功能开发上。

## 2.支持的平台
自动生成服务支持的硬件方案有：独立MCU方案、SOC方案。其中独立MCU方案支持的硬件平台有：stm32f103c8x平台、通用平台（即“其他平台”）；SOC方案支持的硬件平台有：ESP8266平台。

MCU方案与SOC方案区别：

|方案	  |说明 |
|:---    |:---|
|MCU方案 |模组负责与云端信息的交互，通过串口与主控板（即MCU）进行通信，需要在MCU上进行协议解析与外设控制的开发。|
|SoC方案 |节省一颗MCU芯片，利用模组内部资源完成传感器操作和产品逻辑。|

MCU方案中除了支持STM32平台，还可以将我们生成好的通用平台版代码移植到符合条件的任意平台，从而实现机智云所提供的各种功能（详细移植过程请查看《GoKit3二次开发--通用平台版移植说明》）。
# “代码自动生成工具”的使用
## 1.创建产品
登录机智云开发者中心：[http://dev.gizwits.com/](http://dev.gizwits.com/)

![登录机智云开发者中心](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478158998646.png)

点击右上角创建新产品

输入相应的产品信息后点击“保存”。

![创建产品](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159012724.png)

## 2.添加数据点

添加相应的数据点

![添加相应的数据点](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159509259.png)

![添加相应的数据点](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159514757.png)

添加成功后点击“应用”

![添加相应的数据点](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159536876.png)

## 3.生成目标平台代码

>注：如果之前没有定义数据点则无法使用自动生成代码服务。

### 3.1 生成MCU方案代码

定义好产品后，选择左侧服务中的“MCU开发”(假设采用的MCU是STM32F103C8x)，选中硬件方案中的“独立MCU方案”，再选择“硬件平台”中的“stm32f103c8x”，最后点击“生成代码包”，等待生成完毕下载即可。

>注：如果是其他MCU芯片，请选择“其他平台”选项，然后将生成的代码包移植到使用的平台，移植方法参考《GoKit3二次开发-通用平台版移植说明》。

![生成MCU方案代码](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159645300.png)

![生成MCU方案代码](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159651968.png)

### 3.2 生成SoC方案代码

定义好产品后，选择左侧服务中的“SoC开发”(假设使用的SoC芯片是esp8266)，选中硬件方案中的“SoC方案”，则选择“硬件平台”中的“esp8266”，最后点击“生成代码包”，等待生成完毕下载即可。

![生成SoC方案代码](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159682322.png)

下载完成后解压如下

![生成SoC方案代码](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159695787.png)

# 自动生成代码说明

##  1. STM32平台文件说明

![STM32平台文件说明](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478159738287.png)

主要文件说明：

|文件	  |说明 |
|:---    |:---|
|gizwits_product.c	|该文件为产品相关处理函数，如 gizEventProcess()平台相关硬件初始化，如串口、定时器等|
|gizwits_product.h |	该文件为 gizwits_product.c 的头文件，存放产品相关宏定义如： HARDWARE_VERSION 、SOFTWARE_VERSION|
|gizwits_protocol.c	|该文件为 SDK API 接口函数定义文件|
|gizwits_protocol.h	|该文件为 gizwits_protocol.c 对应头文件,相关 API 的接口声明均在此文件中|

## 2. ESP8266平台文件说明

![ESP8266平台文件说明](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478160012758.png)

主要文件说明：

|文件	  |说明 |
|:---    |:---|
 |libgagent.a |	该文件为机智云设备接入协议库文件,文件位于 lib 目录下 |
 |gagent_external.h	 |该文件为 libgagent.a 对应头文件,两个文件配合使用 |
 |gizwits_product.c	 |该文件为平台相关处理文件，存放事件处理API接口函数，即 gizwitsEventProcess() |
 |gizwits_product.h 	 |该文件为  gizwits_product.c 的头文件，存放产品相关宏定义如： HARDWARE_VERSION 、SOFTWARE_VERSION |
 |gizwits_protocol.c	 |该文件为协议实现文件，存放 SDK API 接口函数 |
 |gizwits_protocol.h	 |该文件为 gizwits_protocol.c 对应头文件，协议相关宏定义即 API 接口声明均在此文件中。 |


# 二次开发介绍

## 1. 代码二次开发需知

自动生成的代码已经根据用户定义的产品数据点信息，并针对STM32、ESP8266等平台，生成了对应的机智云串口协议层代码，用户只需要调用相应的API接口或添加相应的逻辑处理即可。代码框架如下图所示：

![代码二次开发需知](/assets/zh-cn/deviceDev/DevSDK/tool/tool_old/1478160174766.png)


>需要开发的部分为：

> - 下行处理：例如LED灯开关、电机转速控制等。

> - 上行处理：例如温湿度数据采集，红外传感器状态获取等。

> - 配置处理：配置入网及恢复出厂设置。

## 2. 二次开发举例

自动生成的代码在各平台之间使用了统一的协议封装，故二次开发所完成的修改也几乎相同，下面以STM32平台为例。

### 2.1 下行处理

首先要完成的是传感器驱动开发，然后在Gizwits目录下的gizwits_product.c文件中的gizwitsEventProcess()函数中处理相应事件即可（如下例中的ledRgbControl(),功能是控制RGB灯的颜色）。

下面以控制RGB LED为例，代码示例如下：

**修改前**：

```c
if(0x01 == currentDataPoint.valueLED_ONOFF)
{
  //user handle
}
else
{
  //user handle    
}        
break;
```

**修改后**：

```c
if(0x01 == currentDataPoint.valueLED_ONOFF)
{
  //user handle
ledRgbControl(254,0,0);
}
else
{
  //user handle    
ledRgbControl(0,0,0);  
}        
break;
```

### 2.2 上行处理
首先要完成的是传感器驱动开发，然后在user目录下main.c文件中的userHandle()函数中实现传感器数据采集，用户只需并将采集到的数值赋值给对应用户区的设备状态结构体数据位即可（如下例中的：currentDataPoint.valueInfrared = irHandle();）。

下面以红外传感器的数据获取为例（只读型数据点的操作会被云端自动生成），如下：

**修改前**：

```c
void userHandle(void)
{
    /*
    currentDataPoint.valueInfrared = ;//Add Sensor Data Collection
    */
}
```

**修改后**：

```c
vvoid userHandle(void)
{
    currentDataPoint.valueInfrared = irHandle();
}
```

>特别提醒：userHandle()被while循环调用，执行速度较快，需要针对不同的需求，用户可调整数据点数据的采集周期和接口实现位置，预防由于传感器数据采集过快引发的不必要的问题。但不建议在userHandle()中调用延时函数来降低执行频率。正确方法如下：

```c
void userHandle(void)
{
static uint32_t irLastTimer = 0;

if((gizGetTimerCount()-irLastTimer ) > SAMPLING_TIME_MAX)     	{
currentDataPoint.valueInfrared = irHandle();

irLastTimer = gizGetTimerCount();
    }
}
```

###  2.3 配置处理

除了数据的上行与下行处理外，还需要一些配置操作需要完成，如下：

- 配置入网
- 恢复出厂配置

我们提供了一个API接口，实现上述操作，定义如下。

```
int32_t gizwitsSetMode(uint8_t mode)
```

参数mode支持WIFI_RESET_MODE、WIFI_SOFTAP_MODE、WIFI_AIRLINK_MODE三种(详情见gizwits_protocol.h文件的 WIFI_MODE_TYPE_T)，分别完成恢复出厂配置，进入softap配置模式，进入airlink配置模式操作。您可以根据产品的定义实现不同的配置操作，如按键触发进入配置模式。
