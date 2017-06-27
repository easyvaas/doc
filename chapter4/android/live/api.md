## 推流端API参考说明 - Android
### 主要方法
#### 初始化 (EVLive)

```
public EVLive(Context context)
```

该方法创建EVLive对象

#### 设置参数（setParameter）

```
public void setParameter(EVStreamerParameter parameter)
```
设置推流参数。其中音视频编码码率、视频编码分辨率、横竖屏推流、音频编码方式等参数设置后在推流过程中不可更改，而前置摄像头、美颜开关等参数在推流过程中可以更改。

#### 设置摄像头预览View（setCameraPreview）

```
public void setCameraPreview(GLSurfaceView preview)
```
设置摄像头预览窗口

#### 设置摄像头显示方向（setDisplayOrientation）
```
public void setDisplayOrientation(int orientation)
```
设置摄像头显示方向，在横竖屏直播设置时使用，横屏直播时orientation设置为90，竖屏直播时orientation设置为0。

#### 设置错误回调（setOnErrorListener）
```
public void setOnErrorListener(OnErrorListener listener)
```
设置错误消息回调

#### 设置通知回调（setOnInfoListener）
```
public void setOnInfoListener(OnInfoListener listener)
```
设置消息通知回调

#### 开始预览（startCameraPreview）
```
public void startCameraPreview()
```

打开摄像头预览，该函数必须显式调用

#### 静音开关（switchAudioMute）
```
public void switchAudioMute()
```
直播前或直播中可调用，用于打开关闭静音功能。

#### 前后摄像头切换（switchCamera）
```
public void switchCamera()
```
前后摄像头动态切换，直播前或直播中都可调用

#### 闪光灯开关（switchFlashlight）
```
public void switchFlashlight()
```
闪光灯开关，直播前或直播中都可调用

#### 美颜开关（switchBeauty）
```
public void switchBeauty(boolean isBeautyOn)
```
美颜开关，isBeautyOn=true，美颜功能打开，isBeautyOn=false，美颜功能关闭

#### 设置美颜级别（setBeautyFilter）
```
public void setBeautyFilter(int level)
```
设置美颜级别，通过调用该接口设置不同美颜级别，目前支持1-4级，算法复杂度和美颜效果随级别依次提高。

#### 添加水印（addWaterMarkLogo）
```
public void addWaterMarkLogo(String path, float x, float y, float w, float h, float alpha)
```
添加水印，参数说明如下：

| 名称 | 描述 |
|:--|:--|
| path | 水印图片文件的路径 |
| x | 水印图片左上角顶点x坐标，取值0-1之间浮点数，相对于视频左侧实际距离=预览width*x |
| y | 水印图片左上角顶点y坐标，取值0-1之间浮点数，相对于视频顶部实际距离=预览height*y |
| w | 水印的显示宽度，0-1之间，实际宽度=预览width*w，为0时会根据h及logo图片的比例自适应 |
| h | 水印的显示高度，0-1之间，实际宽度=预览height*h，为0时会根据w及logo图片的比例自适应 |
| alpha | 水印的透明度，0-1之间 |

#### 开始推流（startStreaming）
```
public void startStreaming(String lid, String key)
```
开始推流，该方法有两种调用方式：

* 直播准备完成，点击直播按钮触发调用
* 打开预览后自动开始直播，在收到LiveConstants.EV_LIVE_INFO_CAMERA_INIT_DONE回调通知后调用，自动开始推流

参数说明：

| 名称 | 描述 |
|:--|:--|
| lid | 推流唯一id，通过业务后台获取或者调用genstream接口获取 |
| key | 流id对应的推流鉴权key，key错误会导致推流失败 |

#### 停止推流（stopLive）
```
public void stopLive()
```
停止推流

#### 混音功能开关（setEnableAudioMix）
```
public void setEnableAudioMix(boolean enable)
```
开启和关闭混音功能,打开该功能解决主播端插上耳机后，背景音乐无法采集的问题

#### 设置背景音乐播放完成回调（setBgmOnCompletionListener）
```
public void setBgmOnCompletionListener(IMediaPlayer.OnCompletionListener listener)
```
设置背景音乐播放完成的回调通知

