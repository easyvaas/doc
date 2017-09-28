## 直播端 API 参考说明 - iOS
### 推流

#### 推流相关属性

设置代理回调对象，可以回调 **流状态变化、缓存区状态变化、连麦状态、背景音乐播放器状态** 的变化

```objective-c
@property (nonatomic, weak) id<EVStreamerDelegate> delegate;
```

推流端是否静音，默认为NO

```objective-c
@property (nonatomic, assign) BOOL mute;
```

是否切换为前置摄像头，默认为NO

```objective-c
@property (nonatomic, assign) BOOL frontCamera;
```

是否使用闪光灯, 默认为 NO，前置摄像头状态下设置闪关灯不起作用

```objective-c
@property (nonatomic, assign) BOOL flashOn;
```

获取当前推流器的推流状态（只读）

```objective-c
@property (nonatomic, readonly, assign) EVVideoStreamerState state;
```

获取视频发送缓冲区状态（只读）

```objective-c
@property (nonatomic, readonly, assign) EVVideoStreamerStreamBufferState bufferState;
```

音频时长（只读）

```objective-c
@property (nonatomic, readonly, assign) NSInteger audioLength;
```

视频时长（只读）

```objective-c
@property (nonatomic, readonly, assign) NSInteger videoLength;
```

预览设置成镜像模式，默认为NO

```objective-c
@property (nonatomic, assign) BOOL previewMirrored;
```

推流设置成镜像模式, 默认为NO

```objective-c
@property (nonatomic, assign) BOOL streamerMirrored;
```

音效类型 default = EVAudioEffectType_NONE

```objective-c
@property (nonatomic, assign) EVAudioEffectType effectType;
```

**注：以下参数要在 livePrepareComplete: 之前完成**

推流器预览视图，推流前需要将其插入到目标视图中（只读）

```objective-c
@property (nonatomic, readonly, weak) UIView *preview;
```

视频采集帧率，默认为25，暂时不建议修改

```objective-c
@property (nonatomic, assign) NSUInteger captureFps;
```

摄像头采集输出的分辨率 默认为 CCRecoderVideoFrameSize_1280x720

```objective-c
@property (nonatomic, assign) EVStreamerVideoFrameSize captureFrameSize;
```

上传到服务器的视频流，每帧的大小（宽高），默认为 CCRecorderStreamFrameSize_360x640

```objective-c
@property (nonatomic, assign) EVStreamFrameSize streamFrameSize;
```

视频编码器类型, default = EVVideoCodec_AUTO

```objective-c
@property (nonatomic, assign) EVVideoCodec videoCodec;
```

本次直播的目标场景 default = EVLiveScene_Default

```objective-c
@property (nonatomic, assign) EVLiveScene liveScene;
```

视频初始化码率默认为 700 kbps, 然后会根据网络情况动态调整

```objective-c
@property (nonatomic, assign) NSUInteger videoBitrate;
```

视频最大码率, 默认 800 kbps

```objective-c
@property (nonatomic, assign) NSUInteger maxVideoBitrate;
```

视频最小码率，默认 200 kbps

```objective-c
@property (nonatomic, assign) NSUInteger minVideoBitrate;
```

音频编码码率(单位：kbps), 默认为48 kpbs

```objective-c
@property (nonatomic, assign) EVStreamerAudioBitrate audioBitrate;
```

是否开启横屏直播，如开启，则必须让屏幕处于 UIInterfaceOrientationLandscapeRight 状态 

```objective-c
@property (nonatomic, assign) BOOL useHorizonMode;
```

立体声推流，主播推流为单声道，背景音乐如果是双声道的音频,通过修改bStereoStream, 可以让观众听到双声道的音乐。只能在开始推流前修改本属性, 开始推流后, 修改无效 default = NO

```objective-c
@property (nonatomic, assign) BOOL bStereoAudioStream;
```

视频 id (必填)

```objective-c
@property (nonatomic, copy) NSString *lid;
```

水印 logo 图片

```objective-c
@property (strong, nonatomic) UIImage *watermakLogoImage;
```

水印logo的相关信息，内部包含水印的透明度，以及水印的位置和大小
传入格式为:

```objective-c
@{
    @"alpha": @(1),
    @"relativeFrame": NSStringFromCGRect(frame)
}
```

```objective-c
@property (strong, nonatomic) NSDictionary *watermakLogoInfo;
```

---

#### 推流相关方法

开启直播前的准备方法，处理完成后会回调 `EVStreamerCompleteBlock` block，返回 *错误码（EVStreamerResponseCode枚举）、结果信息（字典）、错误信息（NSError）*，当错误码为 `EVStreamerResponse_Okay` 时，调用开启直播方法

```objective-c
- (void)livePrepareComplete:(EVStreamerCompleteBlock)complete;
```

