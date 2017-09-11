## 功能详细说明

#### 关于创建、加入频道

多人连麦 SDK 中提供了三种身份：Master（主播）、LiveGuest（连麦观众）、Guest（观众），一个频道中只能有一个 Master，可以有最多六个 LiveGuest，若干 Guest。

* 一个频道只能创建一次，创建该频道的用户身份应为 `Master`
* 用户只能加入已创建的频道进行多人连麦，加入已有频道进行连麦的用户身份应为 `LiveGuest`
* 当某一频道开启了旁路直播，则可以通过获取流地址进行观看，观看者用户身份应为 `Guest`

所以，一个频道的连麦步骤大体如下：

**一. Master 创建并加入一个频道 `channel`，开启旁路直播，保存视频**

```java
mEVRtc.createAndJoinChannel(channel, uid, true, true);
```

此时名为 `channel` 的频道已经由一名 Master 用户成功创建了，此频道中当前只有创建频道的用户一个人

**二. 一名连麦观众（LiveGuest）想加入 `channel` 频道**

```java
mEVRtc.joinChannel(channel, uid);
```

此时频道 `channel` 中有两名用户，其他主播也可以通过第[二]步操作，以 `LiveGuest` 身份加入连麦，进行多人连麦

*注：一个频道中包括 Master 在内最多可以容纳七人。*

**三. 一名观众（Guest）想观看 `channel` 频道中的多人连麦视频流，但自己不进行连麦操作**

```java
mEVRtc.watchLive(channel);
```

获取连麦视频流地址后，既可通过播放器观看 `channel` 频道下的视频流，人数无限制。

#### 旁路直播画中画设置

是否开启旁路推流，以及旁路视频流中画布的布局设置，只有 Master 才有权限设置

设置旁路视频流画布布局可在视图渲染后，通过 `mEVRtc.configCompositiongLayout(regions);` 方法进行设置，regions 是一个由 `EVLayoutRegion` 类型对象组成的数组，每个对象中主要包含对应的 uid、isMaster、x、y、width、height 等

*注：x、y、width、height 的值区间为 [0,1]，按百分比计算*

```java
mEVRtc.configCompositiongLayout(regions);

public class EVLayoutRegion {
    //待显示在该区域的连麦用户uid
    private int uid;
    
    //连麦用户是否为主播
    private boolean isMaster;
    
    //屏幕里该区域的横坐标，取值范围[0.0,1.0]
    private double x;
    
    //屏幕里该区域的纵坐标，取值范围[0.0,1.0]
    private double y;
    
    //该区域的实际宽度，取值范围[0.0,1.0]
    private double w;
    
    //该区域的实际高度，取值范围[0.0,1.0]
    private double h;
}
```

#### 枚举值

* 用户加入频道的身份枚举

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|RTC_ROLE_MASTER|0|主播|
|RTC_ROLE_LIVE_GUEST|1|连麦观众|
|RTC_ROLE_GUEST|2|观众|

* 连麦视频清晰度

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|RTC_RESOLUTION_HD|0|高清: 1280x720 15fps 1000kbps|
|RTC_RESOLUTION_SD|1|标清: 640x360 15fps 400kbps|
|RTC_RESOLUTION_LD|2|流畅: 320x180 15fps 160kbps|

* 回调中的错误码枚举 

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|RTC_ERROR_LIVE_SHOW|1001|播放连麦房间直播错误|
|RTC_ERROR_RECORD_SHOW|1002|播放连麦房间录播错误|
|RTC_ERROR_CHANNEL_NOT_EXIST|1003|连麦房间不存在|
|RTC_ERROR_INVALID_APPID|1004|无效的APPID |
|RTC_ERROR_CREATE_CHANNEL|1005|主播创建连麦房间失败|
|RTC_ERROR_CHANNEL_CONFLICT|1006|连麦房间冲突,创建失败|
|RTC_ERROR_USER_AUTH|1007|用户没有连麦权限|
|RTC_ERROR_PERMISSION_KEY|1008|获取连麦权限key出错|
|RTC_ERROR_CHANNEL_EMPTY|1009|传入的连麦房间ID为空|
|RTC_ERROR_CHANNEL_MAX_USERS|1010|超出房间连麦人数最大限制|
|RTC_ERROR_MASTER_OFFLINE|1011|主播离开房间|
|RTC_ERROR_JOIN_CHANNEL|1012|加入互动房间失败|
|RTC_ERROR_USER_ALREADY_JOINED|1013|用户已经在房间|


