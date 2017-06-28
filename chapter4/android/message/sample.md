### 示例代码
#### 创建全局Application
创建全局Application文件，如果目标工程全局Application文件已经存在，则在该文件合适的地方使用EVSdk.init()方法来进行初始化，将上面获取到的APPID，Access Key，Secret Key作为该方法的参数，该方法调用的位置应该是：**目标工程用户登录成功后调用全局Application设置用户信息的函数，将用户信息作为EVSdk.init的参数**。如下：

        //打开SDK详细日志开关
        EVSdk.enableDebugLog();
            
        //初始化SDK，传入APPID，Access Key，Secret Key，userdata等参数
        EVSdk.init(app.getApplicationContext(), "APPID", "Access Key", "Secret Key", "userdata");

#### 使用消息系统
可参考demo工程中的com.easyvaas.sdk.demo.MessageActivity类完成一个简单的消息系统示例

* 在AndroidManifest.xml文件中注册Activity，固定为竖屏

        <activity
            android:name=".MessageActivity"
            android:configChanges="keyboard"
            android:label="@string/title_activity_message"
            android:screenOrientation="portrait"
            android:windowSoftInputMode="adjustPan" >
        </activity>
        
* 创建消息系统回调

        private MessageCallback mMessageCallback = new MessageCallback() {
            @Override public void onConnected() {
                Log.d(TAG, "连接聊天服务器成功");
            }

            @Override public void onNewMessage(String message, String userdata, String userid, String channel) {
                Log.d(TAG, "收到消息：" + message);
            }
            
            @Override public void onHistoryMessage(List<MsgInfoEntity> msgs, int next, int count) {
            //处理历史消息
            }
            
            @Override public void onUserJoin(List<String> users) {
            //处理用户进入消息
            }
            
            @Override public void onUserLeave(List<String> users) {
            //处理用户离开消息
            }

            @Override public void onLikeCount(int likeCount) {
                Log.d(TAG, "点赞数更新: " + likeCount);
            }

            @Override public void onWatchingCount(int watchingCount) {
                Log.d(TAG, "正在观看人数: " + watchingCount);
            }

            @Override public void onWatchedCount(int watchedCount) {
                Log.d(TAG, "已观看人数: " + watchedCount);
            }

            @Override public void onError(int errCode) {
                Log.d(TAG, "聊天交互发生错误,错误码: " + errCode);
            }

            @Override public void onClose() {
                Log.d(TAG, "连接关闭，上层需要决定是否重连");
            }
        };

* 创建消息系统对象

        mEVMessage = new EVMessage(this);
        mEVMessage.setMessageCallback(mMessageCallback);

* 连接消息系统服务器

        mEVMessage.connect(mChannel);
mChannel:连接消息时需要指定channel字符串，即频道名称，一般指直播间的唯一id，由上层业务决定

* 发送消息      
    
	    //发送评论
	    mEVMessage.send(mChannel, comment, userData, Constants.MESSAGE_TYPE_MSG, mSendCallback);

* 点赞

		//增加点赞数，上层可以点一次调用一次，也可以记录用户的点击次数，定时调用该接口，减少接口调用开销
		mEVMessage.addLikeCount(mChannel, 1);
* 获取最近条数历史消息
		
		//获取最近count条历史消息
		public void getLastHistoryMsgs(String channel, int count, String type)

	* channel：聊天频道id，指聊天房间唯一id
	* count：获取历史消息的条数
	* type：历史消息类型，如果想要获取多种类型的历史消息，使用逗号分隔，例如"system,msg,redpack,gift"，如果该字符串为空，获取全部类型的历史消息

* 获取历史消息
		
		//获取从start开始的count条历史消息
		public void getHistoryMsgs(String channel, int start, int count, String type)
	* channel：聊天频道id，指聊天房间唯一id
	* start：历史消息起始点消息id
	* count：获取历史消息的条数
	* type：历史消息类型，如果想要获取多种类型的历史消息，使用逗号分隔，例如"system,msg,redpack,gift"，如果该字符串为空，获取全部类型的历史消息
        
* 关闭

		mEVMessage.close();  //重要，退出Activity时在OnDestroy函数中调用，否则会重复收到消息


