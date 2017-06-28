# 快速开始
本节提供快速集成易视云 iOS 观看端 SDK 的步骤和示例代码。具体可参考 demo 中相关代码。 

## 开发环境配置及系统要求
* 开发工具：Xcode 8.3 + [下载地址](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12)
* 系统要求：iOS 7 +
* 为了缩减 SDK 文件大小，移除了虚拟机架构的支持，请使用真机运行

## 前置条件
* 已经注册易视云账号，以下文档中统称为APPID
* 申请开通消息系统权限，获得Access Key和Secret Key，用于鉴权

## 导入SDK
将`EVSDKBase`、`EVPlayer`文件夹中的导入到工程中（也可以直接将如下的**.a**、**.framework**、**.h**直接导入工程中）

```
- EVSDKBase（文件夹）
    - EVSDKManager.h
    - libEVMedia.a
    - libEVSDKBase.a
- EVPlayer（文件夹）
    - EVPlayer.h
    - EVPlayerConfig.h
```

## 工程配置

#### 系统库配置

导入如下系统库：

```
libsqlite3.tbd
libstdc++.6.tbd
libbz2.tbd
libz.tbd
SystemConfiguration.framework
VideoToolbox.framework
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

