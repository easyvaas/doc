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
int JoinChannel(IN const char* appid, IN const char* channel_id="", IN unsigned int uid=0, EVLive_User_Role useRole = EVLIVE_USER_ROLE_BROADCASTER, IN const int type = 1, IN const int record = 1)
```
加入频道(连麦、视频会议使用), 如果还未创建频道, 会自动创建。该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。如果已在通话中，用户必须调用leaveChannel退出当前通话，才能进入下一个频道。

参数说明：

| 名称 | 描述 |
|:--|:--|
| appid | 需要向售前申请 |
| channel_id |频道id，如果为空串则有服务器自动分配，并且在回调函数中返回。需用户自行维护保存维护 |
| uid | 用户id，如果为空串则有服务器自动分配，并且在回调函数中返回。需用户自行维护保存维护|
| userRole |  必须为EVLIVE_USER_ROLE_AUDIENCE(0)或者EVLIVE_USER_ROLE_BROADCASTER(1) |
| type | 0：不开启旁路直播，1：开启旁路直播。默认为1。只有对主播此参数才有效。|
| record |  0：不开启录制，1：开启录制，默认为1。只有对主播此参数才有效。|
| 返回值 | 0-成功，<0-失败 |

**注解**
    channelid不为空，则uid也不空。
    如果channelid和uid都为空，则服务器会创建一个新频道，并返回channelid和uid，而且此时必须为主播身份。进入成功即为频道拥有者。
    如果以主播身份，channelid和uid为之前保存的信息，则再次进入频道服务器就不会再创建新频道（后台的逻辑、
    如果频道有了拥有者，则该channelid不能为其他用户使用，返回Room exist...
    连麦观众想要进入频道必须有channelid，如果不输入uid则服务器分配一个。
    
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
| canvas | 画布信息 |
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

#### 配置旁路推流地址 (ConfigurePublisher)
```
int ConfigurePublisher(const EVPublisherConfiguration& config)
```
* 该方法用于配置旁路推流地址，以决定是否推流给普通观众观看。
* 请确保用户已经调用 SetClientRole() 且已将用户角色设为主播。
* 主播必须在调用CreateChannel之后，JoinChannel之前调用。
* CreateChannel的回调函数返回旁路推流地址，作为该函数入口参数config的推流地址。
**注解:主播必须在进入频道之前调用该接口，也就是说进入频道开始连麦之后，就不能再对旁路推流信息进行配置**
 参数说明:
 
| 名称 | 描述 |
|:--|:--|
| config | 旁路推流信息 |
| 返回值 | 0-成功，<0-失败 |

```
struct EVPublisherConfiguration{
	int width;
	int height;
	int framerate;
	int bitrate;
public:

	EVPublisherConfiguration()
		: width(640)
		, height(360)
		, framerate(15)
		, bitrate(500)
	{}
};
```

**注解**
请确保设置的分辨率、码率和帧率与主播上行设置一致，以免视频质量下降。

| 名称 | 描述 |
|:--|:--|
| width	| 设置旁路直播的输出码流的宽度。默认值为 360  |
|height	| 设置旁路直播的输出码流的高度。默认值为 360 |
|framerate	| 设置旁路直播的输出码率帧率。默认值为 15 fps |
|bitrate | 	设置旁路直播输出码流的码率。默认设置为 500 Kbps  |

#### 设置旁路推流布局(SetVideoCompositingLayout)
```
int SetVideoCompositingLayout(const EVVideoCompositingLayout& sei)
```
该方法设置直播场景里的画中画布局。该方法仅适用于在旁路推流的场景。当您在服务器端进行推流时:

* 您需要首先定义一个画布(canvas): 画布的宽和高(即视频的分辨率), 背景颜色，和您想在屏幕上显示的视频总数。
* 您需要在画布上定义每个视频的位置和尺寸(无论画布定义的宽和高有多大，每个视频用0到1的相对位置和尺寸进行定义)，图片所在的图层，图片的透明度，视频是经过裁减的还是缩放到合适大小等等。

参数说明:

| 名称 | 描述 |
|:--|:--|
| EVVideoCompositingLayout | 旁路推流布局设置 |

```

struct EVVideoCompositingLayout
{
 struct Region {
                 uid_t uid;
                 double x;//[0,1]
                 double y;//[0,1]
                 double width;//[0,1]
                 double height;//[0,1]
                 int zOrder; //optional, [0, 100] //0 (default): bottom most, 100: top most

                 //  Optional
                 //  [0, 1.0] where 0 denotes throughly transparent, 1.0 opaque
                 double alpha;

