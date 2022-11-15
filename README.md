## ESP32

ESP32 simulate JoyCon,Pro Controller,Amiibo for Nintendo Switch

通过ESP32来模拟 Joy-Con和Pro Controller，实现伊机控自动脚本，使用EasyConAPI，兼容所有伊机控衍生版本。

![image-20220914002841981](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209140028015.png)

**特性：**

- 发包频率，最高50Hz
- 支持更换手柄颜色
- 支持更换手柄
- 支持Amiibo，20个永久存储，可更换
- 一次配对，永久重连
- 支持断连自动重连



视频看这里

> https://b23.tv/2RdzJYN



## 开发板

ESP32建议购买ESP32-WROOM-32、ESP32-WROOM-32E、ESP32-WROOM-32D，任何使用或者标明使用CH340的请不要买，大概率会重启，推荐带有**cp2102关键字**的

- 注意不是ESP32-Cx ESP32-Sx ，蓝牙并不一样



#### 推荐

![image-20220918205306450](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209182053515.png)

- 按键是按照引脚15对齐的，正面和屏幕一致

不差钱可以买这样的，带屏幕+按键，平常用起来也方便

![image-20220918205817738](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209182058913.png)

如果是4*4键盘，定义如图，其他按键时重复功能（IO不够用）

![image-20220918205929241](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209182059355.png)



省钱请搜以下关键词，找便宜的

**淘宝关键词**

- ESP32 DevKitC
- ESP32 Core Board
- ESP32-PICO-KIT
- ESP-WROVER-KIT
- Wemos Oled （自带屏幕）
- TTGO T8（可以插SD卡）
- D1 LOLIN32



#### 有点小问题的

有点小问题：(**第一次连接伊机控的时候会自动重启，不影响实际使用**)
淘宝名称为：USB Type-C ESP32开发板 CH340C WiFi+蓝牙超低功耗双核ESP32（就是最便宜的款）
淘宝名称为：ESP-32 CP2102/CH9102/CH340驱动开发板WIFI+蓝牙CPU模块系统板



#### 避坑

**千万别买**，发的模块不一致，存在断联、卡连接、重启等问题，每个人买到的都可能不一样

![image-20220914002257620](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209140022682.png)



淘宝名称为：**ESP32 WIFI蓝牙开发板CP2102版USB转串口扩展MicroUSB接口** (挂羊头卖狗肉，图上有屏幕，实际发货没有）



## 驱动

第一次可能没有驱动，串口找不到，请先安装驱动，根据你板子自己找驱动或者问店家，cp210x的在群文件，其他自己解决



## 烧写

**使用数据线连接ESP32到电脑，注意必须是数据线，只能充电的线不可以**

打开烧写工具（群内有）

选择ESP32

![image-20220911091739733](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209110917798.png)

如图烧写

![image-20221004034600466](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202210040346519.png)

有屏幕和无屏幕都是同一个固件，自动识别的



## 配对

![image-20220914130720512](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209141307707.png)

当蓝灯闪烁时，表示此时没连接到NS，连接到NS以后会自动灭灯。

当红的长亮，表示当前板子供电正常



进入`更改握法/顺序`进行配对，会自动显示手柄并且自动A，不需要任何人为操作

![image-20220911093355824](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209110933907.png)



## 重启

![image-20220914130821172](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209141308272.png)



## 取消自动连接配对

由于默认是自动连接配对的，要更换NS的时候，建议先连上伊机控，然后取消配对，成功以后，断电重启ESP32，之后再和新NS重新配对即可

![image-20220911091919946](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209110919971.png)





## 更改手柄属性

首先正常连接伊机控

手柄模式选择以后，设置，断电重启ESP32，就能看到对应的手柄了

![image-20221004034633382](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202210040346427.png)

手柄颜色请先点击对应的位置，选好颜色以后，再设置，断电重启ESP32，就能看到对应的手柄了

![image-20220911092201264](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209110922295.png)

颜色可以自定义，随便什么数值都可以



## Amiibo

Amiibo 内置了一个Mipha的，最多可以存储20个，通过手柄设置进行设置

![image-20221004034703372](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202210040347422.png)

默认Amiibo是自动生成的，不需要你自己下载。**如果自动生成的Amiibo没有你要的**，你可以找到对应的bin，放在伊机控下的Amiibo文件夹中，然后**游戏选择自定义**，然后选择你的bin



脚本语法

```
【Amiibo切换】
语法： AMIIBO 序号(序号范围0-19)

示例：
# 循环切换amiibo
FOR $3 = 0 TO 19
    AMIIBO $3 # 从0-19号切换Amiibo
NEXT
# 切换固定amiibo序号
AMIIBO 3 # 激活3号amiibo
```

- 注意触发Amiibo读取需要先A一下，否则不会自动读，Amiibo读取时请不要疯狂按键，可能会导致报错重连



## 断开手柄连接

某些情况下需要取消ESP32和NS的配对（建议先让板子断开自动匹配）

![image-20220911092606511](http://img.elmagnifico.tech:9514/static/upload/elmagnifico/202209110926582.png)

进入以后长按X，就可以断开所有手柄



## 常见问题

重启5次都无法连上，先取消`自动连接配对`，断电ESP32，再`断开NS手柄连接`，重启NS，之后重新配对即可



**某些情况下怎么都无法配对或者自动重连，请重启NS**

