## 功能详细说明
### 发送消息
* 消息类型

```objective-c
typedef NS_ENUM(NSUInteger, EVMessageType) {
    EVMessageTypeMsg = 0,   // default message type
    EVMessageTypeSystem,
    EVMessageTypeGift,
    EVMessageTypeRedPack
};
```

基础消息类型分四种：消息、系统消息、礼物、红包，默认消息类型为`EVMessageTypeMsg`。

* 消息等级

```objective-c
typedef NS_ENUM(NSUInteger, EVMessageLevel) {
    EVMessageLevel_0 = 0,
    EVMessageLevel_1,
    EVMessageLevel_2,
    EVMessageLevel_3,
    EVMessageLevel_4,   // default level
    EVMessageLevel_5,
    EVMessageLevel_6,
    EVMessageLevel_7,
    EVMessageLevel_8,
    EVMessageLevel_9,
    EVMessageLevel_10
};
```

消息等级代表所发送消息的权重，区间为[0, 10]，默认为`EVMessageLevel_4`。

*注意：EVMessageLevel_9、EVMessageLevel_10的消息不能被过滤*

#### 普通接口

```objective-c
/**
 发送消息

 @param topic 所在topic
 @param message 发送的消息字符串
 @param userData 透传消息
 @param type 消息类型
 @param callBack 回调
 */
- (void)sendWithTopic:(NSString *)topic message:(NSString *)message userData:(NSDictionary *)userData type:(EVMessageType)type result:(EVMessageCallBack)callBack;
```
消息系统发送消息的简易方法，主要传入消息内容、透传信息、消息类型即可，默认消息等级为4，并保留消息。

#### 高级接口

```objective-c
/**
 发送消息(全参数)

 @param topic 所在topic
 @param message 发送的消息字符串
 @param userData 透传消息
 @param type 消息类型
 @param level 消息等级
 @param save 是否保留为历史消息
 @param callBack 回调
 */
- (void)sendWithTopic:(NSString *)topic message:(NSString *)message userData:(NSDictionary *)userData type:(EVMessageType)type level:(EVMessageLevel)level save:(BOOL)save result:(EVMessageCallBack)callBack;
```
消息系统发送消息的完整方法，除了上述简易方法中的参数外，还可对消息等级（level）、是否保留为历史消息（save）进行设置。

*注意：透传消息userData为NSDictionary格式，传递结构化的数据，由上层业务系统定义该协议。例如发送评论时，协议内容可能为发送该消息的用户昵称、@等信息；发送红包时，协议内容包含红包相关信息。*

### 错误码
消息系统SDK错误状态码如下表：

| 名称 | 数组 | 含义 |
|:--|:--|:--|
| EVMessageErrorNone | 0 | 无错误 |
| EVMessageErrorNetworkTimeout | -1001 | 连接超时 |
| EVMessageErrorNetworkInvalidURL | -1002 | 消息服务器URL错误 |
| EVMessageErrorNetworkNotConnect | -1003 | 未连接 |
| EVMessageErrorNetworkUnkown | -1004 | 未知网络错误 |
| EVMessageErrorSDKNotInit | -2001 | sdk未初始化或初始化不成功 |
| EVMessageErrorInternalServer | -3001 | 消息服务器内部错误 |
| EVMessageErrorPermissionDenied | -3002 | 没有权限 | 
| EVMessageErrorJoinTopic | -3003 | 加入 topic 失败 |
| EVMessageErrorSend | -3004 | 发送失败 |
| EVMessageErrorShutuped | -3005 | 被禁言 |


