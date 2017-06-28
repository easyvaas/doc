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

#### 创建播放主界面
可参考demo工程中的com.easyvaas.sdk.demo.PlayerActivity类完成一个简单的播放示例

* 在AndroidManifest.xml文件中注册Activity，固定为竖屏、全屏播放

        <activity
            android:name=".PlayerActivity"
            android:configChanges="orientation|keyboardHidden"
            android:label="@string/title_activity_player"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"
            android:windowSoftInputMode="stateHidden|adjustResize" >
        </activity>

* 在布局文件中加入播放显示View

        <com.easyvaas.sdk.player.base.EVVideoView
            android:id="@+id/player_surface_view"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center"/>

* 初始化EVVideoView

        mVideoView = (EVVideoView) findViewById(R.id.player_surface_view);

* 播放参数设置
播放参数EVPlayerParameter使用了Builder模式，这里只设置当前播放的流是否为直播流。如下：

        EVPlayerParameter.Builder builder = new EVPlayerParameter.Builder();
        builder.setLive(true);

* 创建播放事件、错误回调
播放过程中出现错误，需要在错误回调中捕获相关错误信息，给予用户友好的提示。同时一些重要的事件信息也需要进行合适的处理。如下：

        private EVPlayerBase.OnPreparedListener mOnPreparedListener = new EVPlayerBase.OnPreparedListener() {
            @Override public boolean onPrepared() {
                //播放准备完成，开始播放
                return true;
            }
        };
        
        private EVPlayerBase.OnCompletionListener mOnCompletionListener = new EVPlayerBase.OnCompletionListener() {
            @Override public boolean onCompletion() {
                //观看录播时，收到该回调弹出播放结束提示；对于直播而言，收到该回调后应该查询业务服务器该直播是否结束，如果未结束，应该尝试重连
                return true;
            }
        };
        
        private EVPlayerBase.OnInfoListener mOnInfoListener = new EVPlayerBase.OnInfoListener() {
            @Override public boolean onInfo(int what, int extra) {
                switch (what) {
                    case IMediaPlayer.MEDIA_INFO_BUFFERING_START:
                        //开始缓冲，界面上给出提示
                        break;
                    case IMediaPlayer.MEDIA_INFO_BUFFERING_END:
                        //缓冲结束，界面上提示消失
                        break;
                }
                return true;
            }
        };
        
        private EVPlayerBase.OnErrorListener mOnErrorListener = new EVPlayerBase.OnErrorListener() {
            @Override public boolean onError(int what, int extra) {
                switch (what) {
                    case PlayerConstants.EV_PLAYER_ERROR_SDK_INIT:
                        //sdk初始失败，给出提示
                        break;
                    case PlayerConstants.EV_PLAYER_ERROR_GET_RESOURCE:
                        //播放资源调度失败，给出提示
                        break;
                    case PlayerConstants.EV_PLAYER_ERROR_GET_URL:
                        //获取播放地址失败，给出提示
                        break;
                    default:
                        //底层播放器抛出的错误，对于网络io错误可以考虑进行重连
                        break;
                }
    
                return true;
            }
        };

* 创建EVPlayer对象，在onCreate()函数中调用

        mEVPlayer = new EVPlayer(this);
        mEVPlayer.setParameter(builder.build());
        mEVPlayer.setVideoView(mVideoView);
        mEVPlayer.setEnableAutoReconnect(true);  //是否打开自动重连，默认情况下关闭，网络发生抖动直接报播放错误
        mEVPlayer.setOnPreparedListener(mOnPreparedListener);
        mEVPlayer.setOnCompletionListener(mOnCompletionListener);
        mEVPlayer.setOnInfoListener(mOnInfoListener);
        mEVPlayer.setOnErrorListener(mOnErrorListener);
        mEVPlayer.onCreate();

* 开始播放
调用mEVPlayer.watchStart()函数开始播放，该函数需要传入两个参数：lid和表示是否在直播的living，这两个参数通过上层业务获取。如下：

        mEVPlayer.watchStart(lid， living);

**关于lid和living参数**，sdk使用者应该自己搭建基本的视频管理后台，管理直播流的列表以及各个流的生命周期，包括流创建、直播中、停止、录播文件创建中、录播文件创建完成等状态（管理后台需要配置直播流状态通知url接收状态通知）。流列表中处于直播中或录播文件创建完成状态时，可以将lid和living参数下发给播放器sdk进行直播或回放视频的播放。

* Activity的生命周期回调处理。播放过程中，需要处理来电、误触、切后台等情况，**这些状态都依赖于Activity的生命周期，所以必须在Activity的生命周期中也调用EVPlayer SDK相应的接口**，以确保播放正常退出或恢复。如下：

        @Override
        protected void onStart() {
            super.onStart();
            if (null != mEVPlayer) {
                mEVPlayer.onStart();
            }
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            if (null != mEVPlayer) {
                mEVPlayer.onResume();
            }
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            if (null != mEVPlayer) {
                mEVPlayer.onStop();
            }
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            if (null != mEVPlayer) {
                mEVPlayer.onDestroy();
            }
        }


