## Easyvaas iOS SDK 更新日志

* 版本 1.1.10  2017-06-27
> [下载地址](https://github.com/easyvaas/sdk_demo_iOS/releases/download/v1.1.10/sdk_demo_iOS_v1.1.10.zip)
  
    *新功能：*
    1. 更新了消息系统，新版消息系统在速度和稳定性上都有大幅度提升
    2. 针对新版消息系统更新了`EVMessageManager`大部分 API，使用方式简易，可轻松过渡版本
    3. 消息系统中新增*历史消息*获取方法，便于用户获取某话题下的历史消息
    4. 消息系统中新增*消息等级、消息类型*枚举，便于用户针对特定消息进行处理
    5. 消息系统内部加入重连机制，让用户更专注于顶层业务逻辑

    *优化：*
    1. 解决了 Demo 中存在的一些已知问题

---

* 版本 1.1.9  2017-06-19  
    *新功能：*
    1. 在推流类（EVStreamer）中增加 `useHorizonMode` 属性，用于设置横屏推流

---

* 版本 1.1.8  2017-06-12  
    *优化：*  
    1. 修改默认解码方式为软解  
    2. 提高连麦后辅播推流的分辨率  

---

* 版本 v1.1.7  2017-06-07   
    *新功能：*  
    
    1. 在推流类(EVStreamer)中增加`winRect(CGRect)`属性，用于调整连麦后小窗口的布局
    
    *优化：*  
    
    1. 删除推流类已废弃的回调方法：  
    ~~- (void)EVStreamerUpdateState:(EVStreamerState)state error:(NSError *)error;~~
    ~~- (void)EVStreamerRecordAudioBufferList:(AudioBufferList *)audioBufferList;~~
    ~~- (void)EVInteractiveLiveUpdateStatus:(EVInteractiveLiveStatus)status;~~
    2. 删除EVVideoChatState枚举中的 ~~EVVideoChatStateChannelOffline、EVVideoChatStateChannelJoinError~~ ，出现连接问题会直接终止连麦  
    
    *修复：*  
    
    1. 修复推流类回调方法`- (void)EVStreamerUpdateVideoChatState:(EVVideoChatState)chatState;`失效的问题  
    2. 解决连麦长时间偶尔出现的音视频不同步问题 

---

* 版本 v1.1.6  2017-05-26  
    *修复：*  
    
    1. 修复了设置默认摄像头方向失效的问题 

---

* 版本 v1.1.5  2017-05-20   
    *修复：*  
    1. 修改默认播放视频格式，避免视频出现卡帧的现象

---

* 版本 v1.1.4  2017-05-18  
    *新功能：*  
    
    1. 播放器增加`-seek:`方法，通过精准定位改变录播视频进度  

    *优化：*  
    
    1. 为Demo增加二维码QR扫描获取lid的模块，方便用户使用  
    2. 为Demo增加录播播放页  

    *修复：*  
    
    1. 修复iphone5播放录屏时偶尔出现黑屏的问题

---

* 版本 v1.1.3  2017-05-06  
    *修复：*
    
    1. 修复通过lid播放录播时无法获取流地址的Bug

---

* 版本 v1.1.2  2017-05-05  
    *优化：*  
    
    1. EVPlayer中新增`playerViewFrame`属性，用以控制播放器视图frame

---

* 版本 v1.1.1  2017-05-04
    *新功能:*  
    
    1. 播放器添加手机竖屏模式下横屏观看的设置方法`setVideoPlayInHorizonMode`  
    2. 播放器添加显示模式设置方法`scalingMode`  


