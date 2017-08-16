## 功能详细说明

#### 改变录播播放进度
EVPlayer 为录播播放进度的修改，提供了两种修改的方式：

```objective-c
@property (nonatomic) NSTimeInterval currentPlaybackTime;
```

```objective-c
- (void)seekTo:(double)pos;
```

通过设置 `currentPlaybackTime` 改变播放进度是通过关键帧进行修改；
通过调用 `-seekTo:` 方法改变播放进度是精准定位修改；

可以根据具体情况采用合适的方式，对播放进度进行调整。


#### 播放回调
播放器除了回调播放状态外，还提供了

视频第一帧渲染的回调：

```objective-c
- (void)EVPlayerFirstVideoFrameDidRender:(EVPlayer *)player;
```

提示需要重新加载的回调：

```objective-c
- (void)EVPlayerSeguestedToReload:(EVPlayer *)player;
```

播放结束的回调：

```objective-c
- (void)EVPlayerDidFinishPlay:(EVPlayer *)player reason:(MPMovieFinishReason)reason;
```

缓冲状态的回调（可根据此回调显示加载图）：

```objective-c
- (void)EVPlayer:(EVPlayer *)player cacheState:(EVPlayerCacheState)state;
```

通过不同的回调事件处理应用中的业务逻辑，让视频播放变得简单。

#### 枚举值
播放器准备时回调的错误码枚举列表：

|枚举值|数值|含义|
|:--|:--|:--|
|EVPlayerResponse_Okay|0|观看请求成功|
|EVPlayerResponse_error_sdkNotInitedOrInitedFailure|1|没有进行sdk初始化，或者sdk初始化失败|
|EVPlayerResponse_error_sdkPlayRequestError|2|观看请求失败|
|EVPlayerResponse_error_sdkNoPlayerContainerView|3|没有设置视频显示的 container view|
|EVPlayerResponse_error_sdkNoLid|4|没有设置 lid|

视频解码模式：

|枚举值|含义|
|:--|:--|
|EVMovieVideoDecoderMode_Software|视频解码方式采用软解|
|EVMovieVideoDecoderMode_Hardware|视频解码方式采用硬解|
|EVMovieVideoDecoderMode_AUTO|自动选择解码方式，8.0以上的系统优先选择硬解|
|EVMovieVideoDecoderMode_DisplayLayer|使用系统接口进行解码和渲染，只适用于8.0及以上系统，低于8.0的系统自动使用软解|

播放状态列表：

|枚举值|含义|
|:--|:--|
|EVPlayerStateUnknown|未知状态|
|EVPlayerStateBuffering|缓冲中|
|EVPlayerStatePlaying|播放中|
|EVPlayerStateComplete|视频结束|
|EVPlayerStateConnectFailed|连接失败|

缓冲状态列表：

|枚举值|含义|
|:--|:--|
|EVPlayerCacheStateCaching|缓冲中|
|EVPlayerCacheStateFinish|缓冲完毕|

