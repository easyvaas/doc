## 推流端API参考说明 - Windows
### 主要方法
#### 初始化SDK（InitSDK）
```
void InitSDK(IN char* strAppID, IN char* strAccessKey, IN char* strSecretKey, IN char* strUserData)
```

初始化SDK，App调用SDK前，必须先调用此接口，参数说明：

| 名称 | 描述 |
|:--|:--|
| strAppID | AppID |
| strAccessKey | 视频云鉴权使用的AccessKey |
| strSecretKey | 视频云鉴权使用的SecretKey |
| strUserData | 用户自定义信息 |

#### 初始化 (InitInstance)

```
void InitInstance (HINSTANCE hMain)
```

显示视频窗口前，必须先初始化Instance。

| 名称 | 描述 |
|:--|:--|
| hMain | 主程序的实例句柄 |

#### 设置参数（setParameter）

```
void SetParameter(EVStreamerParameter& streamPara)
```
设置推流参数。

#### 初始化视频窗口（InitVideoWnd）

```
void InitVideoWnd(HWND hMain, HWND hVideo, bool bRemember)
```
初始化视频窗口，参数说明：

| 名称 | 描述 |
|:--|:--|
| hMain | 主窗口句柄 |
| hVideo | 视频窗口句柄 |
| bRemember | 是否记忆上次的场景源设置 |

#### 获取游戏源列表（GetGameOptions）
```
bool GetGameOptions(OUT GameInfo* pGameOpts, OUT int& iCount)
```
获取游戏源列表，参数说明：

| 名称 | 描述 |
|:--|:--|
| pGameOpts | 游戏源列表 |
| iCount | 游戏源数目 |

#### 获取可用的视频设备列表（GetVideoDevices）
```
bool GetVideoDevices(OUT DevicesInfo* pVideoDev, OUT int& iDevCount)
```
获取可用的视频设备列表，参数说明：

| 名称 | 描述 |
|:--|:--|
| pVideoDev | 视频设备列表 |
| iDevCount | 视频设备数目 |

#### 获取可用的音频设备列表（GetAudioDevices）
```
bool GetAudioDevices(OUT DevicesInfo* pAudioDev, OUT int& iDevCount)
```
获取可用的音频设备列表，参数说明：

| 名称 | 描述 |
|:--|:--|
| pAudioDev | 音频设备列表 |
| iDevCount | 音频设备数目 |

#### 添加场景源（AddScene）
```
bool AddScene(IN SceneInfo* pSceneInfo)
```

添加场景源，参数说明：

| 名称 | 描述 |
|:--|:--|
| pSceneInfo | 场景源信息的基类指针 |

场景源基类指针可代表如下对象：

* GameInfo
* MonitorsInfo
* WindowsInfo
* CameraInfo
* TextInfo
* ImageInfo

#### 编辑场景源（ModifyScene）
```
bool ModifyScene(IN SceneInfo* pSceneInfo)
```
编辑场景源，参数说明：

| 名称 | 描述 |
|:--|:--|
| pSceneInfo | 场景源信息的基类指针 |

#### 获取选中的场景源类型（GetSelSceneInfo）
```
bool GetSelSceneInfo(OUT SceneInfo* pScene)
```
获取选中的场景源类型，参数说明：

| 名称 | 描述 |
|:--|:--|
| pScene | 场景源信息 |

#### 选中场景源的操作（SetSelItemState）
```
void SetSelItemState (SceneItemsState itemState)
```
选中场景源的操作，itemState表示操作方式，包含：

```
SceneItemsState_MoveUp = 0,         // 上移;
SceneItemsState_MoveDown,           // 下移;
SceneItemsState_MoveToTop,          // 置顶;
SceneItemsState_MoveToBottom,       // 置底;
SceneItemsState_MoveToFull,         // 全屏;
```

#### 删除场景源（DeleteSelItems）
```
void DeleteSelItems()
```
删除场景源。

#### 准备推流（PrepareStream）
```
void PrepareStream(IN const char* strLID, IN const char* strKey)
```
准备推流，获取推流地址。参数说明：

| 名称 | 描述 |
|:--|:--|
| strLID | 推流唯一id |
| strKey | 获取推流地址接口鉴权key |

#### 开始推流（StartStream）
```
void StartStream()
```
开始推流

#### 结束推流（StopStream）
```
void StopStream()
```
结束推流

#### 停止推流（stopLive）
```
public void stopLive()
```
停止推流

#### 设置麦克风音量（SetMicropVolume）
```
void SetMicropVolume(float fValue)
```
设置麦克风音量，fValue是0-1之间的浮点数。

#### 获取麦克风音量（GetMicropVolume）
```
float GetMicropVolume()
```
获取麦克风音量。

#### 设置扬声器(桌面)音量（SetDesktopVolume）
```
void SetDesktopVolume(float fValue)
```
设置扬声器(桌面)音量。

