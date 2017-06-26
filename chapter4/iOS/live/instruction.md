## 功能详细说明

#### 推流参数设置
* 视频分辨率  
目前支持(360x640)、(540x960)、(720x1280)三种分辨率，分别对应的枚举值为：
    * EVStreamFrameSize_360x640
    * EVStreamFrameSize_540x960
    * EVStreamFrameSize_720x1280
默认的枚举值为 `EVStreamFrameSize_default` 分辨率为 (360x640)，即 `EVStreamFrameSize_360x640`。

* 视频码率
SDK 推流器的默认视频码率为 *700 kbps*，默认最大视频码率 *800 kbps*，最小视频码率 *200 kbps*。SDK 支持根据网络动态调整视频码率，当网络较差时调低码率保证视频的流畅性，网络状态变好时提高视频码率以提供更清晰的视频质量。

* 音频码率
音频码率由 `EVStreamerAudioBitrate` 枚举管理，目前支持 32、48、64、128 （单位：kbps），分别对应如下枚举值：
    * EVStreamerAudioBitrate_32
    * EVStreamerAudioBitrate_48
    * EVStreamerAudioBitrate_64 
    * EVStreamerAudioBitrate_128  
默认为 48 kbps，即 `EVStreamerAudioBitrate_48`。

#### 网络错误码
当进行 `- (void)livePrepareComplete:(EVStreamerCompleteBlock)complete;` 与 `- (void)liveStartComplete:(EVStreamerCompleteBlock)complete;` 操作时，会在回调 block 中回调错误码，错误码列表如下：

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|EVStreamerResponse_Okay|0|无错误|
|EVStreamerResponse_error_sdkNotInitedOrInitedFailure|1|没有进行sdk初始化，或者sdk初始化失败|
|EVStreamerResponse_error_sdkLiveRequestError|2|直播请求失败|
|EVStreamerResponse_error_sdkNotLivePrepareOrPrepareFailure|3|没有调用直播准备接口，或调用直播准备失败|
|EVStreamerResponse_error_sdkNoLid|4|没有设置 lid, lid 一定要在 liveStart 之前设置|
|EVStreamerResponse_error_sdkNoURI|5|没有设置URI, URI 一定要在 liveStart 之前设置|
|EVStreamerResponse_error_sdkInitHardware|6|初始化硬件错误|
|EVStreamerResponse_error_sdkNoKey|7|没有设置 key|

#### 横屏直播
推流器支持横屏直播，采用当前主流的 `UIInterfaceOrientationLandscapeRight` 屏幕方向，SDK 使用者需要先将屏幕方向设置为 UIInterfaceOrientationLandscapeRight，然后设置 `useHorizonMode` 属性为 `YES`，则可开启横屏直播。

#### 背景音乐播放
推流器支持背景音乐播放功能，但需要将音乐文件下载到本地，传入本地文件路径进行播放。

播放背景音乐：

```objective-c
[self.streamer BGMPlayWithPath:musicePath];
```

暂停背景音乐：

```objective-c
[self.streamer BGMPause];
```

调节背景音乐在本地播放的音量：

```objective-c
self.streamer.BGMVolume
```

恢复播放背景音乐：

```objective-c
[self.streamer BGMResume];
```

背景音乐支持混音，如开始播放音乐后，通过 `BGMVolume` 属性将本地播放的音量调到 0，观看端仍然可以听到主播和音乐的声音。默认为混音状态，如果未插入耳机外放音乐时，观看端有声音重叠的感觉，可以通过关闭混音解决，方法如下：

```objective-c
- (void)BGMTrackAndPlayCapturedAudioSwitch:(BOOL)YorN;
```
*注：未插入耳机时，开启耳返会有很大噪音*

#### 美颜
推流器支持美颜功能，主播可以自己决定是否开启美颜，并可以在开启美颜后设置美颜级别。

美颜开关：

```objective-c
- (void)enableFaceBeauty:(BOOL)enabled;
```

设置美颜级别(区间为[1, 5])：

```objective-c
- (void)configBeautyLevel:(NSInteger)level;
```

默认美颜级别为 3.

#### 主辅连麦
直播端SDK支持连麦功能，在直播过程中特定观众可以发起连麦请求，主播接受连麦请求后，连麦观众和主播之间进行实时互动。目前版本仅支持1对1连麦。

**重要提示**：要测试或使用连麦功能，需要向易视云商务人员申请连麦id，否则点击界面上的连麦按钮Demo会提示“请先设置agoraAppid”。

主播端已开始直播，推流器对象已初始化完毕。

加入频道 `evdemo02` 进行连麦：

```objective-c
[self.streamer joinChannel:@"evdemo02"];
```

连麦状态会通过 `streamer` 的代理回调给代理类：

```objective-c
- (void)EVStreamerUpdateVideoChatState:(EVVideoChatState)chatState {
    // 当前连麦状态
}
```

通过设置属性切换主辅视频窗口：

```objective-c
@property (nonatomic, assign) BOOL selfInFront;
```

可以通过设置如下属性修改小窗口的位置、大小：

```objective-c
@property (nonatomic, readwrite) CGRect winRect; 
```

离开当前连麦频道：

```objective-c
[self.streamer leaveChannel];
```