                 EVRENDER_MODE_TYPE renderMode;//EV_RENDER_MODE_HIDDEN: Crop, RENDER_MODE_FIT: Zoom to fit
                 Region()
                                 :uid(0)
                                 , x(0)
                                 , y(0)
                                 , width(0)
                                 , height(0)
                                 , zOrder(0)
                                 , alpha(1.0)
                                 , renderMode(EV_RENDER_MODE_HIDDEN)
                 {}

 };

```

下表包含各参数的详细解释。

| 名称 | 描述 |
|:--|:--|
| canvasWidth | 请忽略此参数。画布的宽度由 ConfigurePublisher() 方法设置而不是通过 canvasWidth 设置。 |
| canvasHeight | 请忽略此参数。画布的高度由 ConfigurePublisher() 方法设置而不是通过 canvasHeight 设置。 |
| backgroundColor | 用以定义屏幕(画布)的背景颜色，可根据 RGB 填写所需颜色对应的 6 位 符号 |
| regions | AgoraRtcVideoCompositingRegion 的主播用户列表。频道内每位主播在屏幕上均可以有一个区域显示自己的头像或视频,下面会详细说明 |
| appData | 应用程序自定义的数据 |

Region的详细说明:

| 名称 | 描述 |
|:--|:--|
|uid|待显示在该区域的主播用户 uid|
|x[0.0,1.0]| 屏幕里该区域的横坐标|
|y[0.0,1.0]|屏幕里该区域的横坐标|
|width[0.0, 1.0]|该区域的实际宽度|
|height[0.0, 1.0]|该区域的实际高度|
|zOrder[0, 100]|用于定义图层。 0 表示该区域图像位于最下层，而 100 表示该区域图像位于最上层|
|alpha[0.0, 1.0]|用于定义图像的透明度。 0 表示图像为透明的， 1 表示图像为完全不透明的|
|renderMode: RENDER_MODE_HIDDEN(1)|经过裁减的|


#### 连麦-静音所有远端音频（MuteAllRemoteAudioStreams）
```
int MuteAllRemoteAudioStreams(bool mute)
```
该方法用于允许/禁止播放远端用户的音频流，即对所有远端用户进行静音与否。该方法不影响音频数据流的接收，只是不播放音频流。参数说明：

| 名称 | 描述 |
|:--|:--|
| mute | 是否禁止播放远端音频流。true:禁止，false:取消禁止|
| 返回值 | 0-成功，<0-失败 |

#### 连麦-静音指定用户音频（MuteRemoteAudioStream）
```
int MuteRemoteAudioStream(unsigned int uid, bool mute)
```
静音指定远端用户/对指定远端用户取消静音。本方法用于允许/禁止播放远端用户的音频流。该方法不影响音频数据流的接收，只是不播放音频流。参数说明：

| 名称 | 描述 |
|:--|:--|
| uid | 用户uid，无符号整型|
| mute | 是否禁止播放远端音频流。true:禁止，false:取消禁止|
| 返回值 | 0-成功，<0-失败 |


#### 连麦-将自己静音（MuteLocalAudio）
```
 BOOL MuteLocalAudio(BOOL bMuted = TRUE)
```
/静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。该方法不影响录音状态，并没有禁用麦克风。参数说明：

| 名称 | 描述 |
|:--|:--|
| bMuted | 是否禁止本地音频流。true:禁止，false:取消禁止|
| 返回值 | 0-成功，<0-失败 |


#### 连麦-是否将自己静音（IsLocalAudioMuted）
```
 BOOL IsLocalAudioMuted()
```
/静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。该方法不影响录音状态，并没有禁用麦克风。参数说明：

| 名称 | 描述 |
|:--|:--|
| 返回值 | FALSE-未将自己静音，<TRUE-将自己静音 |


#### 连麦-暂停所有远端视频流 （MuteAllRemoteVideoStreams）
```
  int MuteAllRemoteVideoStreams(bool mute);
```
该方法用于允许/禁止播放所有远端视频流。该方法不影响视频数据流的接收，只是不播放视频流。。参数说明：

| 名称 | 描述 |
|:--|:--|
| bMuted | 是否禁止远端视频流。True : 停止播放所有用户的视频流，False : 允许播放所有用户的视频流|
| 返回值 | 0-成功，<0-失败 |


#### 连麦-暂停指定远端视频流 （MuteRemoteVideoStream）
```
  int MuteRemoteVideoStream(unsigned int uid, bool mute);
```
该方法用于允许/禁止播放指定的远端视频流。该方法不影响视频数据流的接收，只是不播放视频流。参数说明：

| 名称 | 描述 |
|:--|:--|
| bMuted | 是否禁止uid指定用户的远端视频流。True : 停止播放指定用户的视频流，False : 允许播放指定用户的视频流|
| 返回值 | 0-成功，<0-失败 |

#### 连麦-暂停本地视频流 （MuteLocalVideo）
```
  BOOL MuteLocalVideo(BOOL bMuted = TRUE);
```
该方法暂停发送本地视频流。该方法不影响本地视频流获取，没有禁用摄像头。只是不播放视频流。参数说明：

| 名称 | 描述 |
|:--|:--|
| bMuted | 是否禁止本地视频流。True : 不发送本地视频流，False : 发送本地视频流|
| 返回值 | 0-成功，<0-失败 |


#### 连麦-设置采集摄像头 （SetCurCamera）
```
  BOOL SetCurCamera(const char* deviceID)
```
设置采集要使用的摄像头设备id。参数说明：

| 名称 | 描述 |
|:--|:--|
| deviceID | 摄像头设备id。|
| 返回值 | 0-成功，<0-失败 |


#### 连麦-获取当前采集摄像id （GetCurCameraID）
```
  void GetCurCameraID(std::string& cameraId)
```
设置采集要使用的摄像头设备id。参数说明：

| 名称 | 描述 |
|:--|:--|
| cameraId | 当前使用的摄像头的设备id。|

**注解**
cameraId需要调用std::string::reserve预定一个足够大的空间。否则，cameraId的大小会超出默认为std::string分配的空间，造成重新分配内存空间，导致报错。
目前定义MAX_DEV_ID_LENGTH=512，预定内存。

#### 连麦-获取可以使用的所有摄像头（GetCameraDevices）
```
BOOL GetCameraDevices(OUT DevicesInfo* pVideoDev, OUT int& iDevCount, int maxDeviceCount)
```

| 名称 | 描述 |
|:--|:--|
| pVideoDev | 返回的可使用摄像头列表。|
| iDevCount | 可使用的摄像头的数目。|
| maxDeviceCount | 指定最多支持的摄像头数目。|

**注解**
DevicesInfo，包含设备名称：strDevName，设备id：strDevID。需要预先在对内存分配空间，并且由用户自行释放内存。






