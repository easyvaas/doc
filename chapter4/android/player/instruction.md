## **功能详细使用说明**

### 事件和错误回调

播放端SDK定义事件信息和错误回调包括以下四种

* EVPlayerBase.OnPreparedListener 接收准备完成事件
* EVPlayerBase.OnCompletionListener 接收播放完成事件
* EVPlayerBase.OnInfoListener 接收播放过程中所有的事件
* EVPlayerBase.OnErrorListener 接收播放过程中所有的错误，这里的错误被捕获到后，sdk内部会停止拉流

事件信息状态码如下表：

| 名称 | 数值 | 含义 |
|:--|:--|:--|
| MEDIA_INFO_BUFFERING_START | 701 | 开始缓冲 |
| MEDIA_INFO_BUFFERING_END | 702 | 缓冲完成 |


错误状态码如下表：

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

### 自动重连
Android播放端SDK内部支持自动重连，需要主动调用接口使能自动重连功能。例如：

```
mEVPlayer.setEnableAutoReconnect(true);
```
开启自动重连后，在观看直播或点播过程中，网络发生抖动，或者网络从Wifi切换到4G，播放器SDK内部会自动重连，最大重连次数10次，每次间隔2s。

### 自定义MediaController
Android播放端SDK支持自定义的MediaController，SDK使用者可以根据自己的业务需求定制ui风格和控制逻辑。以下简要说明自定义MediaController的步骤。
#### 控制接口类（MediaPlayerControl）
该接口类定义了视频控制相关的回调
##### 开始播放（start）
```
void start()
```
##### 暂停播放（pause）
```
void pause()
```
##### 获取视频时长（getDuration）
```
int getDuration()
```
##### 获取当前播放位置（getCurrentPosition）
```
int getCurrentPosition()
```
##### 播放定位（seekTo）
```
void seekTo(long pos)
```
##### 是否正在播放（isPlaying）
```
boolean isPlaying()
```
##### 获取缓冲进度（getBufferPercentage）
```
int getBufferPercentage()
```
##### 是否支持暂停（canPause）
```
boolean canPause();
```

#### 自定义MediaController类（MediaController）
* MediaController类继承FrameLayout，可根据自定义的layout文件实现用户自定义的播放器控制ui
* MediaController类提供一系列的播放控件，可浮动显示在视频窗口上面。MediaController类提供setAnchorView()接口完成此功能。

#### 绑定播放器View
实际使用过程中，在播放页面Activity中将MediaController和VideoView进行关联。例如：

```
mMediaController = new MediaController(this);
mMediaController.setMediaPlayer(mediaControl);
mMediaController.setAnchorView(mVideoView);
```


