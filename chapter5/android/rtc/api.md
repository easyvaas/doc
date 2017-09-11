## 多人连麦API参考说明 - Android
### 主要方法
#### 初始化 (EVRtc)

```
public EVRtc(Context context, boolean isMaster)
```

该方法创建EVRtc对象

#### 设置连麦视频清晰度（setVideoProfile）

```
public void setVideoProfile(int profile)
```
设置连麦视频清晰度，参数profile指定清晰度，使用者可以根据实际网络状况或使用场景，设置合适的清晰度，清晰度可选列表如下：

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|RTC_RESOLUTION_HD|0|高清: 1280x720 15fps 1000kbps|
|RTC_RESOLUTION_SD|1|标清: 640x360 15fps 400kbps|
|RTC_RESOLUTION_LD|2|流畅: 320x180 15fps 160kbps|

#### 创建并加入连麦房间（createAndJoinChannel）

```
public void createAndJoinChannel(String channel, long uid, boolean isLive, boolean isRecord)
```
创建并加入连麦房间,主播身份调用。参数说明如下：

| 名称 | 描述 |
|:--|:--|
| channel | 连麦房间ID,当传递null或空字符串时,连麦服务后台会自动分配一个唯一的房间ID |
| uid | 连麦用户id。如果传0，后台会自动分配一个用户id |
| isLive | 指定连麦房间是否支持旁路直播 |
| isRecord | 指定连麦房间是否支持回放 |

#### 获取观看h5链接（getShareUrl）
```
public void getShareUrl(String channel)
```
获取到的观看链接，可以分享到微信直接观看。

#### 加入连麦房间（joinChannel）
```
public void joinChannel(String channel, long uid)
```
加入连麦房间,连麦观众调用。参数说明：

| 名称 | 描述 |
|:--|:--|
| channel | 连麦房间ID,不能为null或空字符串 |
| uid | 连麦用户ID,如果该参数传递0,连麦服务后台会自动分配一个用户ID,加入房间成功后返回 |

#### 设置通知回调（setOnInfoListener）
```
public void setOnInfoListener(OnInfoListener listener)
```
设置消息通知回调

#### 离开连麦房间（leaveChannel）
```
public void leaveChannel(String channel)
```

主播或连麦观众主动离开房间都会调用

#### 创建视频渲染窗口（createRendererView）
```
public SurfaceView createRendererView(Context context)
```

#### 设置本地视频渲染窗口（setupLocalView）
```
public void setupLocalView(SurfaceView view, int uid)
```
参数说明：

| 名称 | 描述 |
|:--|:--|
| view | 本地视频渲染窗口 |
| uid | 用户ID,需要与用户系统唯一ID或自动分配的ID保持一致 |

#### 设置远程视频渲染窗口（setupRemoteView）
```
public void setupRemoteView(SurfaceView view, int uid)
```
参数说明：

| 名称 | 描述 |
|:--|:--|
| view | 远程视频渲染窗口 |
| uid | 用户ID,需要与用户系统唯一ID或自动分配的ID保持一致 |

#### 开启或关闭本地视频预览（startPreview）
```
public void startPreview(boolean start, SurfaceView view, int uid)
```
参数说明：

| 名称 | 描述 |
|:--|:--|
| start | 是否开启,true:开启预览,false:关闭预览 |
| view | 预览渲染窗口 |
| uid | 用户ID,需要与用户系统唯一ID或自动分配的ID保持一致 |

#### 前后置摄像头切换（switchCamera）
```
public void switchCamera()
```

#### 静音/取消静音（muteLocalAudioStream）
```
public void muteLocalAudioStream(boolean isMute)
```

#### 暂停/恢复本地视频流（muteLocalVideoStream）
```
public void muteLocalVideoStream(boolean isMute)
```

#### 设置旁路推流画中画布局（configCompositiongLayout）
```
public void configCompositiongLayout(List<EVLayoutRegion> layoutRegions)
```
regions 是一个由 `EVLayoutRegion` 类型对象组成的数组，每个对象的属性如下：

| 名称 | 描述 |
|:--|:--|
| uid | 待显示在该区域的连麦用户uid |
| isMaster | 连麦用户是否为主播 |
| x | 屏幕里该区域的横坐标，取值范围[0.0,1.0] |
| y | 屏幕里该区域的纵坐标，取值范围[0.0,1.0] |
| w | 该区域的实际宽度，取值范围[0.0,1.0] |
| h | 该区域的实际高度，取值范围[0.0,1.0] |


#### 设置连麦事件回调通知处理函数（setRtcEventHandler）
```
public void setRtcEventHandler(EVEventHandler handler)
```
回调详情见下面的回调说明

#### 删除连麦事件回调（removeRtcEventHandler）
```
public void removeRtcEventHandler(EVEventHandler handler)
```
删除连麦事件回调，停止时调用

#### 销毁对象（release）
```
public void release()
```

### 回调事件
多人连麦SDK回调包含连麦鉴权、连麦状态、连麦错误等回调。
#### 事件处理接口（EVEventHandler）

##### 收到远程视频（onFirstRemoteVideoDecoded）
```
void onFirstRemoteVideoDecoded(int uid, int width, int height, int elapsed)
```
##### 加入连麦房间成功（onJoinChannelSuccess）
```
void onJoinChannelSuccess(String channel, int uid, int elapsed)
```

##### 用户离开房间（onUserOffline）
```
void onUserOffline(int uid, int reason)
```

##### 用户开启/关闭了视频显示（onUserMuteVideo）
```
void onUserMuteVideo(int uid, boolean muted)
```

##### 用户开启/关闭了音频传输（onUserMuteAudio）
```
void onUserMuteAudio(int uid, boolean muted)
```

##### 旁路直播的播放地址（onPlayUrl）
```
void onPlayUrl(String url)
```

##### 连麦房间创建成功（onChannelCreate）
```
void onChannelCreate(String channel)
```

##### 对方断开连接（onShareUrl）
```
void onShareUrl(String url)
```

##### 连麦鉴权成功（onAuthSuccess）
```
void onAuthSuccess(String channel, long masterId, long uid)
```

##### 连麦错误回调（onError）
```
void onError(int error, String msg)
```


