## 多人连麦 API 参考说明

设置代理对象，可以回调 **已加入频道、远端视频第一帧已解码、有用户离开** 等事件

```objective-c
@property (nonatomic, weak) id<EVRTCDelegate> delegate;
```

查看当前用户是否为当前频道的 Owner

```objective-c
@property (nonatomic, assign, readonly) BOOL isOwner;
```

初始化方法，必须传入连麦 ID

```objective-c
- (instancetype)initWithRTCID:(NSString *)rtcId;
```

创建并加入一个频道，可以设置是否开启旁路推流，是否保存视频（对应操作身份为 Owner）

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)createAndJoinChannel:(NSString *)channel hasPublisher:(BOOL)hasPublisher record:(BOOL)record callback:(EVRTCCallback)callback;
```

加入一个频道（对应操作身份为 Broadcaster）

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)joinChannel:(NSString *)channel callback:(EVRTCCallback)callback;
```

离开当前频道，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)leaveChannel;
```

配置该 uid 用户本地画面的显示方式

```objective-c
- (void)configCanvasWithView:(UIView *)view uid:(NSUInteger)uid mode:(EVRtcRenderMode)mode;
```

配置旁路推流画面中，各个画面的位置信息 （只有频道 owner 才有权限调用此方法）

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

获取录播中某频道的旁路推流地址（如果频道 owner 未保存视频流，则无法获取）

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
- (void)watchRecordWithChannel:(NSString *)channel callback:(EVRTCCallback)callback;
```

关闭本地音频推流，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)muteLocalAudioStream:(BOOL)mute;
```

切换本地摄像头方向，返回 0 则成功，返回负数则表明出现异常

```objective-c
- (int)switchCamera;
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

频道中有用户断线的回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didOfflineOfUid:(NSUInteger)uid reason:(EVRtcOfflineReason)reason;
```

连麦中出错的回调

```objective-c
- (void)evRTCKit:(EVRTCKit *)kit didOccurErrorWithCode:(NSInteger)errorCode;
```



