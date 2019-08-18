---
layout:     post
title:      "把Android Studio项目迁移到Android源码中编译"
subtitle:   "详解Android.mk"
date:       2019-08-18 
author:     "hex2bc"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
catalog: true
tags:
    - Android系统编译
    - Gradle
    - Android.mk
---  

>Android Studio引入Gradle作为构建App的系统，但在Android系统源码编译中，则用的Makefile(Android.mk)来构建App。两种不同的编译方式，差异还是挺大的。本文通过把github [okdownload](https://github.com/lingochamp/okdownload)中的Sample项目迁移到源码编译中，介绍其中的技巧，和可能遇到的坑。


## 一、Gradle的构建方式
### 1.1 待续

