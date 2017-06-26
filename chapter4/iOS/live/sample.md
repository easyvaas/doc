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

#### 初始化推流器
初始化推流器没有特殊的配置，只是一个对象的正常初始化，之后会对其相关属性进行配置

```objective-c
_streamer = [[EVStreamer alloc] init];
```

#### 配置推流器
推流器初始化后，需要对相关属性进行设置，以保证获得期望的推流效果：

```objective-c
[self.view insertSubview:self.streamer.preview atIndex:0];
self.streamer.delegate = self;
self.streamer.videoBitrate = (NSNumber *)videoBitrate;
self.streamer.audioBitrate = (NSNumber *)audioBitrate;
self.streamer.mute = [isMute boolValue];
self.streamer.frontCamera = [useFrontCamera boolValue];
[self.streamer enableFaceBeauty:YES];
```

首先需要将推流器的预览视图插入到目标视图的最底层，摄像头采集的图像会通过预览视图显示到屏幕上。
将当前控制器设置为代理，便于接受推流器开始工作后的相关回调信息。
配置*视频码率、音频码率、前后置摄像头、美颜*等相关属性，部分参数只能在推流开始前配置。

#### 准备推流
推流开始前，需要调用`- (void)livePrepareComplete:(EVStreamerCompleteBlock)complete;`推流准备方法，让SDK对即将开始的推流进行一些处理。

```objective-c
__weak typeof(self) wSelf = self;
[self.streamer livePrepareComplete:^(EVStreamerResponseCode responseCode, NSDictionary *result, NSError *err) {
   __strong typeof(wSelf) sSelf = wSelf;
   if (responseCode == EVStreamerResponse_Okay) {
       [sSelf startLive];
   } else {
        // 推流器错误
   }
}];
```

经过推流器准备处理后，SDK 会返回准备的回调结果，如果为`EVStreamerResponse_Okay`，则准备开始直播；如为其他 ResponseCode，则表示出现问题，可以弹框提示用户，并联系客服解决。

#### 开始预览
开始推流前，需要开启预览视图，并获取当前推流的 `lid`。

开始预览：

```objective-c
[self.streamer startPreview];

```

获取推流视频的 `lid`：

```objective-c
- (void)generateLid{
    NSString *url = [NSString stringWithFormat:@"http://video.api.easyvaas.com/genstream?appid=%@", [EVSDKManager appID]];
    
    __weak typeof(self) wSelf = self;
    [EVNetRequestManager httpGetWithUrl:url moreHeaders:nil success:^(NSDictionary *info) {
        __strong typeof(wSelf) sSelf = wSelf;
        NSNumber *state = info[@"state"];
        
        if ([state integerValue] == 0) {
            NSDictionary *content = info[@"content"];
            NSString *lid = content[@"lid"];
            NSString *key = content[@"key"];
            if (lid && key) {
                [sSelf startWithVid:lid key:key];
            }
        } else {
            // 获取 lid 出错
        }
    } failure:^(NSError *err) {
        // 获取 lid 失败
    }];
}
```

获取推流视频的 `lid` 是使用已申请的 `AppID` ，通过 GET 的网络请求方式获取，回调数据为 JSON 格式，如果成功返回 lid 则开始直播；如失败则表示出现问题，可以弹框提示用户，并联系客服解决。

#### 开始推流
准备工作全部完成后，将获取的 `lid`、`key` 赋值给推流器，开始直播：

```objective-c
self.streamer.lid = self.vid;
self.streamer.key = self.key;

__weak typeof(self) wSelf = self;
[self.streamer liveStartComplete:^(EVStreamerResponseCode responseCode, NSDictionary *result, NSError *err) {
   __strong typeof(wSelf) sSelf = wSelf;
   
   if (responseCode == EVStreamerResponse_Okay) {
       [sSelf.streamer startStream];
   }
}];
```

#### 接收回调
进行开始推流操作后，可以通过之前设置的代理对象，来接受推流状态的回调，并根据不同的推流状态进行响应业务逻辑处理：

```objective-c
#pragma mark - EVStreamerDelegate
- (void)EVStreamerStreamStateChanged:(EVVideoStreamerState)state{
    switch (state) {
        case EVVideoStreamerStreamIdle:
            NSLog(@"闲置状态，未推流");
            break;
            
        case EVVideoStreamerStreamError:
            NSLog(@"推流失败");
            break;
            
        case EVVideoStreamerStreamConnecting:
            NSLog(@"服务器连接中");
            break;
            
        case EVVideoStreamerStreamConnected:
            NSLog(@"开始推流");
            break;
            
        case EVVideoStreamerStreamDisconnecting:
            NSLog(@"断开连接");
            break;
            
        default:
            break;
    }
}
```

在推流过程中，除了流状态的变化，也会有网络抖动等情况的发生，可以通过网络状态变化的回调进行对应处理：

```objective-c
- (void)EVStreamerBufferStateChanged:(EVVideoStreamerStreamBufferState)state{
    switch (state) {
        case EVVideoStreamerStreamBufferStateNormal:
            NSLog(@"正常推流");
            break;
            
        case EVVideoStreamerStreamBufferStateLv1:
            NSLog(@"当前网络比较差，等级1，还可以坚持继续播");
            break;
            
        case EVVideoStreamerStreamBufferStateLv2:
            NSLog(@"当前网络弱爆了，等级2，建议关闭直播");
            break;
            
        case EVVideoStreamerStreamBufferStateLv3:
            NSLog(@"当前网络无法直播，可以直接掐掉了");
            break;
            
        default:
            break;
    }
}
```

#### 关闭推流
当需要结束推流时，调用 `- (void)shutDown;` 方法结束推流即可：

```objective-c
[self.streamer shutDown];
```

