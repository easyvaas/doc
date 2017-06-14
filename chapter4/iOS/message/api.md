## 直播端API参考说明 - iOS
### - 主动调用方法
#### 初始化(EVMessageManager)
使用 Easyvaas 的消息系统时，首先需要初始化 SDK

```objective-c
[EVSDKManager initSDKWithAppID:appid appKey:appkey appSecret:appsecret userID:userid];
```

当接收到初始化 SDK 成功的通知后，方可继续进行其他操作。

针对消息系统的初始化而言，EVMessageManager 是一个内部维护的单例管理类，所以通常不需要进行单独的初始化操作，在设置代理对象时管理类自然会常驻内存。

#### 设置代理(EVMessageProtocol)

```objctive-c
@property (nonatomic, weak) id<EVMessageProtocol> delegate;
```
在 EVMessageManager 中设置了 delegate 代理属性，用户可以通过设置该代理来决定由哪个类来接收回调方法。

eg:

```objective-c
[EVMessageManager shareManager].delegate = self;
```

#### 连接话题(join topic)
使用 Easyvaas 的消息系统，第一步操作就是加入话题，成功加入话题后，才能进行消息系统内部发送消息、点赞等其他操作。

```objective-c
/**
 连接 topic

 @param topic 想加入的topic
 */
- (void)connect:(NSString *)topic;
```

连接方法传入的参数即为想要加入的话题，不能为空，连接成功后会走代理回调。

#### 发送消息（简易方法）
成功加入话题后，就可以同该话题下的其他人聊天了，我们提供了一个发送消息的简易方法，只需要传入最基础的消息内容等信息，未传入的参数为默认值。

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

#### 发送消息（全参数）
全参数发送消息的方法，主要增加了消息等级、是否保留为历史消息的设置，默认`level=EVMessageLevel_4、save=true、type=EVMessageTypeMsg`

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

#### 点赞操作（like）
点赞操作除了需要传入所在话题（topic）外，还可以传入点赞数，默认点赞数为1。

```objective-c
/**
 点赞操作

 @param topic 所在topic
 @param count 点赞数
 @param callBack 回调
 */
- (void)addLikeCountWithTopic:(NSString *)topic count:(NSUInteger)count result:(EVMessageCallBack)callBack;
```

#### 获取最近历史消息（history）
当新加入一个话题（topic）时，如果想获取该话题最近的历史消息，可以通过此接口获取，只需要传入所需要获取的历史消息数即可。

```objective-c
/**
 点赞操作

 @param topic 所在topic
 @param count 点赞数
 @param callBack 回调
 */
- (void)addLikeCountWithTopic:(NSString *)topic count:(NSUInteger)count result:(EVMessageCallBack)callBack;
```

获取历史消息成功后，如果有值，则从 response[@"content"] 中获取消息数组，数组中是 EVMessageModel 对象，用户可以根据自身需求对其进行处理。

#### 获取历史消息（history）
完整的获取历史消息接口，可以指定获取的 start、count、type 信息，以针对性的获取对应的历史消息。

```objective-c
/**
 获取历史消息

 @param topic 所在topic
 @param start 开始位置
 @param count 历史消息数
 @param type 消息类型
 @param callBack 回调（字典中的历史消息是 EVMessageModel 对象）
 */
- (void)getHistoryMessageWithTopic:(NSString *)topic start:(NSUInteger)start count:(NSUInteger)count type:(EVMessageType)type result:(EVMessageCallBack)callBack;
```

#### 离开话题（leave topic）
当用户想离开当前话题（topic）时，可以调用此方法，成功后会通过回调方法告知用户。

```objective-c
/**
 离开 topic

 @param topic 所操作的topic
 @param callBack 回调
 */
- (void)leaveWithTopic:(NSString *)topic result:(EVMessageCallBack)callBack;
```

#### 关闭连接（disconnect）
当用户想关闭 socket 连接时，可以调用此方法，成后会通过回调方法告知用户。

```objective-c
/**
 关闭连接
 */
- (void)closeConnect;
```

#### 用户操作block回调
用户在发送消息、点赞、获取历史消息、离开话题时，都会有 `EVMessageCallBack` block作为回调，可通过判断 error == nil 来获知当前操作是否成功。
获取历史消息时，会回调 response 字典，内部为历史消息内容。

```objective-c
typedef void(^EVMessageCallBack)(NSDictionary *response, NSError *error);
```

