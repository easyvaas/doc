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

#### 初始化连麦类

初始化多人连麦类 `EVRTCKit` 时，必须传入已申请的连麦 ID

```objective-c
self.rtcKit = [[EVRTCKit alloc] initWithRTCID:@"your_rtcId"];
```

#### 设置代理

用于接收 SDK 回调的连麦状态等信息

```objective-c
self.rtcKit.delegate = self;
```

#### 创建本地视频画布

创建本地视频采集后显示的容器视图。可以使用自定义的辅助类 `VideoSession` 在内部创建 UIView，并保存 uid，创建完成后，调用 SDK 内部方法`- (void)configCanvasWithView:(UIView *)view uid:(NSUInteger)uid mode:(EVRtcRenderMode)mode;`进行配置

传入 uid = 0，作为本地视频的标识，当需要创建其他远端视频的画布时，则传入对应的 uid

```objective-c
VideoSession *newSession = [[VideoSession alloc] initWithUID:uid];
[self.rtcKit configCanvasWithView:newSession.hostingView uid:newSession.uid mode:EVRtc_Render_Fit];
```

#### 创建频道

Easyvaas 多人连麦 SDK 中提供了三种身份：Master（主播）、LiveGuest（连麦观众）、Guest（观众），一个频道中只能有一个 Master，可以有最多六个 LiveGuest，若干 Guest。

Master 和 LiveGuest 加入频道的方式是不一样的，此处以 Master 为例

Master 作为频道的拥有者，需要调用 `- (void)createAndJoinChannel:(NSString *)channel uid:(NSUInteger)uid hasPublisher:(BOOL)hasPublisher record:(BOOL)record callback:(EVRTCCallback)callback;` 方法创建并加入频道，可以设置是否进行*旁路推流*，以及是否需要*保存视频*

处理后，SDK 会返回回调结果，如果为`EVRtcResponseCode_None`，则准备开始直播；如为其他 code，则表示出现问题，可以弹框提示用户，并联系客服解决。

```objective-c
[self.rtcKit createAndJoinChannel:customChannel uid:0 hasPublisher:YES record:YES callback:^(EVRtcResponseCode code, NSDictionary *info, NSError *error) {
    if (code == EVRtcResponseCode_None) {
        // 创建、加入频道成功
    } else {
        // 创建、加入频道失败
    }
}];
```

#### 设置旁路推流布局

Master 在*创建频道*的方法中，可以设置是否进行*旁路推流*(`hasPublisher`)，如果设置为 `true`，则 Master 可以通过调用 `- (void)configVideoRegion:(NSArray <EVRTCVideoRegion*> *)regions;` 方法，设置当前频道中各个用户在旁路视频流中的视频画面布局

*注：只有 Master 身份才可以设置旁路推流视频布局*

```objective-c
[self.rtcKit configVideoRegion:regions];
```

#### 离开频道

离开当前频道只需要调用 `- (int)leaveChannel;` 方法即可

```objective-c
[self.rtcKit leaveChannel];
```

---  
多人连麦中很多方法有特定的调用位置，可以下载 [EVSDKDemo](https://github.com/easyvaas/sdk_demo_iOS) 查看详情。