开启预览

```objective-c
- (void)startPreview;
```

关闭预览

```objective-c
- (void)stopPreview;
```

开启直播，处理完成后会回调 `EVStreamerCompleteBlock` block，返回 *错误码（EVStreamerResponseCode枚举）、结果信息（字典）、错误信息（NSError）*，当错误码为 `EVStreamerResponse_Okay` 时，调用开始推流方法

```objective-c
- (void)liveStartComplete:(EVStreamerCompleteBlock)complete;
```

开始推流，必须在 `-liveStartComplete:` 回调 `EVStreamerResponse_Okay` 后调用才有作用

```objective-c
- (void)startStream;
```

停止推流

```objective-c
- (void)stopStream;
```

重新连接

```objective-c
- (void)retryConnect;
```

关闭推流

```objective-c
- (void)shutDown;
```

闪光灯开关

```objective-c
- (void)turnOnFlashLight:(BOOL)on;
```

是否显示水印

```objective-c
- (void)enableWatermark:(BOOL)on;
```

获取当前是否开启了水印

```objective-c
- (BOOL)isWatermakOn;
```

是否开启美颜
*注：美颜功能仅限 iPhone5s 以后的机型，并且 iOS8 以后系统使用*

```objective-c
- (void)enableFaceBeauty:(BOOL)enabled;
```

设置美颜级别（只在开启美颜状态下有效）
*注：美颜级别 level [1, 5]，逐级增强, 默认为3*

```objective-c
- (void)configBeautyLevel:(NSInteger)level;
```

获取当前是否开启了美颜

```objective-c
- (BOOL)isFaceBeautyEnabled;
```

截取当前画面的 block，返回截取后的图片

```objective-c
- (void)getCapture:(void(^)(UIImage *image))imageBlock;
```

视频处理回调接口，sampleBuffer 为原始采集到的视频数据

```objective-c
@property(nonatomic, copy) void(^videoProcessingCallback)(CMSampleBufferRef sampleBuffer);
```

音频处理回调接口，sampleBuffer 为原始采集到的音频数据

```objective-c
@property(nonatomic, copy) void(^audioProcessingCallback)(CMSampleBufferRef sampleBuffer);
```

获取 EVStreamer 模块版本号信息

```objective-c
+ (NSString *)getVersion;
```

### 连麦

连麦 ID，使用连麦功能必须在 `-livePrepareComplete:` 方法调用前设置连麦 ID

```objective-c
@property (nonatomic, copy) NSString *agoraAppid;
```

加入一个频道进行连麦

```objective-c
- (void)joinChannel:(NSString *)channel;
```

离开当前连麦频道

```objective-c
- (void)leaveChannel;
```

主辅窗口视图切换

```objective-c
@property (nonatomic, assign) BOOL selfInFront;
```

连麦状态（只读）

```objective-c
@property (nonatomic, readonly, assign) EVVideoChatState chatState;
```

连麦小后窗口图层的大小
*注:取值区间为[0,1] eg:CGRectMake(0, 0.7, 0.3, 0.3)*

```objective-c
@property (nonatomic, readwrite) CGRect winRect;
```

### 背景音乐

播放背景音乐
*注：filePath 必须为本地音乐的路径*

```objective-c
- (void)BGMPlayWithPath:(NSString *)filePath;
```

暂停播放

```objective-c
- (void)BGMPause;
```

恢复播放

```objective-c
- (void)BGMResume;
```

停止播放

```objective-c
- (void)BGMStop;
```

背景音乐“混音”和“耳返”的开关，默认为混音状态，如果未插入耳机外放音乐时，观看端有声音重叠的感觉，可以通过关闭混音解决
*注：未插入耳机时，开启耳返会有很大噪音*

```objective-c
- (void)BGMTrackAndPlayCapturedAudioSwitch:(BOOL)YorN;
```

当前背景音乐播放状态（只读）

```objective-c
@property (nonatomic, readonly, assign) EVAudioPlayerState BGMState;
```

是否支持循环播放

```objective-c
@property (nonatomic, assign) BOOL BGMSupportLoop;
```

播放中背景音乐在本地的音量，不会印象观看端音量

```objective-c
@property (nonatomic, assign) float BGMVolume;
```

音乐的音调，调整范围 [-24.0 ~ 24.0], 默认为0.01, 单位为半音。  0.01 为1度, 1.0为一个半音, 12个半音为1个八度

```objective-c
@property (nonatomic, assign) double BGMPitch;
```

当前背景音乐播放进度

```objective-c
@property (nonatomic, readonly ,assign) CGFloat BGMCurrentPlayTime;
```

背景音乐音频时长

```objective-c
@property (nonatomic, readonly, assign) CGFloat BGMDuration;
```



