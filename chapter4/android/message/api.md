## 直播端API参考说明 - Android
### 方法
#### 初始化 (EVMessage)

```
public EVMessage(Context context)
```

该方法创建EVMessage对象

#### 设置回调（setMessageCallback）

```
public void setMessageCallback(MessageCallback callback)
```
设置消息系统回调函数。消息系统运行过程中各种事件的回调，包括网络连接、发送消息、接收消息等事件的回调。MessageCallback的定义详见下方事件中的描述

#### 连接（connect）

### 事件

