# 集成SDK常见问题

[集成易视云iOS SDK常见问题](#1)

* [1.为什么我运行后初始化提示我 Code=-1001?](#1.1)
* [2.导入 iOS Easyvaas 的 framework 后，无法运行工程？](#1.2)
* [3. 推流模块中的连麦按钮为什么点击后无响应？](#1.3)
* [4. demo中推流端点击vid会生成二维码，拉流端扫描后可以观看，必须要这样交互吗？](#1.4)
* [5. 项目可运行，但输入直播vid后，点击观看为何直接崩溃？](#1.5)
* [6. 我集成了VR播放功能，但为何无法播放从网上找的VR链接？](#1.6)
* [7. Xcode运行后控制台输出：Reason: image not found](#1.7)
* [8. Xcode9运行后报错： "___llvm_profile_runtime", referenced from:](#1.8)


<h3 id='1'> 集成易视云iOS SDK常见问题 </h3>

---

<h4 id='1.1'> 1. 为什么我运行后初始化提示我 Code=-1001? </h4>

```
根据提示信息：Code=-1001 info=appid cannot be nil

解：请填入正确的 appid/appkey/appsecret
```

---


<h4 id='1.2'> 2. 导入 iOS Easyvaas 的 framework 后，无法运行工程？ </h4>

```
通常是因为缺少配置导致的

解：请参考各模块文档中的[快速开始]，导入对应的系统库和对应配置
文档地址：https://easyvaas.github.io/doc/chapter4/iOS.html
```

---

<h4 id='1.3'> 3. 推流模块中的连麦按钮为什么点击后无响应？ </h4>

```
首先需要确保 kAgoraId 已经正确填入，连麦需要两个streamer进行交互，demo中已经在直播播放器中集成了连麦的逻。
连麦流程：在推流端推流，提示“开始推流”后，通过另一台设备拉流观看直播，此时推流端、拉流端都开启连麦按钮即可进行连麦互动。

解：全局搜索 kAgoraId，将易视云提供的信息填入缺失的两处字符串中
```

---

<h4 id='1.4'> 4. demo中推流端点击vid会生成二维码，拉流端扫描后可以观看，必须要这样交互吗？ </h4>

```
demo中的二维码只是为了在拉流时不需要手动输入vid，实际使用过程中可结合自身情况使用

解：可直接使用vid
```

---

<h4 id='1.5'> 5. 项目可运行，但输入直播vid后，点击观看为何直接崩溃？ </h4>

```
/Users/xxx/detu-ijkmediaplayer/ijkmedia/ijksdl/ijksdl_mutex.c, line 63.
首先查看Xcode控制台输出，如果信息如上所示，那是因为同时导入了两个播放器导致的。
首先需要明确：是否需要VR播放功能？

如果需要VR播放功能，解：
TARGETS->Build Settings 搜索 other linker，添加 -all_load
如果不需要VR播放功能，解：
去除项目里的 EVVRFramework.framework、ios_player_dylib.framework
```

---

<h4 id='1.6'> 6. 我集成了VR播放功能，但为何无法播放从网上找的VR链接？ </h4>

```
可以先用VLC、ffplay等播放器确认URL可播放，如果资源来源于互联网视频，那么其为一个录播，demo默认勾选了[直播]，导致无法播放

解：播放配置选择[录播]，VR配置选择[VR视频]
```

---

<h4 id='1.7'> 7. Xcode运行后控制台输出：Reason: image not found </h4>

```
通常是因为使用了动态的 framework，但没有正确配置
解：将动态库引入到 TARGETS->General->Embedded Binaries 里

(iOS SDK中的动态库有两个：EVSocket.framework、ios_player_dylib.framework)
```

---

<h4 id='1.8'> 8. Xcode9运行后报错： "___llvm_profile_runtime", referenced from: </h4>

```
由于Xcode9使用新版本的LLVM，对内部模块进行了改动，出现了该问题
解：TARGETS->Build Settings 搜索 other linker，添加 -fprofile-instr-generate
```

---