#### 设置背景音乐播放错误回调（setBgmOnErrorListener）
```
public void setBgmOnErrorListener(IMediaPlayer.OnErrorListener listener)
```
设置背景音乐播放错误回调

#### 设置背景音乐播放音量（setBgmVolume）
```
public void setBgmVolume(float volume)
```
设置背景音乐播放音量，参数说明：

| 名称 | 描述 |
|:--|:--|
| volume | 0-1之间的浮点数 |

#### 背景音乐静音开关（setBgmMute）
```
public void setBgmMute(boolean mute)
```
背景音乐静音开关

#### 开始背景音乐播放（startBgmPlayer）
```
public void startBgmPlayer(String path, boolean loop)
```
开始播放背景音乐，参数说明：

| 名称 | 描述 |
|:--|:--|
| path | 背景音乐文件路径 |
| loop | 是否重复播放 |

#### 暂停播放背景音乐（pauseBgmPlayer）
```
public void pauseBgmPlayer()
```
暂停播放背景音乐

#### 恢复播放背景音乐（resumeBgmPlayer）
```
public void resumeBgmPlayer()
```
恢复播放背景音乐

#### 关闭背景音乐（stopBgmPlayer）
```
public void stopBgmPlayer()
```
关闭背景音乐播放

#### 背景音乐播放定位（seekToBgm）
```
public void seekToBgm(long position)
```
seek定位,用于上层开发背景音乐播发器控制功能

#### 获取背景音乐时长（getBgmDuration）
```
public long getBgmDuration()
```
获取背景音乐的时长

#### 获取背景音乐当前播放进度（getBgmCurrentPosition）
```
public long getBgmCurrentPosition()
```
获取背景音乐当前播放进度，单位毫秒

#### 初始化连麦参数（initInteractiveLiveConfig）
```
public void initInteractiveLiveConfig(String appid, boolean isAnchor)
```
初始化连麦参数。开通连麦功能需要申请连麦appid，调用该函数前确保appid有效性。该函数建议在推流界面初始化函数OnCreate函数中调用。
参数说明：

| 名称 | 描述 |
|:--|:--|
| appid | 连麦需要的appid |
| isAnchor | 是否主播角色 |

#### 开始连麦（startInteractiveLive）
```
public void startInteractiveLive(String channelId)
```
开始连麦。参数说明：

| 名称 | 描述 |
|:--|:--|
| channelId | 连麦房间id，由业务后台提供 |

#### 设置连麦小窗口位置（setRTCSubScreenRect）
```
public void setRTCSubScreenRect(float left, float top, float width, float height)
```
设置连麦小窗口显示的位置，参数说明：

| 名称 | 描述 |
|:--|:--|
| left | 左上角顶点x坐标，取值0-1之间浮点数，相对于视频左侧实际距离=预览width*left |
| top | 左上角顶点y坐标，取值0-1之间浮点数，相对于视频顶部实际距离=预览height*top |
| width | 显示宽度，0-1之间，实际宽度=预览宽度*width |
| height | 显示高度，0-1之间，实际宽度=预览高度*height |

#### 设置大窗口显示内容（setEnableMainScreenRemote）
```
public void setEnableMainScreenRemote(boolean enable)
```
设置大窗口是否显示连麦对方视频数据，enable=true，大窗口显示对方视频数据，enable=false，大窗口显示自己的视频数据

#### 结束连麦（endInteractiveLive）
```
public void endInteractiveLive()
```
结束连麦

#### 设置连麦状态回调（setOnInteractiveLiveListener）
```
public void setOnInteractiveLiveListener(OnInteractiveLiveListener listener)
```
设置连麦状态回调

#### 生命周期函数
```
public void onCreate()
public void onResume()
public void onPause()
public void onDestroy()
```
推流端SDK提供Android界面对应的同名生命周期函数，在推流界面的生命周期函数内应该对应调用相关的同名函数，进行相关资源的申请和释放，调用onPause()和onResume()函数处理应用进入后台再回到前台自动恢复推流的逻辑。

### 回调事件
推流端SDK回调包含直播状态、直播错误、背景音乐播放完成、背景音乐播放错误、连麦状态等回调。
#### 直播状态事件（OnInfoListener）

##### 直播状态（onInfo）
```
boolean onInfo(int what)
```
what表示事件信息状态码，如下表：