### - 代理回调方法（EVMessageProtocol）
#### 必须实现的方法（@required）
#### 连接成功
当调用`- (void)connect:(NSString *)topic;`连接加入话题成功时，会回调此代理。

```objective-c
/**
 *  消息服务器连接成功
 */
- (void)EVMessageConnected;
```

#### 连接失败
当调用`- (void)connect:(NSString *)topic;`连接加入话题失败时，会回调此代理。

```objective-c
/**
 *  消息服务器连接失败
 *
 *  @param error 失败原因
 */
- (void)EVMessageConnectError:(NSError *)error;
```


#### 可选实现的方法（@optional）

#### 连接断开
当处于连接中的 socket 由于其他原因断开时，回调此方法，用户可根据错误码查看分析原因。

用户在没有进行离开话题（leave topic）操作时，如果系统回调此方法，SDK 内部会为用户尝试重新连接，最大重连数为3次，重连过程中会给予用户状态回调。

```objective-c
/**
 *  断开连接
 *
 *  @param code   错误码
 *  @param reason 原因
 */
- (void)EVMessageDidCloseWithCode:(EVMessageErrorCode)code
                           reason:(NSString *)reason;
```

#### 收到新消息
当 socket 连接收到服务器下发的会话消息时，会将会话内容回调给用户，其中 userData 为用户自定义的字典信息，可用于传输头像地址等信息。

```objective-c
/**
 *  接收到新消息
 *
 *  @param topic    话题
 *  @param userid   用户 id
 *  @param message  消息内容
 *  @param userData 自定义消息
 */
- (void)EVMessageRecievedNewMessageInTopic:(NSString *)topic
                               sendedFrom:(NSString *)userid
                                  message:(NSString *)message
                                 userData:(NSString *)userData;
```

#### 用户加入当前话题
当用户所在话题（topic）有其他用户加入时，服务器会通过 socket 下发告知用户，SDk 内部会回调此消息给用户。

```objective-c
/**
 *  有用户加入话题
 *
 *  @param userids 加入的用户
 *  @param topic   话题
 */
- (void)EVMessageUsers:(NSArray <NSString *>*)userids
           joinedTopic:(NSString *)topic;
```

#### 用户离开当前话题
当用户所在话题（topic）中有用户离开（leave）时，服务器会通过 socket 下发告知用户，SDk 内部会回调此消息给用户。

```objective-c
/**
 *  有用户从话题离开
 *
 *  @param userids 离开的用户
 *  @param topic   话题
 */
- (void)EVMessageUsers:(NSArray <NSString *>*)userids
             leftTopic:(NSString *)topic;
```

#### 更新点赞数
此方法会回调当前话题（topic）的总点赞数，目前代理只有在用户调用了`- (void)addLikeCountWithTopic:(NSString *)topic count:(NSUInteger)count result:(EVMessageCallBack)callBack;`方法时才会回调。

```objective-c
/**
 *  更新点赞数
 *
 *  @param likeCount 更新后的点赞数
 *  @param topic     话题
 */
- (void)EVMessageDidUpdateLikeCount:(long long)likeCount
                            inTopic:(NSString *)topic;
```

#### 更新正在观看的人数
当用户所在话题（topic）的观看人数发生变化时，SDK 会通过此方法回调正在观看的人数给用户。

```objective-c
/**
 *  更新正在观看数
 *
 *  @param watchingCount 更新后的正在观看数
 *  @param topic         话题
 */
- (void)EVMessageDidUpdateWatchingCount:(NSInteger)watchingCount
                                inTopic:(NSString *)topic;
```

#### 更新已观看的人数
此回调会与*正在观看人数*的回调方法同步回调，用于更新已观看的人数。

```objective-c
/**
 *  更新观看人次
 *
 *  @param watchedCount 更新后的观看人次
 *  @param topic        话题
 */
- (void)EVMessageDidUpdateWatchedCount:(NSInteger)watchedCount
                               inTopic:(NSString *)topic;
```

### - 消息模型
当获取历史消息时，response 字典中回调的数组即是由`EVMessageModel`组成的，该对象中的属性代表一条消息的相关内容。

```objective-c
@interface EVMessageModel : NSObject

@property (nonatomic, copy) NSString *context;          /**< 消息内容 */
@property (nonatomic, copy) NSDictionary *userData;     /**< 透传消息 */
@property (nonatomic, copy) NSString *userID;           /**< 用户标识 */
@property (nonatomic, assign) EVMessageType type;       /**< 消息类型 */

@end
```

