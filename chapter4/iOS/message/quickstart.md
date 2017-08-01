# 快速开始
本节提供快速集成易视云iOS消息系统SDK的步骤和示例代码。具体可参考demo中相关代码。

## 开发环境配置及系统要求
* 开发工具：Xcode 8.3 + [下载地址](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12)
* 系统要求：iOS 8 +
* 为了缩减 SDK 文件大小，移除了虚拟机架构的支持，请使用真机运行

## 前置条件
* 已经注册易视云账号，以下文档中统称为APPID
* 申请开通消息系统权限，获得Access Key和Secret Key，用于鉴权

## 导入SDK
导入对应的 framework:

```
- EVSDKBaseFramework.framework
- EVMessageFramework.framework
- EVSocket.framework
```

注：其中 `EVSocket.framework` 为动态库，需要添加到项目工程 TARGETS->General->Embedded Binaries 中，否则运行应用时控制台会输出 **image not found** 的错误


## 工程配置
#### 网络配置
由于SDK内部存在鉴权的网络请求，所以需要打开配置ATS，在工程的`info.plist`中添加如下内容：

```objective-c
<key>NSAppTransportSecurity</key>
<dict>
   <key>NSAllowsArbitraryLoads</key>
   <true/>
</dict>
```

#### 动态库配置
* project->targets->General->Embedded Binaries 点击加号(+)，添加`EVSocket.framework`
* project->targets->Build Settings 搜索：Always Embed Swift Standart Libraries，设置其为`YES`

## 示例代码
#### 初始化SDK流程
在初始视图控制器中添加引用

```objective-c
#import "EVSDKManager.h"
```

在当前视图控制器进行SDK初始化操作，需要传入参数`appid、appkey、appsecret、userid`

```objective-c
[EVSDKManager initSDKWithAppID:appid appKey:appkey appSecret:appsecret userID:userid];
```

添加初始化SDK后的回调通知

```objective-c
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(initSDKError:) name:EVSDKInitErrorNotification object:nil];
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(initSDKSuccess) name:EVSDKInitSuccessNotification object:nil];
```

初始化成功后，即可进行其他操作。

#### 使用消息系统
* 设置当前消息系统视图控制器为代理

```objective-c
[EVMessageManager shareManager].delegate = self;
```

* 开始回话前，首先建立连接，加入某一频道，connect成功后会回调`- (void)EVMessageConnected`

```objective-c
[[EVMessageManager shareManager] connect:channel];
```

* 在已加入的频道中发送消息，可以根据需求透传附加消息userData，userData的格式是一个字典

```objective-c
[[EVMessageManager shareManager] sendWithChannel:channel message:message userData:customUserData type:EVMessageTypeMsg result:^(NSDictionary *response, NSError *error) {
    // 发送消息的block回调，error为nil则成功
}];
```

* 发送点赞操作，点赞数为10

```objective-c
[[EVMessageManager shareManager] addLikeCountWithChannel:channel count:10 result:^(NSDictionary *response, NSError *error) {
    // 发送点赞的block回调，error为nil则成功
}];
```

* 获取最近20条的历史消息

```objective-c
[[EVMessageManager shareManager] getLastHistoryMessageWithChannel:channel count:20 type:EVMessageTypeMsg result:^(NSDictionary *response, NSError *error) {
    // 获取历史消息的block回调，error为nil则成功，并可获取response中的历史消息内容
}];
```

* 离开当前频道

```objective-c
[[EVMessageManager shareManager] leaveWithChannel:channel result:^(NSDictionary *response, NSError *error) {
    // 发送离开频道的block回调，error为nil则成功
}];
```

* 关闭连接

```objective-c
[[EVMessageManager shareManager] closeConnect];
```

* 消息系统操作回调方法

```objective-c
#pragma mark - EVMessageProtocol
- (void)EVMessageConnected {
    // 连接成功
}

- (void)EVMessageConnectError:(NSError *)error {
    // 连接错误
}

- (void)EVMessageDidCloseWithCode:(EVMessageErrorCode)code reason:(NSString *)reason {
    // 连接关闭
}

- (void)EVMessageRecievedNewMessageInChannel:(NSString *)channel sendedFrom:(NSString *)userid message:(NSString *)message userData:(NSDictionary *)userData {
    // 收到了新的消息
}

- (void)EVMessageUsers:(NSArray<NSString *> *)userids joinedChannel:(NSString *)channel {
    // 有用户加入了当前频道
}

- (void)EVMessageUsers:(NSArray<NSString *> *)userids leftChannel:(NSString *)channel {
    // 有用户从当前频道离开
}

- (void)EVMessageDidUpdateLikeCount:(long long)likeCount inChannel:(NSString *)channel {
    // 更新like数
}

- (void)EVMessageDidUpdateWatchingCount:(NSInteger)watchingCount inChannel:(NSString *)channel {
    // 更新正在观看的人数
}

- (void)EVMessageDidUpdateWatchedCount:(NSInteger)watchedCount inChannel:(NSString *)channel {
    // 更新已经观看的人数
}

```