|名称 | 数值 | 含义 |
|:--|:--|:--|
|EV_LIVE_INFO_STREAMING | 101 | 正在直播中|
|EV_LIVE_INFO_RECONNECTING | 102 | 网络正在重连中|
|EV_LIVE_INFO_RECONNECTED | 103 | 网络重连成功|
|EV_LIVE_INFO_START_SUCCESS | 104 | 开始直播成功|
|EV_LIVE_INFO_STOP_SUCCESS | 105 | 停止直播成功|
|EV_LIVE_INFO_STREAM_SUCCESS | 106 | 流上传成功，真正开始推流|
|EV_LIVE_INFO_NETWORK_ISSUE | 107 | 网络变差，码率开始降低|
|EV_LIVE_INFO_NETWORK_BAD | 108 | 网络卡顿，开始丢包|

#### 直播错误事件（OnErrorListener）
##### 直播错误（onError）
```
boolean onError(int what)
```

what表示错误状态码，如下表：

|名称 | 数值 | 含义 |
|:--|:--|:--|
|EV_LIVE_ERROR_STARTING | -1001 | 直播开始过程中出错|
|EV_LIVE_ERROR_STREAMING | -1002 | 直播过程中出错|
|EV_LIVE_ERROR_RECONNECT | -1003 | 网络重连失败|
|EV_LIVE_ERROR_VERSION_LOW | -1004 | Android系统版本过低，暂不支持直播功能|
|EV_LIVE_ERROR_OPEN_CAMERA | -1005 | 打开摄像头出错|
|EV_LIVE_ERROR_CREATE_AUDIORECORD | -1006 | 打开音频设备出错|
|EV_LIVE_ERROR_VIDEO_ALREADY_STOPPED | -1007 | 直播已经停止|
|EV_LIVE_ERROR_NETWORK_DIE | -1008 | 网络太差导致无法直播|
|EV_LIVE_ERROR_AUDIO_ENCODER | -1009 | 音频编码出错|
|EV_LIVE_ERROR_VIDEO_ENCODER | -1010 | 视频编码出错|
|EV_LIVE_ERROR_NETWORK | -1011 | 网络发送错误|
|EV_LIVE_ERROR_SDK_INIT | -2001 | sdk未初始化或初始化失败|
|EV_LIVE_PUSH_LOCATE_ERROR | -3001 | 推流资源调度失败|
|EV_LIVE_PUSH_REDIRECT_ERROR | -3002 | 获取推流地址失败|
|EV_LIVE_PUSH_PARAMETER_ERROR | -3003 | 推流参数错误 |
|EV_LIVE_PUSH_KEY_ERROR | -3004 | 推流请求key错误 |

#### 背景音乐播放完成事件（IMediaPlayer.OnCompletionListener）
##### 播放完成（onCompletion）
```
void onCompletion(IMediaPlayer iMediaPlayer)
```
背景音乐播放完成

#### 背景音乐播放错误事件（IMediaPlayer.OnErrorListener）
##### 播放错误（onError）
```
boolean onError(IMediaPlayer iMediaPlayer, int what, int extra)
```

背景音乐播放错误回调
#### 连麦事件（OnInteractiveLiveListener）
##### 连麦是否成功（onJoinChannelResult）
```
void onJoinChannelResult(boolean isSuccess)
```

连麦是否成功回调。

| 名称 | 描述 |
|:--|:--|
| isSuccess | true，连麦成功；false，连麦失败 |
##### 退出连麦房间（onLeaveChannelSuccess）
```
void onLeaveChannelSuccess()
```

连麦用户退出会回调
##### 连麦开始（onFirstRemoteVideoDecoded）
```
void onFirstRemoteVideoDecoded()
```

收到连麦对方的第一帧数据，可以认为连麦真正开始了
##### 对方断开连接（onUserOffline）
```
void onUserOffline(int userId,int reason)
```

对方断开连麦，参数说明：

| 名称 | 描述 |
|:--|:--|
| userId | 连麦用户id |
| reason | 断开原因 |

##### 连麦发生错误（onError）
```
void onError(int code,String message)
```

对方断开连麦，参数说明：

| 名称 | 描述 |
|:--|:--|
| code | 错误码 |
| message | 错误描述 |

