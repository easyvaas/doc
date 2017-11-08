## 功能详细说明

#### 关于创建、加入频道

Easyvaas 多人连麦 SDK 中提供了三种身份：Master（主播）、LiveGuest（连麦观众）、Guest（观众），一个频道中只能有一个 Master，可以有最多六个 LiveGuest，若干 Guest。

* 一个频道只能创建一次，创建该频道的用户身份应为 `Master`
* 用户只能加入已创建的频道进行多人连麦，加入已有频道进行连麦的用户身份应为 `LiveGuest`
* 当某一频道开启了旁路直播，则可以通过获取流地址进行观看，观看者用户身份应为 `Guest`

一个频道的连麦步骤大体如下：

**一. Master 创建并加入一个频道 `customChannel`，开启旁路直播，保存视频**

```objective-c
[self.rtcKit createAndJoinChannel:customChannel uid:0 hasPublisher:YES record:YES callback:^(EVRtcResponseCode code, NSDictionary *info, NSError *error) {
    if (code == EVRtcResponseCode_None) {
        // 创建、加入频道成功
    } else {
        // 创建、加入频道失败    
    }
}];
```

此时名为 `customChannel` 的频道已经由一名 Master 用户成功创建了，此频道中当前只有创建频道的用户一个人

**二. 一名连麦观众（LiveGuest）想加入 `customChannel` 频道**

```objective-c
[self.rtcKit joinChannel:customChannel uid:0 callback:^(EVRtcResponseCode code, NSDictionary *info, NSError *error) {
    if (code == EVRtcResponseCode_None) {
        // 加入频道成功
    } else {
        // 加入频道失败
    }
}];
```

此时频道 `customChannel` 中有两名用户，其他主播也可以通过第[二]步操作，以 `LiveGuest` 身份加入连麦，进行多人连麦

*注：一个频道中包括 Master 在内最多可以容纳七人。*

**三. 一名观众（Guest）想观看 `customChannel` 频道中的多人连麦视频流，但自己不进行连麦操作**

```objective-c
[self.rtcKit watchLiveWithChannel:self.roomName callback:^(EVRtcResponseCode code, NSDictionary *info, NSError *error) {
    if (code == EVRtcResponseCode_None) {
        // 已获取到播放地址
    } else {
        // 获取播放地址失败
    }
}];
```

获取连麦视频流地址后，既可通过播放器观看 `customChannel` 频道下的视频流，人数无限制。

#### 关于画布设置

##### 本地画布设置

一个频道中的每一位用户，都有自己唯一的标识 `uid`。当 `customChannel` 频道中加入了新用户时，我们想显示该用户的画面，就需要为其创建一个和 uid 绑定的 `UIView`，通过改变这个视图的布局，来实现更改本地各个连麦画面布局的效果。

创建好了绑定唯一 uid 的 newView，我们需要让该 uid 对应用户的视频流显示到新创建的视图上，调用如下方法传入 newView，SDK 内部会对其进行渲染处理

```objective-c
[self.rtcKit configCanvasWithView:newView uid:uid mode:EVRtc_Render_Fit];
```

本地需要维护一个数组array，来缓存每个用户的 uid、view 等信息。
当前用户创建或加入频道时，可标识 uid = 0 创建用户信息并加入到array中，本地即可使用 uid=0 来识别当前用户；新加入的用户可以在代理回调`firstRemoteVideoDecodedOfUid`中创建新用户信息，并加入数组array。详情请参考demo。

##### 旁路视频流中画布设置

是否开启旁路推流，以及旁路视频流中画布的布局设置，只有 Master 才有权限设置

设置旁路视频流画布布局可在视图渲染后，通过 `- (void)configVideoRegion:(NSArray<EVRTCVideoRegion *> *)regions;` 方法进行设置，regions 是一个由 `EVRTCVideoRegion` 类型对象组成的数组，每个对象中主要包含对应的 uid、x、y、width、height 等

*注：x、y、width、height 的值区间为 [0,1]，按百分比计算*

```objective-c
[self.rtcKit configVideoRegion:regions];
```
SDK内默认的旁路推流布局如下，其中数字代表连麦观众显示的顺序，最外层为屏幕边缘，具体效果详见 Demo 运行效果

```
 --------------------------------
 |       |     4    |     1     |
 |       -----------------------|
 |       |     5    |     2     |
 |       -----------------------|
 |       |     6    |     3     |
 --------------------------------
```

#### 注意事项

* 使用RTC模块前，需要先通过 appid/appkey/appsecret 进行初始化，初始化成功才可使用服务
* 三种身份中，Master只能、必须有`1个`，LiveGuest最多有`6个`，Guest个数`不限`(Guest身份只是获取播放地址，所以不存在限制)
* Master如果在连麦过程中出现异常退出，可通过原来的 channel/uid 可重新加入其已创建的房间
* LiveGuest异常退出后，不能重新以原有 uid 重新加入房间，并且原有 uid 在其心跳超时之前会占用一个 LiveGuest 的位置
 

#### 枚举值

用户加入频道的身份枚举 `EVRtcClientRole`

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|EVRtc_ClientRole_Master|0|主播|
|EVRtc_ClientRole_LiveGuest|1|连麦观众|
|EVRtc_ClientRole_Guest|2|观众|

连麦断线理由枚举 `EVRtcOfflineReason`

| 名称 | 数值 |
| :-- | :-- |
|EVRtc_UserOffline_Quit|0|
|EVRtc_UserOffline_Dropped|1|
|EVRtc_UserOffline_BecomeAudience|2|

画面布局类型枚举 `EVRtcRenderMode`

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|EVRtc_Render_Hidden|1|裁剪画面，填充显示|
|EVRtc_Render_Fit|2|不裁剪画面，自适应视图|
|EVRtc_Render_Adaptive|3|如果屏幕方向一致，则等同于 AgoraRtc_Render_Hidden；屏幕方向不一致，则等同于 AgoraRtc_Render_Fit|

远端视频流清晰度枚举 `EVRtcVideoStreamType`

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|EVRtc_VideoStream_High|0|高清晰度，默认|
|EVRtc_VideoStream_Low|1|低清晰度|

回调中的错误码枚举 `EVRtcResponseCode`

| 名称 | 数值 | 含义 |
| :-- | :-- | :-- |
|EVRtcResponseCode_None|0|无错误|
|EVRtcResponseCode_Server|-1004|SDK 内部网络请求错误|
|EVRtcResponseCode_NoAppId|-2002|获取 App id 失败|
|EVRtcResponseCode_Core|-4000|核心处理模块错误 |
|EVRtcResponseCode_NoRtcId|-4001|没有传入连麦 id|
|EVRtcResponseCode_NoCanvasView|-4002|没有传入画布视图|
|EVRtcResponseCode_NotTheMaster|-4003|操作用户不是 master|
|EVRtcResponseCode_NoRegions|-4004|没有传入 Regions 数组|
|EVRtcResponseCode_NoChannel|-4005|没有传入频道字符串|
|EVRtcResponseCode_MasterExit|-4006|主播退出了频道|
|EVRtcResponseCode_GuestExceeded|-4007|连麦观众超出了6个|


