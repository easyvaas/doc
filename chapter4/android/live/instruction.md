## **功能详细使用说明**

### 推流参数设置
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

名称 | 数值 | 含义
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

### 水印
推流端SDK提供添加水印功能，水印位置、透明度可自定义。

    public void addWaterMarkLogo(String path, int region, int x, int y, int w, int h);
    
参数说明：

* path，类型：String。logo图片文件的路径（本地sdcard路径、assets路径）
* region，类型：int。logo文件摆放位置：0-左上角，1-右上，2-右下，3-左下
* x，类型：int。logo叠加位置，离最近边的横向距离
* y，类型：int。logo叠加位置，离最近边的纵向距离
* w，类型：int。logo图片的宽度
* h，类型：int。logo图片的高度
* 以上水印的坐标和长宽是基于1280x720的坐标系，实际位置需要根据编码输出的分辨率进行计算

使用示例：

    mEVLive.addWaterMarkLogo("/sdcard/test.png", 1, 10, 10, 160, 68);

***

### 混音
SDK支持背景音乐播放功能，在主播插入耳机时实现了背景音乐与mic的混音功能。
混音功能在默认情况下关闭，也需要申请开通才能使用，否则调用该功能接口无效。

#### 背景音乐播放功能
com.easyvaas.sdk.live.base.audio.EVBgmPlayer类实现了背景音乐播放功能，使用方法：

* 创建播放器

    mEVBgmPlayer = EVBgmPlayer.getInstance();

* 创建播放器回调
    
        mEVBgmPlayer.setOnBgmPlayerListener(new EVBgmPlayer.OnBgmPlayerListener() {
                @Override public void onComplete() {
                    //处理播放完成事件，如果是单曲循环则不被调用
                }

                @Override public void onError(int err) {
                    //处理播放错误事件
                }
            });

* 播放控制接口

        //设置背景音乐音量，同时影响推流端和观看端背景音乐的音量
        public void setVolume(float volume);
        
        //设置背景音乐静音，仅仅影响推流端，观看端混音不受影响
        public void setMute(boolean mute);


#### 混音功能

* 开启背景音乐播放及混音

        mEVLive.setBgmPlayer(mEVBgmPlayer);
        
        //TEST_MP3为本地音乐文件路径，true表示单曲循环
        mEVLive.startBgmPlayer(TEST_MP3, true);
        
        //设置耳机是否插入，上层需要监听耳机插入和拔出事件，参数传入true时才触发混音流程
        mEVLive.setHeadsetPlugged(true);

* 停止背景音乐播放及混音

        mEVLive.stopBgmPlayer();
        
### 美颜
直播端SDK支持美颜功能，在开启直播前和直播过程中可以进行美颜功能开关的设置。
美颜功能在默认情况下关闭，也需要申请开通才能使用，否则调用该功能接口无效。

* 修改预览View

要使用美颜功能，首先将预览View由com.easyvaas.sdk.live.base.view.CameraPreview修改为com.easyvaas.sdk.live.base.view.CameraTextureView，layout文件修改如下：

		<FrameLayout
        	android:layout_width="match_parent"
        	android:layout_height="match_parent"
        	android:background="@android:color/black">
	
        	<com.easyvaas.sdk.live.base.view.CameraTextureView
            	android:id="@+id/txv_preview"
            	android:layout_width="match_parent"
            	android:layout_height="match_parent" />
    	</FrameLayout>

* 设置TextureView.SurfaceTextureListener回调
	
		private TextureView.SurfaceTextureListener mSurfaceTextureListener = new TextureView.SurfaceTextureListener() {
			@Override public void onSurfaceTextureAvailable(SurfaceTexture surface, int width, int height) {
        		if (mEVLive != null) {
            		mEVLive.createPreview(surface, width, height);
        		}
    		}

    		@Override public void onSurfaceTextureSizeChanged(SurfaceTexture surface, int width, int height) {
        		if (mEVLive != null) {
            	mEVLive.updatePreview(width, height);
        	}
    	}

    		@Override public boolean onSurfaceTextureDestroyed(SurfaceTexture surface) {
        		if (mEVLive != null) {
            		mEVLive.destroyPreview();
        		}

        		return true;
    		}
		}
	

* 初始化CameraTextureView

		mCamTextureView = (CameraTextureView)this.findViewById(R.id.txv_preview);
        mCamTextureView.setKeepScreenOn(true);
        mCamTextureView.setSurfaceTextureListener(mSurfaceTextureListener);
        mCamTextureView.requestLayout();

* 设置开始直播时就开启美颜

		builder.setIsBeautyOn(true);
		mEVLive.setParameter(builder.build());

* 直播过程中美颜开关

		mEVLive.switchBeauty(true); //打开美颜
		mEVLive.switchBeauty(false); //关闭美颜

### 连麦
直播端SDK支持连麦功能，在直播过程中特定观众可以发起连麦请求，主播接受连麦请求后，连麦观众和主播之间进行实时互动。目前版本仅支持1对1连麦。

#### 角色定义

开启连麦功能后，参与者角色由之前的主播/观众二者关系转变为主播/辅播/观众三者关系，新增了辅播角色，即发起连麦请求的观众在通过连麦请求后升级为辅播。

#### 主播端实现

* 定义显示辅播视频的View

主播和辅播连麦成功后，需要在主播端显示辅播的视频数据，在原有ui上加入一个FrameLayout用于显示辅播视频。例如在右下角显示辅播视频：

	<FrameLayout
        android:layout_width="120dp"
        android:id="@+id/fl_remote_container"
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:layout_marginBottom="50dp"
        android:layout_height="214dp">
    </FrameLayout>

* 初始化连麦参数

主播接受连麦请求后首先需要初始化连麦参数。

	mEVLive.initInteractiveLiveConfig(this, true); //主播端isAnchor参数为true


* 设置辅播视频窗口

设置辅播窗口

	mFlRemoteViewContainer= (FrameLayout) this.findViewById(R.id.fl_remote_container);
	mEVLive.setRemoteVideoViewContainer(mFlRemoteViewContainer);

* 设置连麦状态回调

设置回调
	
	mEVLive.setOnInteractiveLiveListener(listener);

* 开始连麦

根据上层业务系统获取到唯一的通话id callId，开始连麦

	mEVLive.startInteractiveLive(callId);

* 结束连麦

结束连麦

	mEVLive.endInteractiveLive();

#### 辅播端实现

* 定义显示主播和辅播视频的View

辅播在观看视频时发起连麦请求，请求通过并且连麦成功后，先退出播放窗口，然后将主播的视频数据全屏显示，在小窗口显示本地视频数据。主播全屏显示，本地视频右下角显示的layout如下：

	<FrameLayout
        android:id="@+id/fl_remote_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

    <FrameLayout
        android:id="@+id/fl_local_container"
        android:layout_width="120dp"
        android:layout_height="214dp"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"/>

* 初始化连麦参数

辅播开始连麦首先也需要初始化连麦参数。

	mEVLive.initInteractiveLiveConfig(this, false);  //辅播端isAnchor参数为false

* 设置主播和辅播的视频窗口

主播窗口为远程窗口，辅播窗口为本地窗口

	mEVLive.setRemoteVideoViewContainer((FrameLayout) findViewById(R.id.fl_remote_container));
    mEVLive.setLocalVideoViewContainer((FrameLayout) findViewById(R.id.fl_local_container));

* 设置连麦状态回调

设置回调

	mEVLive.setOnInteractiveLiveListener(listener);

* 开始连麦

开始连麦

	mEVLive.startInteractiveLive(callId);

* 结束连麦

结束连麦

	mEVLive.endInteractiveLive();


