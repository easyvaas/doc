## 播放端API参考说明 - Android
### 主要方法
#### 初始化 (EVPlayer)

```
public EVPlayer(Context context)
```

该方法创建EVPlayer对象

#### 设置参数（setParameter）

```
public void setParameter(EVPlayerParameter parameter)
```
设置播放参数。目前播放参数只有一个live，表示播放的视频是否为直播。

#### 设置播放窗口（setVideoView）

```
public void setVideoView(EVVideoView videoview)
```
设置播放窗口

#### 设置是否自动重连（setEnableAutoReconnect）
```
public void setEnableAutoReconnect(boolean enable)
```
设置是否自动重连。SDK内部默认不进行自动重连，enable=true，网络发生错误时自动重连，enable=false，网络发生错误时不自动重连。

#### 设置prepared回调（setOnPreparedListener）
```
public void setOnPreparedListener(EVPlayerBase.OnPreparedListener l)
```
设置prepared消息回调

#### 设置播放完成回调（setOnCompletionListener）
```
public void setOnCompletionListener(EVPlayerBase.OnCompletionListener l)
```
设置播放完成回调

#### 设置播放状态回调（setOnInfoListener）
```
public void setOnInfoListener(EVPlayerBase.OnInfoListener l)
```

设置播放状态回调

#### 设置错误回调（setOnErrorListener）
```
public void setOnErrorListener(EVPlayerBase.OnErrorListener l)
```
设置错误回调

#### 开始播放（watchStart）
```
public void watchStart(String lid, boolean living)
```
开始播放，参数说明：

| 名称 | 描述 |
|:--|:--|
| lid | 播放视频唯一id |
| living | 播放的视频是否为直播 |

#### 生命周期函数
```
public void onCreate()
public void onStart()
public void onResume()
public void onPause()
public void onStop()
public void onDestroy()
```
播放端SDK提供Android界面对应的同名生命周期函数，在播放界面的生命周期函数内应该对应调用相关的同名函数，进行相关资源的申请和释放。

### 回调事件
播放端SDK回调事件。
#### prepared事件（EVPlayerBase.OnPreparedListener）

##### prepared（onPrepared）
```
boolean onPrepared()
```
收到该事件回调说明已经请求到网络数据，可以开始播放了

#### 播放完成事件（EVPlayerBase.OnCompletionListener）
##### 播放完成（onCompletion）
```
boolean onCompletion()
```
收到该事件回调说明视频播放结束，在播放点播时收到该事件表示正常结束，可以退出播放界面。

#### 播放状态事件（EVPlayerBase.OnInfoListener）
##### 播放状态（onInfo）
```
boolean onInfo(int what, int extra)
```
播放状态回调，包括以下状态码：

| 名称 | 数值 | 含义 |
|:--|:--|:--|
| MEDIA_INFO_BUFFERING_START | 701 | 开始缓冲 |
| MEDIA_INFO_BUFFERING_END | 702 | 缓冲完成 |
| MEDIA_INFO_VIDEO_RENDERING_START | 3 | 视频开始渲染，可用于上层计算首屏打开时间 |

#### 播放错误事件（EVPlayerBase.OnErrorListener）
##### 播放错误（onError）
```
boolean onError(int what, int extra)
```
播放错误回调，包含以下错误码：

| 名称 | 数值 | 含义 |
|:----------|:--|:--|
| MEDIA_ERROR_UNKNOWN | 1 | 位置错误|
|MEDIA_ERROR_SERVER_DIED | 100 | 播放服务器错误|
|MEDIA_ERROR_IO | -1004 | 网络IO错误|
|MEDIA_ERROR_MALFORMED | -1007 | 不支持的格式|
|MEDIA_ERROR_UNSUPPORTED | -1010 | 播放资源无法识别|
|MEDIA_ERROR_TIMED_OUT | -110 | 连接超时|
|EV_PLAYER_ERROR_SDK_INIT | -2001 | sdk未初始化或初始化失败|
|EV_PLAYER_ERROR_GET_RESOURCE | -3001 | 播放资源调度失败|
|EV_PLAYER_ERROR_GET_URL | -3002 | 获取播放地址失败|
|EV_PLAYER_ERROR_PARAMETER | -3003 | 播放参数错误|
|EV_PLAYER_ERROR_NOT_ACCEPTABLE | -3004 | 不支持的播放格式|
|EV_PLAYER_ERROR_NONE_STREAM | -3005 | 播放的视频不存在|
|EV_PLAYER_ERROR_LIVE_EXCEPTION | -3006 | 请求的直播还未开始或已经结束|
|EV_PLAYER_ERROR_RECORD_EXCEPTION | -3007 | 请求的录播文件还未生成|
|EV_PLAYER_ERROR_FILE_EXCEPTION | -3008 | 录播文件出错,播放失败|


