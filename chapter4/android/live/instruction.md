## **功能详细使用说明**

### 推流参数设置/Users/liya/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/b152f13cf0ec7da7f34f374fb06fd42a/Message/MessageTemp/9e20f478899dc29eb19741386f9343c8/File/杨林工作初步安排.md

* 视频分辨率
目前支持360p（360x640），540p（540x960），720p（720x1280）三种分辨率，分别对应预定义的常量：
    * LiveConstants.VIDEO_RESOLUTION_360P
    * LiveConstants.VIDEO_RESOLUTION_540P
    * LiveConstants.VIDEO_RESOLUTION_720P

视频分辨率默认值为LiveConstants.VIDEO_RESOLUTION_360P
    
* 视频码率
EVLive SDK支持设置视频编码码率的初始值，单位kbps，默认值500。SDK支持根据网络实际情况进行码率自适应，即在网络较差时下调视频码率以保证直播的流畅性，网络变好时上调视频码率以保证直播的清晰度。
* 音频码率
EVLive SDK音频采样率内部固定为44100Hz，支持16kbps，32kbps，48kbps三种，分别对应预定义的常量：
	* LiveConstants.AUDIO_BITRATE_16K
	* LiveConstants.AUDIO_BITRATE_32K
	* LiveConstants.AUDIO_BITRATE_48K

默认值为LiveConstants.AUDIO_BITRATE_32K

* 音频编码方式
EVLive SDK支持AAC编码等级的选择，目前支持AACObject_LC和AACObject_HE两种，分别对应预定义的常量：
    * LiveConstants.AUDIO_CODEC_AAC_LC
    * LiveConstants.AUDIO_CODEC_AAC_HE

默认值为LiveConstants.AUDIO_CODEC_AAC_LC，选择LiveConstants.AUDIO_CODEC_AAC_HE可产生高质量的AAC音频。

####参数设置列表

| 方法 | 功能 | 推荐值 |
|:--|:--|:--|
| setVideoResolution | 设置视频编码分辨率 | VIDEO_RESOLUTION_360P |
| setMaxVideoBitrate | 设置视频最大编码码率 | 800kbps |
| setAudioBitrate | 设置音频编码码率 | 32kbps |
| setAudioCodec | 设置AAC音频编码等级 | AUDIO_CODEC_AAC_LC |
| setUseFrontCamera | 设置是否使用前置摄像头开始预览 | true |

***

### 事件和错误回调

直播端SDK定义事件信息和错误两种回调

* OnInfoListener 接收直播过程中所有的事件
* OnErrorListener 接收直播过程中所有的错误，这里的错误被捕获到后，sdk内部会停止推流

事件信息状态码如下表：

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


错误状态码如下表：

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

***

### 横屏直播
推流端SDK支持横屏推流，除了上层需要做一些处理外，需要调用推流端的相关接口通知底层。

上层ui相关代码：

```
setRequestedOrientation(mIsLandscape
                ? ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE
                : ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
```

调用接口通知SDK进行采集画面的旋转：

```
mEVLive.setParameter(builder.build());
mEVLive.setDisplayOrientation(mIsLandscape ? 90 : 0);
```

***注意事项：*** setDisplayOrientation接口必须在setParameter接口之后调用

### 水印
推流端SDK提供添加水印功能，水印位置、透明度可自定义。

    public void addWaterMarkLogo(String path, float x, float y, float w, float h, float alpha);
    
参数说明：

* path，类型：String。logo图片文件的路径（本地sdcard路径、assets路径）
* x，类型：float。水印图片左上角顶点x坐标，取值0-1之间浮点数，相对于视频左侧实际距离=预览width*x
* y，类型：float。水印图片左上角顶点y坐标，取值0-1之间浮点数，相对于视频顶部实际距离=预览height*y
* w，类型：float。水印的显示宽度，0-1之间，实际宽度=预览width*w，为0时会根据h及logo图片的比例自适应
* h，类型：float。水印的显示高度，0-1之间，实际宽度=预览width*w，为0时会根据w及logo图片的比例自适应
* alpha，类型：float。水印的透明度，0-1之间

