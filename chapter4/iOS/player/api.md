## 观看端 API 参考说明 - iOS

#### 拉流相关属性

播放器将会在此 view 上渲染播放画面,必须在调用 playPrepareComplete: 之前设置(必填)

```objective-c
@property (nonatomic, strong) UIView *playerContainerView;
```

是否开启硬件解码。如果系统版本高于8.0，默认开启硬件解码；否则，默认使用软件解码 default = EVMovieVideoDecoderMode_AUTO (需要在 -prepare 之前配置)

```objective-c
@property (nonatomic, assign) EVMovieVideoDecoderMode videoDecoderMode;
```

bufferTimeMax指定播放时的缓冲时长，单位秒
对于直播流，该属性用于直播延时控制；对于点播流，该属性用于缓冲时长控制
直播流该属性默认为2秒，设置为0或负值时为关闭直播追赶
点播流该属性默认为3600秒，且与bufferSizeMax同时生效，两者取小值

```objective-c
@property (nonatomic, assign) NSTimeInterval bufferTimeMax;
```

指定逆时针旋转角度，只能是0/90/180/270, 不符合上述值不进行旋转

```objective-c
@property (nonatomic, assign) int rotateDegress; 
```

是否是直播视频（必填）

```objective-c
@property (nonatomic, assign) BOOL live;
```

是否静音 default = NO

```objective-c
@property (nonatomic, assign) BOOL shouldMute; 
```

是否隐藏视频，隐藏视频时播放器本身不再进行渲染动作，如果设置了videoDataBlock回调，隐藏视频时数据会照常上抛 default = NO

```objective-c
@property (nonatomic, assign) BOOL shouldHideVideo;
```

指定视频是否镜像显示

```objective-c
@property (nonatomic, assign) BOOL mirror; 
```

播放器视图的位置及尺寸

```objective-c
@property (nonatomic, assign) CGRect playerViewFrame;
```

视频的 `lid` （必填）

```objective-c
@property (nonatomic, copy) NSString *lid;
```

播放器的代理

```objective-c
@property (nonatomic, weak) id<EVPlayerDelegate> delegate;
```

总回放的时长（单位：s）（只读）

```objective-c
@property (nonatomic, readonly) NSTimeInterval duration;
```

可播放的时长，即缓冲时长（单位：s）（只读）

```objective-c
@property (nonatomic, readonly) NSTimeInterval playableDuration;
```

当前回放到的时刻，设置它可以改变回放的进度（单位：s）

```objective-c
@property (nonatomic) NSTimeInterval currentPlaybackTime;
```

播放视图的填充模式，默认为 `MPMovieScalingModeAspectFit`

```objective-c
@property (nonatomic) MPMovieScalingMode scalingMode;
```

视频数据回调(需要在 -prepare 之前配置)

```objective-c
@property (nonatomic, copy) void(^videoDataBlock)(CMSampleBufferRef pixelBuffer);
```

音频数据回调(需要在 -prepare 之前配置)

```objective-c
@property (nonatomic, copy) void(^audioDataBlock)(CMSampleBufferRef sampleBuffer);
```

#### 拉流相关方法

播放准备方法，处理完成后会回调 `EVPlayerCompleteBlock` block，包含 *错误码（EVPlayerResponseCode枚举）、结果信息（字典）、错误信息（NSError）*，如果错误码为 `EVPlayerResponse_Okay`，则可以开始播放

```objective-c
- (void)playPrepareComplete:(EVPlayerCompleteBlock)complete;
```

播放视频，必须在 `-playPrepareComplete:` 回调后调用

```objective-c
- (void)play;
```

暂停播放

```objective-c
- (void)pause;
```

停止播放

```objective-c
- (void)shutDown;
```

当前时刻的视频截图

```objective-c
- (UIImage *)thumbnailImageAtCurrentTime;
```

设置手机竖屏模式下横屏播放

```objective-c
- (void)setVideoPlayInHorizonMode;
```

是否显示横屏视频

```objective-c
- (void)showHorizonVideo:(BOOL)YorN;
```

跳转到制定播放位置（精准定位，适用于媒体文件总时长较小且关键帧间隔较大时）（单位：s）

```objective-c
- (void)seekTo:(double)pos;
```

指定播放器输出音量
参数范围：[0~2.0f] 输入参数超出范围将失效

```objective-c
- (void)setVolume:(float)volume;
```

获取 EVPlayer 模块版本号信息

```objective-c
+ (NSString *)getVersion;
```


