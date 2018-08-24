---
layout:     post
title:      "pixel 折腾日记(1)"
subtitle:   "刷官方及第三方固件"
date:       2018-08-23 
author:     "hex2bc"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
catalog: true
tags:
    - pixel
    - 刷机
    - LineageOS
---  

>闲来无事从闲鱼上买了个一代pixel来玩，体验下刷机的快感。本文主要介绍如何下载pixel官方固件和第三方的LineageOS固件，以及有锁版pixel的解锁。  

## 一、OEM解锁
### 1.1 无锁机器  
可以直接在开发者选项里面打开『OME解锁』。  

### 1.2 有锁机器
如果你跟我一样倒霉，买之前没有了解，买到有锁版的机器，该选项灰色不可点。网上有几种解锁的教程，大多只能针对特别版本有效。但最近大神放出的[解锁方法](https://forum.xda-developers.com/showpost.php?p=76633532&postcount=138)我在最新9.0 pie版本测试是有效的。  
下面是我根据大神方法试验的解锁步骤：

> + 退出谷歌账号并去除锁屏  
> + 拔出sim卡  
> + 恢复出厂设置，在开机向导时不要插卡，不要连接wifi和设置锁屏  
> + 把手机接到电脑，并可以使用adb  
> + 执行： `adb shell pm uninstall --user 0 com.android.phone`  
> + 重启  
> + 连接可以科学上网的wifi，打开chrome随便上个网站，如 `google.com`  
> + 此时进开发者选项会发现『OME解锁』可以点了  
> + 连接电脑执行：`adb reboot bootloader` 或者关机后按住音量调低键，然后按住电源键，进入bootloader模式
> + 然后执行 `fastboot oem unlock` 和 `fastboot flashing unlock` 即可。  

这个方法估计是卸载掉电话模块导致无法检测IMEI，从而无法判断该机器是否有锁。具体原因还得研究后才知道。  
如果以上方法试了几遍都无效的话，只能上XDA找找其它方法能不能用，或者求助万能的淘宝。淘宝好像可以通过降级的方法解锁，不过得花约150大洋。


## 二、刷官方最新固件
### 2.1 下载地址
既然是亲儿子，肯定能用上最新的系统，官方固件从[https://developers.google.com/android/images#sailfish](https://developers.google.com/android/images#sailfish "sailfish for Pixel") （自备梯子）上下载。  
一代pixel的代号是sailfish。Pixel XL的代号是marlin。

### 2.2 官方刷机教程
[https://developers.google.com/android/images#instructions](https://developers.google.com/android/images#instructions)

### 2.3 刷官方固件流程
> + 连接电脑执行：`adb reboot bootloader` 或者关机后按住音量调低键，然后按住电源键，进入bootloader模式
> + 进入bootloader模式后，输入`fastboot devices`有输出内容即可开始刷机。  
> \> fastboot devices  
xxxxxxxx    fastboot  
> + 把安装包sailfish-ppr1.xxx.zip解压后，用命令行进入该目录
> + 执行 `flash-all.bat` (win) 或 `flash-all.sh`(mac/linux)
> + 如果只刷固件，执行`fastboot -w update image-sailfish-xxx.zip` 即可
> + 完成会自动重启到桌面

## 三、刷LineageOS固件

LineageOS前身是著名的CM，根据AOSP源码有少量改动，是当前比较好用的第三方系统。现在最新的lineage-15.1对应的android 8.1。

### 3.1 下载地址
[https://download.lineageos.org/sailfish](https://download.lineageos.org/sailfish)

### 3.2 官方刷机教程
[https://wiki.lineageos.org/devices/sailfish/install](https://wiki.lineageos.org/devices/sailfish/install)

### 3.3 刷LineageOS固件流程
> + 前面的步骤和刷官方的一样, 也要先进入bootloader模式  
> + 此时要安装个第三方的recovery, 从[https://dl.twrp.me/sailfish/](https://dl.twrp.me/sailfish/) 上下载最新的twrp-x.x.x-x-sailfish.img即可。  
> + 用 `fastboot boot twrp-x.x.x-x-sailfish.img` 安装, 安装后会进入twrp的bootloader模式。  
> + 点击wipe --> Advancee Wipe --> 选择前三项 --> 然后Swipe to Wipe
> + 用adb把固件push到手机: `adb push filename.zip /sdcard/`  
> + 回到主菜单, 点击 Install 选择你刚才push到手机的固件安装。安装完会重启。  
> + 如果顺利的话就直接进入Lineage桌面了，无限重启的话以上步骤再试一次，最好在原系统版本在8.1及以下的时候刷，我之前使用9.0刷会无限卡动画。原因不明。  

以上的步骤说的比较简洁，具体的步骤可以看官方的教程，认真按一步步执行就可以了。放心，玩不坏的。买回来这两天我都折腾了几十次了。  
下一节主要说说这两个系统的编译。