使用示例：

    mEVLive.addWaterMarkLogo(TEST_WATERMARK_PATH, 0.08F, 0.06F, 0.2F, 0F, 0.8F);

***

### 混音

SDK支持背景音乐播放功能，在主播插入耳机时实现了背景音乐与mic的混音功能。打开和关闭混音功能的接口：

    public void setEnableAudioMix(boolean enable);

### 背景音乐播放
调用背景音乐相关接口实现BGM播放和控制：

* 初始化BGM播放器

        mEVLive.setBgmOnCompletionListener(new IMediaPlayer.OnCompletionListener() {
            @Override
            public void onCompletion(IMediaPlayer iMediaPlayer) {
                Logger.d(TAG, "end of bgm");
            }
        });
        mEVLive.setBgmOnErrorListener(new IMediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(IMediaPlayer iMediaPlayer, int what, int extra) {
                Logger.e(TAG, "bgm play error: " + what + ", extra: " + extra);
                return false;
            }
        });
        mEVLive.setBgmVolume(0.5F);
        mEVLive.setBgmMute(false);
        mEVLive.setEnableAudioMix(true);

* 打开BGM播放器
    
        mEVLive.startBgmPlayer(mBgmFilePath, true);

* 关闭BGM播放器

        mEVLive.stopBgmPlayer();
        
### 美颜
直播端SDK支持美颜功能，在开启直播前和直播过程中可以进行美颜功能开关的设置。

* 设置开始直播时就开启美颜

		builder.setIsBeautyOn(true);
		mEVLive.setParameter(builder.build());

* 直播过程中美颜开关

		mEVLive.switchBeauty(true); //打开美颜
		mEVLive.switchBeauty(false); //关闭美颜

### 连麦
直播端SDK支持连麦功能，在直播过程中特定观众可以发起连麦请求，主播接受连麦请求后，连麦观众和主播之间进行实时互动。目前版本仅支持1对1连麦。
***重要提示：***要测试或使用连麦功能，需要向易视云商务人员申请连麦id，否则点击界面上的连麦按钮会提示“id为空，无法进入连麦”

* 初始化连麦参数

    使用连麦功能之前，需要初始化连麦参数，传入有效的连麦id，一般在界面的OnCreate函数中调用：

    ```
    mEVLive.initInteractiveLiveConfig(id, true); //如果是主播端isAnchor参数为true
    ```

* 设置连麦小窗口位置

    ```
    mEVLive.setRTCSubScreenRect(0.65F, 0.1F, 0.35F, 0.3F);
    ```
	
	参数说明：
    * width  float 0-1之间，实际宽度=预览width*width，默认值0.35f
    * height float 0-1之间，实际高度=预览height*height，默认值0.3f
    * left   float 左上角顶点x坐标，0-1之间，相对于视频左侧实际距离=预览width*left，默认值0.65f
    * top    float 左上角顶点y坐标，0-1之间，相对于视频顶部实际距离=预览height*top，默认值0.f

* 设置辅播大窗口

    SDK内部默认情况下辅播为大窗口，即连麦成功后大窗口总是显示对方的画面
    
    ```
    mEVLive.setEnableMainScreenRemote(false);  //enable参数赋值false，设置对方为小窗口，自己的camera预览为大窗口
    ```

* 设置连麦状态回调

    ```
    mEVLive.setOnInteractiveLiveListener(listener);
    ```

* 开始连麦

    根据上层业务系统获取到唯一的通话id callId，开始连麦
    
    ```
    mEVLive.startInteractiveLive(callId);
    ```

* 结束连麦

    结束连麦

    ```
    mEVLive.endInteractiveLive();
    ```


