## **快速开始**
本节提供快速集成易视云Android播放端SDK的步骤和示例代码。具体可参考demo中相关代码。

### 设备及系统要求
* 设备要求：搭载Android系统的设备
* 系统要求：Android 2.3（API level 9）及其以上

### 前置条件
* 已经注册易视云账号，以下文档中统称为APPID
* 申请开通播放权限，获得Access Key和Secret Key，用于鉴权

### 使用Gradle导入SDK

易视云SDK提供Gradle添加依赖的导入方式，此种方式导入过程较为简便，省去了手动导入库文件、资源文件、第三方库依赖等繁琐操作，推荐使用。如果您使用Android Studio或IntelliJ IDEA开发，请按照如下的方法导入SDK。
在Gradle依赖中添加：
		
	dependencies {
		compile 'com.easyvaas.sdk:evcore:1.1.7'
		compile 'com.easyvaas.sdk:evplayer:1.1.4'
	}
	
如果无法正常集成请添加如下代码：

	allprojects {
		repositories {
			jcenter()
			maven { url 'https://git.yizhibo.tv/android/mvn-repo/raw/master' }
		}
	}

### 配置项目
导入sdk成功后，依次进行如下配置：

* 如果目标工程使用了混淆，请添加以下混淆配置

        -keep class com.easyvaas.sdk.core.** {*;}
        -keep class com.easyvaas.sdk.player.** {*;}
        
* 在AndroidManifest.xml文件中添加相关权限和服务

        <!-- 声明SDK使用的相关权限 -->
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>


