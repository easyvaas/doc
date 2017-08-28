## 多人连麦 API 参考说明

设置代理对象，可以回调 **已加入频道、远端视频第一帧已解码、有用户离开** 等事件

```objective-c
@property (nonatomic, weak) id<EVRTCDelegate> delegate;
```

查看当前用户是否为当前频道的 Master

```objective-c
@property (nonatomic, assign, readonly) BOOL isMaster;
```

当前频道主播 uid
*注：默认 masterUid=9999999999，加入连麦成功后获取正确的频道主播 uid*

```objective-c
@property (nonatomic, assign, readonly) NSUInteger masterUid;
```

设置连麦视频分辨率，有三种清晰度可供选择：高清、标清、流畅（默认为标清分辨率）
*注：需要在加入频道前设置*

```objective-c
@property (nonatomic, assign) EVRtcVideoProfile profile;
```

初始化方法，必须传入连麦 ID

```objective-c
- (instancetype)initWithRTCID:(NSString *)rtcId;
```

创建并加入一个频道，可以自定义频道名，频道名传入 `nil` 则自动生成一个唯一的 channel；可以设置自定义uid，uid 如果传入 0 则自动生成一个 uid；是否开启旁路推流，是否保存视频（对应操作身份为 Master）

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)createAndJoinChannel:(NSString *)channel uid:(NSUInteger)uid hasPublisher:(BOOL)hasPublisher record:(BOOL)record callback:(EVRTCCallback)callback;
```

加入一个频道，必须传入频道名，可以设置自定义uid，uid 如果传入 0 则自动生成一个 uid（对应操作身份为 LiveGuest）

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)joinChannel:(NSString *)channel uid:(NSUInteger)uid callback:(EVRTCCallback)callback;
```

离开当前频道，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)leaveChannel;
```

配置该 uid 用户本地画面的显示方式

```objective-c
- (void)configCanvasWithView:(UIView *)view uid:(NSUInteger)uid mode:(EVRtcRenderMode)mode;
```

配置旁路推流画面中，各个画面的位置信息 （只有频道 Master 才有权限调用此方法）
*注：主播可以不调用此方法，SDK会使用默认的旁路推流布局，其中数字代表连麦观众显示的顺序，最外层为屏幕边缘，具体效果详见 Demo 运行效果*

```
--------------------------------
|       |     4    |     1     |
|       -----------------------|
|       |     5    |     2     |
|       -----------------------|
|       |     6    |     3     |
--------------------------------
```
```objective-c
- (void)configVideoRegion:(NSArray <EVRTCVideoRegion*> *)regions;
```

设置远端视频的清晰度，默认为高清`EVRtc_VideoStream_High`

```objective-c
- (void)configRemoteVideoStream:(NSUInteger)uid type:(EVRtcVideoStreamType)streamType;
```

获取直播中某频道的旁路推流地址

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)watchLiveWithChannel:(NSString *)channel callback:(EVRTCCallback)callback;
```

获取录播中某频道的旁路推流地址（如果频道 Master 未保存视频流，则无法获取）

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)watchRecordWithChannel:(NSString *)channel callback:(EVRTCCallback)callback;
```

获取特定频道分享地址，分享地址为H5，可播放直播视频

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objectvie-c
- (void)fetchShareURLWithChannel:(NSString *)channel callback:(EVRTCCallback)callback;
```

本地音频推流开关，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)muteLocalAudioStream:(BOOL)mute;
```
本地视频推流开关，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)muteLocalVideoStream:(BOOL)mute;
```

切换本地摄像头方向，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)switchCamera;
```

获取 EVRTC 模块版本号信息

```objective-c
- (NSString *)getVersion;
```

#### EVRTCDelegate 代理回调方法

加入频道成功回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didJoinChannel:(NSString *)channel withUid:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

远端视频流第一帧已解码后的回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit firstRemoteVideoDecodedOfUid:(NSUInteger)uid size:(CGSize)size elapsed:(NSInteger)elapsed;
```

本地视频已渲染到屏幕中的回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit firstLocalVideoFrameWithSize:(CGSize)size elapsed:(NSInteger)elapsed;
```

远端用户是否主动禁用了音频

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

远端用户是否主动禁用了视频

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didVideoMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

当前用户网络连接中断

```objective-c
- (void)evRTCKitConnectionDidInterrupted:(EVRTCKit *)kit;
```

当前用户网络连接丢失

```objective-c
- (void)evRTCKitConnectionDidLost:(EVRTCKit *)kit;
```

频道中有用户断线的回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didOfflineOfUid:(NSUInteger)uid reason:(EVRtcOfflineReason)reason;
```

连麦中出错的回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didOccurErrorWithCode:(NSInteger)errorCode;
```



