# 快速开始
本节提供快速集成易视云 iOS 直播端 SDK 的步骤和示例代码。具体可参考 demo 中相关代码。 

## 开发环境配置及系统要求
* 开发工具：Xcode 8.3 + [下载地址](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12)
* 系统要求：iOS 7 +
* 为了缩减 SDK 文件大小，移除了虚拟机架构的支持，请使用真机运行

## 前置条件
* 已经注册易视云账号，以下文档中统称为APPID
* 申请开通消息系统权限，获得Access Key和Secret Key，用于鉴权

## 导入SDK
将`EVSDKBase`、`EVStreamer`文件夹中的导入到工程中（也可以直接将如下的**.a**、**.framework**、**.h**直接导入工程中）

```
- EVSDKBase（文件夹）
    - EVSDKManager.h
    - libEVMedia.a
    - libEVSDKBase.a
- EVStreamer（文件夹）
    - AgoraRtcEngineKit.framework
    - EVStreamer.h
    - EVStreamerConfig.h
    - KSYGPUResource.bundle
    - videoprp.framework
```

## 工程配置

#### 系统库配置

导入如下系统库：

```
libresolv.tbd
libc++.tbd
libz.tbd
CoreTelephony.framework
CoreMedia.framework
CoreMotion.framework
CoreVideo.framework
OpenGLES.framework
QuartzCore.framework
Security.framework
VideoToolbox.framework
AudioToolbox.framework
AVFoundation.framework
```

#### 网络配置
由于SDK内部存在鉴权的网络请求，所以需要打开配置ATS，在工程的`info.plist`中添加如下内容：

```objective-c
<key>NSAppTransportSecurity</key>
<dict>
   <key>NSAllowsArbitraryLoads</key>
   <true/>
</dict>
```

#### 系统权限

获取`麦克风`、`相机`、`相册`的权限

```
<key>NSCameraUsageDescription</key>
<string>获取相机权限描述</string>
<key>NSMicrophoneUsageDescription</key>
<string>获取麦克风权限描述</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>获取相册权限描述</string>
```

#### 项目配置

Build Setting 配置：

```
TARGETS -> Build Setting -> 搜索 bitcode 并设置为 NO
```

```
TARGETS -> Build Setting -> 搜索 Other Linker Flags 并添加 -ObjC
```




