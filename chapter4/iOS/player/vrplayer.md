## VR 播放器 

###  环境配置

VR 播放器，则导入如下 framework：

```
- EVVRFramework.framework
- ios_player_dylib.framework
```

注：  

* 其中 `ios_player_dylib.framework` 为动态库，需要添加到项目工程 TARGETS->General->Embedded Binaries 中，否则运行应用时控制台会输出 **image not found** 的错误
* `EVMediaFramework.framework` 和 `ios_player_dylib.framework` 同时导入时，需要在项目工程 TARGETS->Build Settings->Other Linker Flags 中添加 **-all_load**

#### 系统库配置

导入如下系统库：

```
libc++.tbd
libstdc++.tbd
CoreAudioKit.framework
CoreAudio.framework
CFNetwork.framework
AVFoundation.framework
libxml2.tbd
```
#### 网络配置
由于SDK内部存在鉴权的网络请求，所以需要打开配置ATS，在工程的`info.plist`中添加如下内容：

```objective-c
<key>NSAppTransportSecurity</key>
<dict>
   <key>NSAllowsArbitraryLoads</key>
   <true/>
</dict>
```
#### 项目配置

Build Setting 配置：

```
TARGETS -> Build Setting -> 搜索 bitcode 并设置为 NO
```

---

### 简单用法

*SDk初始化同 EVPlayer*

#### 初始化播放器
初始化播放器时需要知道一些信息：

* 播放器的尺寸
* 播放VR视频 or VR图片
* 播放的VR视频是否是直播流


```objective-c
EVVRPlayer *vrPlayer = [[EVVRPlayer alloc] initVRPlayerWithFrame:self.view.bounds];
vrPlayer.contentType = EVVRContentTypeVideo;
[self.containerView addSubview:vrPlayer.renderView];
vrPlayer.renderView.backgroundColor = [UIColor clearColor];
vrPlayer.delegate = self;
[vrPlayer playWithUrl:urlString isliving:YES];
```

#### 开始播放
初始化VR播放器时，已经设置了播放器的代理，在代理方法中判断状态，进行播放操作

```objective-c
- (void)evVRPlayer:(EVVRPlayer *)player status:(EVVRPlayerStatus)status {
    switch (status) {
        case EVVRPlayerStatusPrepared: {
            [player play];
            break;
        }
        
        default:
        break;
    }
}
```

#### 切换播放模式
播放VR视频如果需要切换，如：双画面陀螺仪感应、单画面手指拖动，可以通过如下方法设置
*默认：单画面陀螺仪感应*

```objective-c
player.mode = EVVRPlayerModeFingerSingle;
```

#### 停止播放
调用停止播放方法

```objective-c
[player stop];
```

---

### API详细说明

##### 属性

渲染视图

```objective-c
@property (nonatomic, weak, readonly) UIView *renderView;
```

视频时长

```objective-c
@property (nonatomic, readonly) double duration;
```

VR播放器代理

```objective-c
@property (nonatomic, weak) id<EVVRPlayerDelegate> delegate;
```

视频播放地址

```objective-c
@property (nonatomic, readonly, strong) NSURL *playUrl;
```

选择播放全景视频或图片  默认为“视频”

```objective-c
@property (nonatomic, assign) EVVRContentType contentType;
```

视频播放的模式，默认为 EVVRPlayerModeGyroSingle

```objective-c
@property (nonatomic, assign) EVVRPlayerMode mode;
```

是否是直播

```objective-c
@property (nonatomic, assign) BOOL isliving;
```

##### 方法

初始化方法

```objective-c
- (instancetype)initVRPlayerWithFrame:(CGRect)frame;
```

播放录播VR流

```objective-c
- (void)playWithUrl:(NSString *)playUrl;
```

播放VR内容，可选择是否为直播
*VR直播流内部需要获取相对应信息，需要传入YES；VR图片需要传入NO*

```objective-c
- (void)playWithUrl:(NSString *)playUrl isliving:(BOOL)living;
```

播放方法

```objective-c
- (void)play;
```

暂停方法

```objective-c
- (void)pause;
```

停止方法

```objective-c
- (void)stop;
```

关闭播放器

```objective-c
- (void)shutdown;
```

跳到某处开始播放

```objective-c
- (void)seekToTime:(double)time;
```

获取 EVVRPlayer 版本号

```objective-c
+ (NSString *)getVersion;
```

##### 代理

VR播放器代理方法

```objective-c
@protocol EVVRPlayerDelegate <NSObject>
@optional
- (void)evVRPlayer:(EVVRPlayer *)player updateCurrrentTime:(double)time;
- (void)evVRPlayer:(EVVRPlayer *)player status:(EVVRPlayerStatus)status;
- (void)evVRPlayerOnLoading:(EVVRPlayer *)player;
- (void)evVRPlayerOnloaded:(EVVRPlayer *)player;

@end
```

##### 枚举
播放模式枚举
EVVRPlayerMode：

|枚举值|含义|
|:--|:--|
|EVVRPlayerModeDefualt|默认模式，重力感应单画面|
|EVVRPlayerModeGyroSingle|重力感应单画面|
|EVVRPlayerModeGyroDuplicate|重力感应双画面|
|EVVRPlayerModeFingerSingle|手动拖动单画面|
|EVVRPlayerModeFingerDuplicate|手动拖动双画面|

播放状态枚举
EVVRPlayerStatus：

|枚举值|含义|
|:--|:--|
|EVVRPlayerStatusUnknown|位置状态|
|EVVRPlayerStatusPrepared|准备状态|
|EVVRPlayerStatusPlaying|播放中|
|EVVRPlayerStatusBuffering|缓冲中|
|EVVRPlayerStatusPaused|暂停|
|EVVRPlayerStatusStopped|停止|
|EVVRPlayerStatusEnd|结束|
|EVVRPlayerStatusFailed|失败|

播放类型枚举
EVVRContentType：

|枚举值|含义|
|:--|:--|
|EVVRContentTypeVideo|VR视频类型|
|EVVRContentTypePicture|VR图片类型|

