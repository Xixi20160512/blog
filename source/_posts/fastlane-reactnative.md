---
title: fastlane初步
date: 2018-11-28 11:21:33
tags: ['react-native', 'fastlane']
---

### 介绍

fastlane 是一个打包 app 的工具集合

目前仅支持 masOS 环境使用

### 安装使用

`brew cask reinstall fastlane`

注意：

![2018-11-28-11-26-39](http://qiniu.xixi2016.cc/2018-11-28-11-26-39.png)
安装完可能会有以上报错，查阅知道是没有配置环境变量
```
# fastlane
export PATH="$HOME/.fastlane/bin:$PATH"
```
加上就好了

security set-key-partition-list -S apple-tool:,apple:,codesign: -s -k Fpi123456 login.keychain