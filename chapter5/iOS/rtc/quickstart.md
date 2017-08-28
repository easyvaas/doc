# 快速开始
本节提供快速集成易视云 iOS 多人连麦系统 SDK 的步骤和示例代码。具体可参考demo中相关代码。

## 开发环境配置及系统要求
* 开发工具：Xcode 8.3 + [下载地址](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12)
* 系统要求：iOS 8 +
* 为了缩减 SDK 文件大小，移除了虚拟机架构的支持，请使用真机运行

## 前置条件
* 已经注册易视云账号，以下文档中统称为APPID
* 申请开通多人连麦系统权限，获得Access Key和Secret Key，用于鉴权
* 申请多人连麦 RtcId 用于初始化连麦类

## 导入SDK
将如下所需的 .framework 导入工程中

```
- EVSDKBaseFramework.framework
- EVRTCFramework.framework
- AgoraRtcEngineKit.framework
```

## 工程配置

#### 系统库配置

导入如下系统库：

```
libresolv.tbd
libc++.tbd
CoreTelephony.framework
CoreMedia.framework
CoreMotion.framework
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

获取`麦克风`、`相机`的权限

```
<key>NSCameraUsageDescription</key>
<string>获取相机权限描述</string>
<key>NSMicrophoneUsageDescription</key>
<string>获取麦克风权限描述</string>
```

#### 项目配置

Build Setting 配置：

```
TARGETS -> Build Setting -> 搜索 bitcode 并设置为 NO
```