#### 获取扬声器(桌面)音量（GetDesktopVolume）
```
float GetDesktopVolume()
```
获取扬声器(桌面)音量。

#### 连麦-加入频道（JoinChannel）
```
int JoinChannel(const char* lpChannelName, const char* info)
```
加入频道(连麦、视频会议使用), 如果还未创建频道, 会自动创建。该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。如果已在通话中，用户必须调用leaveChannel退出当前通话，才能进入下一个频道。

参数说明：

| 名称 | 描述 |
|:--|:--|
| lpChannelName | 标识通话的频道名称，长度在64字节以内的字符串，由业务后台提供 |
| info | (非必选项)附加信息，一般可设置为空字符串 |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-离开频道（LeaveChannel）
```
int LeaveChannel()
```
* 离开频道，即挂断或退出通话。
* joinChannel后，必须调用leaveChannel以结束通话，否则不能进行下一次通话。
* leaveChannel是异步操作，调用返回时并没有真正退出频道。在真正退出频道后，SDK会触发onLeaveChannel回调。

* 注意:如果您调用了leaveChannel后立即调用 release()，SDK 将无法触发 onLeaveChannel 回调。
* 接口返回值：0-成功，<0-失败

#### 连麦-设置频道属性（SetChannelProfile）
```
int SetChannelProfile(EV_CHANNEL_PROFILE_TYPE profile)
```
* 设置频道属性，该方法用于设置频道模式(Profile)。
* 注意：同一频道内只能同时设置一种模式。该方法必须在加入频道前调用和进行设置，进入频道后无法再设置。

参数说明：

| 名称 | 描述 |
|:--|:--|
| profile | 频道模式 |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-设置用户角色（SetClientRole）
```
int SetClientRole(EV_CLIENT_ROLE_TYPE role, const char* permissionKey)
```
设置连麦用户角色，参数说明：

| 名称 | 描述 |
|:--|:--|
| role | 用户角色 |
| permissionKey | 授权key，当前置为NULL即可 |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-设置本地视频属性（SetVideoProfile）
```
int SetVideoProfile(EV_VIDEO_PROFILE_TYPE profile, bool swapWidthAndHeight)
```
设置本地视频属性。注意：应在调用joinChannel/startPreview前设置视频属性。
参数说明：

| 名称 | 描述 |
|:--|:--|
| profile | 视频属性 |
| swapWidthAndHeight | 是否交换宽和高。true ：交换宽和高；false：不交换宽和高(默认) |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-启用/禁用网络测试（EnableLastmileTest）
```
int EnableLastmileTest(bool bEnable)
```
该方法启用/禁用网络连接质量测试，用于检测用户网络接入质量。默认该功能为关闭状态。

#### 连麦-打开/关闭视频模式（EnableVideo）
```
int EnableVideo(bool bEnable)
```
该方法用于开启/关闭视频模式。可以在加入频道前或者通话中调用，在加入频道前调用，则自动开启视频模式，在通话中调用则由音频模式切换为视频模式。

#### 连麦-设置流类型（SetRemoteStreamType）
```
int SetRemoteStreamType(UINT nUID, EV_REMOTE_VIDEO_STREAM_TYPE nType)
```
设置流类型。参数说明：

| 名称 | 描述 |
|:--|:--|
| nUID | 用户id |
| nType | 视频流类型 |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-开启视频预览（StartPreview）
```
int StartPreview()
```
该方法用于启动本地视频预览。在开启预览前，必须先调用setupLocalVideo设置预览窗口及属性，且必须调用enableVideo开启视频功能。

#### 连麦-开启屏幕共享（StartScreenCapture）
```
int StartScreenCapture(HWND hCaptureWnd)
```
该方法让用户在启用屏幕共享功能后切换共享屏幕或特定窗口，参数说明：

| 名称 | 描述 |
|:--|:--|
| hCaptureWnd | 指定共享的屏幕或窗口 |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-停止视频预览（StopPreview）
```
int StopPreview()
```
该方法用于停止本地视频预览

#### 连麦-设置远程视频显示视图（SetupRemoteVideo）
```
int SetupRemoteVideo(const EVVideoCanvas& canvas)
```
该方法绑定远程用户和显示视图，即设定uid指定的用户用哪个视图显示。参数说明：

| 名称 | 描述 |
|:--|:--|
| canvas | the canvas information |
| 返回值 | 0-成功，<0-失败 |

#### 连麦-设置本地视频视图（SetupLocalVideo）
```
int SetupLocalVideo(const EVVideoCanvas& canvas)
```
* 该方法设置本地视频的显示相关属性。
* 应用程序通过调用此接口绑定本地视频流的显示视窗(view)，并设置视频显示模式。
* 在应用程序开发中，通常在初始化后调用该方法进行本地视频设置，然后再加入频道。

参数说明：

| 名称 | 描述 |
|:--|:--|
| canvas | 视频显示相关信息 |
| 返回值 | 0-成功，<0-失败 |


