# 快速开始
本节提供快速集成易视云Android直播端SDK的步骤和示例代码。具体可参考demo中相关代码。

## 设备及系统要求
* 设备要求:搭载Android系统的设备（为了达到最佳效果，推荐RAM 2GB，CPU 1.2GHz*8以上配置）
* 系统要求:Android 4.0（API level 15）及其以上，如果要使用美颜功能，要求Android 4.4（API level 19）及其以上

## 前置条件
* 已经注册易视云账号，以下文档中统称为APPID
* 申请开通直播权限，获得Access Key和Secret Key，用于鉴权

## 使用Gradle导入SDK
易视云SDK提供Gradle添加依赖的导入方式，此种方式导入过程较为简便，省去手动导入库文件、资源文件、第三方库依赖等繁琐操作，推荐使用。如果您使用Android Studio或 IntelliJ IDEA开发，请按照如下的方法导入SDK。 在Gradle依赖中添加:

```
dependencies {
		compile 'com.easyvaas.sdk:evcore:1.1.6'  //集成时填写当前发布的最新版本号
		compile 'com.easyvaas.sdk:evlive:1.1.3' //集成时填写当前发布的最新版本号
	}
```

添加易视云maven库地址:

```
allprojects {
		repositories {
			jcenter()
			maven { url 'https://git.yizhibo.tv/android/mvn-repo/raw/master' }
		}
	}
```

## 配置项目
导入sdk成功后，依次进行如下配置：

* 如果目标工程使用了混淆，请添加以下混淆配置

        -keep class com.easyvaas.sdk.core.** {*;}
        -keep class com.easyvaas.sdk.live.** {*;}
        
* 在AndroidManifest.xml文件中添加相关权限和服务

        <!-- 声明SDK使用的相关权限 -->
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
        <uses-permission android:name="android.permission.CAMERA"/>
        <uses-permission android:name="android.permission.RECORD_AUDIO"/>
        <uses-permission android:name="android.permission.FLASHLIGHT" />
        <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
        
        <!-- 硬件特性 -->
        <uses-feature android:name="android.hardware.camera" />
        <uses-feature android:name="android.hardware.camera.autofocus" />
        
        <service android:name="com.easyvaas.sdk.core.service.ResourceMonitorService" />



