---
title: rn打包配置
date: 2018-09-20 00:16:13
tags: ['react-native']
---

### 关于文章中使用到特殊变量的介绍

``workspaceroot``: 工程根目录

## android部分

### 关于keystore

``keystore``是对于``release build app``来说必须的东西。

针对未配置的工程，需要首先生成``keystore``。命令如下：

`` bash
keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
``

PS: 最后一个参数为过期时间

对于已配置的工程，当然是不需要重新生成``keystore``，只需要使用已有的就好。

将得到的``keystore``放到``workspaceroot/android/app``目录下面。

下面是``配置过程``(已配置过的项目可以跳过``打包配置``这一段)

### 打包配置

#### signConfigs签名配置

打开 ``android studio`` 定位到 ``android`` 目录：

![](/blog/images/rn-build-step-0.png)

图中圈出的部分即为密匙配置：

```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=****
MYAPP_RELEASE_KEY_PASSWORD=*****
```

上面配置的东西是类似于环境变量，下面就是在具体的签名配置里面使用这些变量：

![](/blog/images/rn-build-step-1.png)
片段1：
``` gradle
signingConfigs {
    release {
        storeFile file(MYAPP_RELEASE_STORE_FILE)
        storePassword MYAPP_RELEASE_STORE_PASSWORD
        keyAlias MYAPP_RELEASE_KEY_ALIAS
        keyPassword MYAPP_RELEASE_KEY_PASSWORD
    }
}
```

片段2：
```
signingConfig signingConfigs.release
```

#### appIcon以及appName配置

安卓配置比较简单，定位到``workspaceroot/android/app/src/main/AndroidManifest.xml``。
![](/blog/images/rn-build-step-2.png)

``android:label``: app显示的名称
``android:icon``: app图标配置

指的注意的是``@string``为全局性的一个属性，它的值被定义在``./res/values/strings.xml``中，而``@mipmap``则是res中``mimap-*``相应目录的映射，会根据``android``设备的分辨率指向特定的文件夹。如图：

![](/blog/images/rn-build-step-3.png)

#### 小结

安卓``release app``的配置理论上只需要``keystore``配置ok就可以了，总体来说是比较简单的。并且在已经配置过``signConfig``之后，其他人只需要拿到``keystore``文件就可以打包了。

### 执行打包

因为比较简单，就直接把命令列出来：

首先确认当前在``workspaceroot``目录。

``` bash
cd android //进入安卓工程
./gradlew assembleRelease //执行release app打包
./gradlew installRelease //执行release app打包，并且安卓在adb连接的设备上
./gradlew assembleDebug //执行 debug app 打包
```

打包出来的apk文件会放在``workspaceroot/android/app/build/outputs/apk``目录。

## IOS部分

> apple安全性确实比较高

### 开发者账号配置

首先在 ``xcode`` 登陆开发者账号。由菜单栏进入：``Xcode => Preferences``

![](/blog/images/rn-build-step-4.png)
箭头指向的 ``➕`` 可以添加新的账号。

登陆之后有一个二次验证过程，需要输入验证码。

#### 配置 Development 证书
成功登陆之后，打开 [``apple developer account``](https://developer.apple.com/account) 找到以下界面：（PS：找不到的话我也没办法）
![](/blog/images/rn-build-step-5.png)

在上图中通过设备名称找到自己的设备，点击之后可以下载到本地。

双击下载后的文件应该会弹出来安装到``keyschain``的窗口直接安装就好了。

以上就是安装 ``Development`` 证书的过程

#### 配置 Production 证书

公司目前的开发模式是，开发人员使用独立的 ``Development`` 证书，所有人公用一个 ``Production`` 证书。这个证书可以导出分享。

![](/blog/images/rn-build-step-6.png)

这个证书一般是 ``***.p12`` 文件。所以也被称作 ``p12证书``。拿到这个证书之后一样双击，会有安装向导。

#### Xcode配置以及打包

使用 Xcode 打开 ``IOS`` 工程， 路径为 ``workspaceroot/ios/<app_name>.xcodeproj``。
![](/blog/images/rn-build-step-8.png)

按照框图中 ``Signing`` 部分，勾选为自动签名.然后选择一个组.

PS：以上不知道是不是配置过之后就会记录到git中，还是每台机器都需要配置，有待验证。

下面是打包过程：

+ 设备设置为 ``generic ios device`` （上图设备选择那里点选）
+ 菜单栏：Product > archive 等待完成 > export 
+ 之后选择部署企业应用（列表中第三项），然后一路next，选择本地保存的路径。
+ 最终会得到 ``**.ipa`` 文件

## app托管以及相关资源

目前公司的app均托管在 [``蒲公英``](https://www.pgyer.com/)。

打开蒲公英网站之后，就可以发布应用了。这部分略。

蒲公英账号，android keystore，ios p12证书。联系邮箱： xihao_ruan@fpi-inc.com


