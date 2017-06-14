## **功能详细使用说明**
### 发送消息
#### 消息类型

* MESSAGE_TYPE_MSG = "msg"; //评论消息
* MESSAGE_TYPE_REDPACK = "redpack"; //红包消息
* MESSAGE_TYPE_GIFT = "gift"; //礼物消息
* MESSAGE_TYPE_SYS = "system"; //系统消息

#### 普通接口
	public void send(String topic, String message, String userdata, String type, MyRequestCallBack<String> callBack)
消息系统sdk提供发送消息的send接口，消息级别和是否保存使用默认值，如果需要指定消息级别或是否保存，使用以下的高级接口sendMsg。

#### 高级接口
	public void sendMsg(String topic, String message, String userdata, String type, int level, boolean save, MyRequestCallBack<String> callBack)
	
* 指定消息优先级，优先级[0,10]，值越大越重要，缺省为4; level为9,10的不能过滤
* 指定消息是否保存，缺省为true

#### 参数说明
* message：String类型，发送评论时，传递评论内容；其他消息时可以为空
* userdata：String类型，一般为json字符串，传递结构化的数据，由上层业务系统定义该协议。例如发送评论时，协议内容可能为发送该消息的用户昵称、@等信息；发送红包时，协议内容包含红包相关信息。
* ***重要说明：*** 组装发送的消息体时，message是消息内容的字符串，userdata是json字符串，需要转换成json对象放送，例如：{"context":"hello","userdata":{"exct":{"nk":"布拉德皮蛋!@#$%"}}}，各个客户端（包括Android，iOS，pc，web）需要严格按照这个规则进行消息的发送和解析

### 错误回调

消息系统SDK错误状态码如下表：

| 名称 | 数组 | 含义 |
|:--|:--|:--|
| EV_MESSAGE_ERROR_NETWORK_TIMEOUT | -1001 | 连接超时 |
| EV_MESSAGE_ERROR_NETWORK_INVALID_URL | -1002 | 消息服务器URL错误 |
| EV_MESSAGE_ERROR_NETWORK_NOT_CONNECT | -1003 | 未连接 |
| EV_MESSAGE_ERROR_NETWORK_UNKNOWN | -1101 | 未知的网络错误 |
| EV_MESSAGE_ERROR_SDK_INIT | -2001 | sdk未初始化或初始化不成功 |
| EV_MESSAGE_ERROR_LOGIN | -2002 | 用户登录错误 |
| EV_MESSAGE_ERROR_INTERNAL_SERVER | -3001 | 消息服务器内部错误 |

***


