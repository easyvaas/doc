### 示例代码
#### 创建全局Application
创建全局Application文件，如果目标工程全局Application文件已经存在，则在该文件合适的地方使用EVSdk.init()方法来进行初始化，将上面获取到的APPID，Access Key，Secret Key作为该方法的参数，该方法调用的位置应该是：目标工程用户登录成功后调用全局Application设置用户信息的函数，将用户信息作为EVSdk.init的参数。如果初期目标工程没有用户系统，在全局Application的onCreate()函数中调用EVSdk.init()，用户信息的参数传空字符串即可。如下：

        @Override public void onCreate() {
            super.onCreate();

            app = this;

            //打开SDK详细日志开关
            EVSdk.enableDebugLog();
            
            //初始化SDK，传入APPID，Access Key，Secret Key，userdata等参数
            EVSdk.init(app.getApplicationContext(), "APPID", "Access Key", "Secret Key", "userdata");
        }

#### 创建推流主界面
可参考demo工程中的com.easyvaas.sdk.demo.RecorderActivity类完成一个简单的推流示例

* 在AndroidManifest.xml文件中注册Activity，固定为竖屏、全屏直播

        <activity
            android:name=".RecorderActivity"
            android:label="@string/title_activity_recorder"
            android:screenOrientation="portrait"
            android:theme="@style/AppTheme.FullScreen" >
        </activity>

* 在布局文件中加入预览View

    ```
<android.opengl.GLSurfaceView
        android:id="@+id/camera_preview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_alignParentBottom="true"
        android:layout_alignParentTop="true" />
    ```

* 初始化CameraPreview

        mCameraPreviewView = (GLSurfaceView) this.findViewById(R.id.camera_preview);

* 推流参数设置

推流参数EVStreamerParameter使用了Builder模式，推流过程中不可动态修改或一些初始的参数通过EVStreamerParameter来指定。如下：

        EVStreamerParameter.Builder builder = new EVStreamerParameter.Builder();
        builder.setVideoResolution(LiveConstants.VIDEO_RESOLUTION_360P)
                .setInitVideoBitrate(500)
                .setAudioBitrate(32)
                .setAudioCodec(LiveConstants.AUDIO_CODEC_AAC_LC)
                .setUseFrontCamera(true)
                .setIsBeautyOn(false);

* 创建推流事件、错误回调

推流过程中出现错误，需要在错误回调中捕获相关错误信息，给予用户友好的提示。同时一些重要的事件信息也需要进行合适的处理，例如捕获到StreamerConstants.EV_STREAMER_INFO_STREAM_SUCCESS的事件，表示底层推流模块真正开始工作，可以进行一些界面元素更新的一些操作。如下：

        private OnInfoListener mInfoListener = new OnInfoListener() {
            @Override
            public boolean onInfo(int what) {
                switch (what) {
                    ...
                    case StreamerConstants.EV_STREAMER_INFO_STREAM_SUCCESS:
                        //更新界面显示，如开始计时
                        break;
                }
                return true;
            }
        };
        
        private OnErrorListener mErrorListener = new OnErrorListener() {
            @Override
            public boolean onError(int what) {
                switch (what) {
                ...
                }
            }
        };

* 创建EVLive对象

        mEVLive = new EVLive(this);
        mEVLive.setParameter(builder.build());
        mEVLive.setCameraPreview(mCameraPreview);
        mEVLive.setOnErrorListener(mErrorListener);
        mEVLive.setOnInfoListener(mInfoListener);
        
* 摄像头预览

开始推流前需要打开摄像头预览，应用进入后台需要关闭摄像头预览，应用再次回到前台重新打开摄像头预览：
    
        mEVLive.startCameraPreview();
    
* 开始推流

调用mEVLive.startStreaming()函数开始推流，该函数需要传入两个参数：lid和key，这两个参数由业务层提供。如下：

        mEVLive.startStreaming(lid， key);

* 推流过程中常用方法

        //前后摄像头切换
        mEVLive.switchCamera();
        
        //闪光灯切换
        mEVLive.switchFlashlight();
        
        //静音切换
        mEVLive.switchAudioMute();

* 停止推流

        mEVLive.stopLive();

* Activity的生命周期回调处理。推流过程中，需要处理来电、误触、切后台等情况，**这些状态都依赖于Activity的生命周期，所以必须在Activity的生命周期中也调用EVLive SDK相应的接口**，以确保推流正常结束或恢复。如下：

        @Override
        protected void onPause() {
            super.onPause();
            if (null != mEVLive) {
                mEVLive.onPause();
            }
        }
        
        @Override
        protected void onResume() {
            super.onResume();
            if (null != mEVLive) {
                mEVLive.onResume();
            }
        }
        
        @Override
        protected void onDestroy() {
            super.onDestroy();
            if (null != mEVLive) {
                mEVLive.onDestroy();
            }
        }


