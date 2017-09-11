### 示例代码
#### 创建全局Application
创建全局Application文件，如果目标工程全局Application文件已经存在，则在该文件合适的地方使用EVRtcWorker.initWorker()和EVSdk.init()方法来进行初始化，将上面获取到的APPID，Access Key，Secret Key作为该方法的参数，该方法调用的位置应该是：目标工程用户登录成功后调用全局Application设置用户信息的函数，将用户信息作为EVSdk.init的参数。如果初期目标工程没有用户系统，在全局Application的onCreate()函数中调用EVSdk.init()，用户信息的参数传空字符串即可。如下：

        @Override public void onCreate() {
            super.onCreate();

            app = this;

            //打开SDK详细日志开关
            EVSdk.enableDebugLog();
            
            //初始化SDK，传入APPID，Access Key，Secret Key，userdata等参数
            EVSdk.init(app.getApplicationContext(), "APPID", "Access Key", "Secret Key", "userdata");
            
            //初始化连麦工作线程
            EVRtcWorker.initWorker(getApplicationContext(), Constant.INTERACTIVE_LIVE_APP_ID);
        }

#### 创建连麦主界面
可参考demo工程中的com.easyvaas.sdk.demo.PlayerActivity类完成一个简单的连麦示例

* 在AndroidManifest.xml文件中注册Activity，固定为横屏模式

        <activity
            android:name=".PlayerActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:label="@string/title_activity_player"
            android:screenOrientation="landscape"
            android:theme="@style/AppTheme.FullScreen"
            android:windowSoftInputMode="stateHidden|adjustResize" />
            
* 在布局文件中加入本地大窗口的View

    ```
    <com.easyvaas.sdk.ilivedemo.GridVideoViewContainer
        android:id="@+id/grid_video_view_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
    ```            

* 在布局文件中加入连麦小窗口列表

    ```
    <ViewStub
        android:id="@id/small_video_view_dock"
        android:inflatedId="@id/small_video_view_dock"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout="@layout/small_video_view_dock"
        android:layout_gravity="right"/>
    ```

* 初始化大窗口View

        mGridVideoViewContainer = (GridVideoViewContainer) findViewById(R.id.grid_video_view_container);
        
* 远程连麦窗口列表初始化

    ```
    if (mSmallVideoViewDock == null) {
        ViewStub stub = (ViewStub) findViewById(R.id.small_video_view_dock);
        mSmallVideoViewDock = (RelativeLayout) stub.inflate();
    }
    
    RecyclerView recycler = (RecyclerView) findViewById(R.id.small_video_view_container);
    ```

* 创建连麦状态回调

连麦状态回调接口如下：

```
    @Override
    public void onFirstRemoteVideoDecoded(int uid, int width, int height, int elapsed) {
        //收到远程视频第一帧数据，可以开始渲染连麦小窗口了
        renderRemoteView(uid);
    }

    @Override
    public void onJoinChannelSuccess(String channel, int uid, int elapsed) {
        //加入连麦房间成功
        
    }

    @Override
    public void onUserOffline(int uid, int reason) {
        //有用户离开，处理界面变化
    }

    @Override
    public void onUserMuteVideo(int uid, boolean muted) {
        //有用户开启/关闭了视频显示
    }

    @Override
    public void onUserMuteAudio(int uid, boolean muted) {
        //有用户开启/关闭了音频传输
    }

    @Override
    public void onPlayUrl(String url) {
        //获取到旁路直播的播放地址
    }

    @Override
    public void onChannelCreate(String channel) {
        //互动房间创建成功
        mChannelID = channel;
    }

    @Override
    public void onShareUrl(String url) {
        //获取到直播观看h5地址
        mShareUrl = url;
    }

    @Override
    public void onAuthSuccess(String channel, long masterId, long uid) {
        //连麦鉴权成功,开启本地大窗口视频预览
    }

    @Override
    public void onError(int error, String msg) {
        //连麦错误信息回调
        showErrorDialog(error, msg);
    }
```

* 初始化连麦参数

    ```
    mEVRtc = new EVRtc(this, mRole == RtcConstants.RTC_ROLE_MASTER);
    mEVRtc.setVideoProfile(mRtcOption.getVideoResolution());
    mEVRtc.setRtcEventHandler(this);
    ```
    
* 主播创建并加入房间

主播调用mEVRtc.createAndJoinChannel()函数创建并进入连麦房间。如下：

        mEVRtc.createAndJoinChannel(channel, uid, true, true);

* 连麦观众加入房间
连麦观众调用mEVRtc.joinChannel()函数加入连麦房间。如下：

        mEVRtc.joinChannel(channel, uid);

* 设置本地预览窗口

        mEVRtc.setupLocalView(localView, uid);

* 设置远程视频预览窗口

        mEVRtc.setupRemoteView(surfaceV, uid);

* 开启本地预览

        mEVRtc.startPreview(true, localView, uid);

* 结束连麦

        mEVRtc.leaveChannel(mChannelID);;

* 释放资源

        mEVRtc.release();


