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

#### 初始化播放器
初始化播放器时，需要提供一个承载视频播放的视图，以及视频的 `lid`，标记其是否为直播视频：

```objective-c
self.player = [[EVPlayer alloc] init];
self.player.playerContainerView = self.containerView;
self.player.live = YES;
self.player.lid = self.vid;
```

#### 准备播放
播放视频前需要先调用 `- (void)playPrepareComplete:(EVPlayerCompleteBlock)complete;` 方法，让SDK对即将开始拉流的操作进行一些处理。

```objective-c
__weak typeof(self) wSelf = self;
    [self.player playPrepareComplete:^(EVPlayerResponseCode responseCode, NSDictionary *result, NSError *err) {
        __strong typeof(wSelf) sSelf = wSelf;
        if (responseCode == EVPlayerResponse_Okay) {
            // 准备操作无异常
        }
    }];

```

经过播放器准备处理后，SDK 会返回准备的回调结果，如果为`EVStreamerResponse_Okay`，则准备开始播放；如为其他 ResponseCode，则表示出现问题，可以弹框提示用户，并联系客服解决。

#### 开始播放

调用 `- (void)play;` 方法播放视频：

```objective-c
[self.player play];
```

#### 接收回调
进行开始播放操作后，可以通过之前设置的代理对象，来接收播放状态的回调，并根据不同的播放状态进行响应业务逻辑处理：

```objective-c
#pragma mark - EVPlayerDelegate
- (void)EVPlayer:(EVPlayer *)player didChangedState:(EVPlayerState)state{
    // 播放状态改变的回调
}
```

#### 获取录播播放进度
播放器播放录播时大多都需要获取当前的播放进度，EVPlayer 中，可以通过如下属性获取时间并显示：

* `currentPlaybackTime` 属性获取当前的播放时长
* `duration` 属性获取视频总时长
* `playableDuration` 属性获取可播放（缓存）视频时长
* `currentPlaybackTime / duration` 获取播放百分比

#### 设置播放器尺寸
通过设置 `playerViewFrame` 属性修改播放器的位置、大小：

```objective-c
self.player.playerViewFrame = newFrame;
```

#### 播放横屏视频
如果 `EVStreamer` 使用横屏推流，则播放端可以设置为横屏播放来进行适配：

```objective-c
[self.player showHorizonVideo:YES];
```

*重新调用该方法，传入 `NO` 则恢复播放器方向。*

#### 暂停播放

暂停播放：

```objective-c
[self.player pause];
```

#### 停止播放

停止播放：

```objective-c
EVPlayerDidFinishPlay
```


