## 消息交互API参考说明 - Android
### 主要方法
#### 初始化 (EVMessage)

```
public EVMessage(Context context)
```

该方法创建EVMessage对象

#### 设置回调（setMessageCallback）

```
public void setMessageCallback(MessageCallback callback)
```
设置消息系统回调函数。消息系统运行过程中各种事件的回调，包括网络连接、发送消息、接收消息等事件的回调。MessageCallback的定义详见下方回调事件中的描述

#### 连接（connect）

```
public void connect(String channel)
```
连接房间，频道号channel字符串由上层业务指定，一般以视频唯一id作为房间号

#### 发送消息（send）
```
public void send(String channel, String message, String userdata, String type,
                     MyRequestCallBack<String> callBack)
```

该方法用于发送消息，消息级别、是否保存、是否过滤等设置使用默认值。参数说明如下：

| 名称 | 描述 |
|:--|:--|
| channel | 房间唯一id，频道号 |
| message | 文本消息内容，用户发送普通文本消息时使用，可以为空 |
| userdata | 用户发送自定义消息时使用，一般为json字符串，由上层业务系统定义该协议 |
| type | 消息类型，用于指定发送消息的类型 |
| callBack | 发送消息异步回调 |

#### 发送消息（sendMsg）
```
public void sendMsg(String channel, String message, String userdata, String type, int level,
                        boolean save, boolean filter, MyRequestCallBack<String> callBack)
```
该方法是send方法的高级版本，可以指定消息级别、是否保存、是否过滤等。参数说明如下：

| 名称 | 描述 |
|:--|:--|
| channel | 房间唯一id，频道号 |
| message | 文本消息内容，用户发送普通文本消息时使用，可以为空 |
| userdata | 用户发送自定义消息时使用，一般为json字符串，由上层业务系统定义该协议 |
| type | 消息类型，用于指定发送消息的类型 |
| level | 优先级[0,10]，值越大越重要，其中level为9,10的不能过滤 |
| save | 消息是否保存 |
| filter | 是否使用关键词过滤 |
| callBack | 发送消息异步回调 |

#### 获取最近N条历史消息（getLastHistoryMsgs）
```
public void getLastHistoryMsgs(String channel, int count, String type)
```
该方法用于获取最近的N条历史消息，第一次获取历史消息时使用。

| 名称 | 描述 |
|:--|:--|
| channel | 房间唯一id，频道号 |
| count | 获取历史消息的条数 |
| type | 逗号分隔的字符串,表示历史消息的消息类型,例如"sys,msg,gift",为空表示获取全部类型的历史消息 |

#### 获取从指定位置开始的N条历史消息（getHistoryMsgs）
```
public void getHistoryMsgs(String channel, int start, int count, String type)
```

该方法用于获取从指定位置开始的N条历史消息，当start为0时，和getLastHistoryMsgs方法效果相同

#### 关闭（close）
```
public void close()
```
退出聊天交互界面时调用该函数，关闭网络连接，停止接收消息。

### 回调事件（MessageCallback）
MessageCallback接口类用于消息SDK向应用程序发送回调事件通知，应用程序通过该接口类的方法获取SDK的事件通知。

#### 连接成功回调（onConnected）
```
void onConnected()
```
表示客户端已经加入聊天交互房间，房间id是connect方法中指定的房间id。
#### 接收到消息回调（onNewMessage）
```
void onNewMessage(final String message, final String userdata, final String userid,
                      final String channel, final String type)
```

表示收到交互房间中的新消息，参数说明如下：

| 名称 | 描述 |
|:--|:--|
| message | 文本消息内容 |
| userdata | 自定义消息字符串 |
| userid | 用户唯一id |
| channel | 房间唯一id，频道号 |
| type | 消息类型 |

#### 历史消息回调（onHistoryMessage）
```
void onHistoryMessage(final List<MsgInfoEntity> msgs, final int next, final int count)
```
调用获取历史消息的异步回调通知，参数说明如下：

| 名称 | 描述 |
|:--|:--|
| msgs | 历史消息列表 |
| next | 下一条历史消息的索引，可以作为继续获取历史消息的起始索引 |
| count | 获取到的历史消息条数 |

#### 用户进入回调（onUserJoin）
```
void onUserJoin(final List<String> users)
```

表示房间中有用户加入。

| 名称 | 描述 |
|:--|:--|
| users | 加入房间用户id的列表 |
#### 用户离开回调（onUserLeave）
```
void onUserLeave(final List<String> users)
```

表示房间中有用户离开。

| 名称 | 描述 |
|:--|:--|
| users | 离开房间用户id的列表 |
#### 正在观看人数回调（onWatchingCount）
```
void onWatchingCount(int watchingCount)
```

房间实时观看人数有变化会回调该方法。

| 名称 | 描述 |
|:--|:--|
| watchingCount | 互动房间在线人数 |
#### 已观看人数回调（onWatchedCount）
```
void onWatchedCount(int watchedCount)
```

房间已经观看人数有变化回调该方法。

| 名称 | 描述 |
|:--|:--|
| watchedCount | 互动房间累计在线人数 |
#### 错误回调（onError）
```
void onError(final int errCode)
```

用户互动过程中发生错误，回调该方法。错误码详情：

| 名称 | 数组 | 含义 |
|:--|:--|:--|
| EV_MESSAGE_ERROR_NETWORK_TIMEOUT | -1001 | 连接超时 |
| EV_MESSAGE_ERROR_NETWORK_INVALID_URL | -1002 | 消息服务器URL错误 |
| EV_MESSAGE_ERROR_NETWORK_NOT_CONNECT | -1003 | 未连接 |
| EV_MESSAGE_ERROR_NETWORK_UNKNOWN | -1101 | 未知的网络错误 |
| EV_MESSAGE_ERROR_SDK_INIT | -2001 | sdk未初始化或初始化不成功 |
| EV_MESSAGE_ERROR_LOGIN | -2002 | 用户登录错误 |
| EV_MESSAGE_ERROR_INTERNAL_SERVER | -3001 | 消息服务器内部错误 |
#### 重连回调（onReconnecting）
```
void onReconnecting()
```

由于网络原因，客户端可能会和消息服务器失去连接，互动消息SDK会进行自动重连，开始重连时触发此回调方法。
#### 重连成功回调（onReconnected）
```
void onReconnected()
```

互动消息SDK内部重连成功后触发此回调方法。
#### 重连失败回调（onReconnectFailed）
```
void onReconnectFailed()
```

互动消息SDK内部不会无限重连，当重连次数超过一定限制还未连接上消息服务器，会触发重连失败回调。
#### 连接断开回调（onClose）
```
void onClose()
```
客户端和消息服务器连接断开会触发此回调。

