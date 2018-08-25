---
layout:     post
title:      "Pixel 折腾日记 (2)"
subtitle:   "AOSP系统编译"
date:       2018-08-25 
author:     "hex2bc"
header-img: "img/post-bg-compile2.jpg"
catalog: true
tags:
    - Pixel
    - AOSP
    - 编译
---  

> 本文主要介绍Pixel的AOSP(Android Open-Source Project)源码和LineageOS源码的编译和下载。

## 一、编译环境及源码下载
AOSP即Android Open-Source Project，就是android的开源部分，全部的ROM都是基于它开发的。Pixel的官方系统也是基于这个有一些修改，所以这个编译出来和Pixel官方的系统也是不一样的，会简陋很多，跟我们看到的AVD虚拟系统是一样的，所以假如没有真机的话，也可以编AVD虚拟机。而且AOSP并不包含GMS(Google Mobile Service)部分，GMS是闭源的。  
这些在官方都有详细步骤，下面我都会给出官方的链接以及我实验过的步骤。

### 1.1 [搭建编译环境](https://source.android.com/setup/build/initializing)  
不能用windows系统编译，只能是Linux或Mac。我用的是[ubuntu-16.04.5-desktop-amd64](https://www.ubuntu.com/download/alternative-downloads)。可以在电脑装个虚拟机或者装个双系统，对电脑配置要求比较高，我在一台老i3台式上虚拟机和实体机都试了，第一次编译花了10h+(之后重编时间会缩减)。可气的是虚拟机上只分配了150G的硬盘，编了一个晚上后发现空间不足。所以建议200G以上的硬盘空间。公司有台72线程的服务器，我用40线程跑大概用了40分钟。下面是所需的环境：
#### 安装jdk：
```  
sudo apt-get update  
sudo apt-get install openjdk-8-jdk
```
####  安装依赖软件：
```
sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip
```

### 1.2 [下载源代码](https://source.android.com/setup/downloading)
由于众所周知的原因，google的链接可能无法下载，可以使用[清华大学的镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/)下载，满速下载。  

#### 下载git仓库管理器repo
```
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
或者使用清华镜像：curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo > ~/bin/repo
chmod a+x ~/bin/repo
```

#### 建立工作目录
```
mkdir WORKING_DIRECTORY
cd WORKING_DIRECTORY
```

#### 初始化仓库
```
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest
```

#### 指定的Android版本
```
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-9.0.0_r1
```
如果提示无法连接到 gerrit.googlesource.com，请参照[git-repo的帮助页面](https://mirrors.tuna.tsinghua.edu.cn/help/git-repo/)的更新一节。  
要检出“master”以外的分支，请使用 -b 指定相应分支。要查看分支列表，请参阅[源代码标记和编译版本](https://source.android.com/source/build-numbers#source-code-tags-and-builds) （注意：这个页面的中文版可能更新不及时，请在左下角改为英文显示）  
android-9.0.0_r1 对应的就是 Pixel 2 XL, Pixel 2, Pixel XL, Pixel

#### 同步源码树
以后更新也只需执行这条命令，如果报错中断了可以重新执行。
```
>repo sync
...
Fetching projects: 100% (669/669), done.  
Syncing work tree: 100% (668/668), done.  
```

## 二、系统编译
```
> source build/envsetup.sh 
> lunch aosp_sailfish-userdebug
// -j4 指用4个cpu线程编译，根据你的电脑线程了决定。
> make -j4

```

