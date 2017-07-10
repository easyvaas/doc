# 播放器相关事件API

## **快速开始**
本节提供快速集成易视云WEB播放端SDK的步骤和示例代码。具体可 [参考Demo](http://static.easyvaas.com/sdk/h5/demo/player.html) 中相关代码。

### 示例代码

#### 播放事件

    el.onclick = function(){
        ysyPlayer.onPlay(function(){
            console.log('点击自定义按钮 播放视频');
        });
    }  

#### 暂停事件
    
    el.onclick = function(){
        ysyPlayer.onPause(function(){
            console.log('点击自定义按钮 暂停视频');
        });
    }  

#### 监听播放事件
      
    ysyPlayer.onPlayHandler(function(){
        console.log('监听视频播放');
    }) 

#### 监听暂停事件
      
    ysyPlayer.onPauseHandler(function(){
        console.log('监听视频暂停');
    }) 

#### 监听结束事件
      
    ysyPlayer.onEndedHandler(function(){
        console.log('监听视频结束');
    }) 

#### 监听错误事件
      
    ysyPlayer.onErrorHandler(function(){
        console.log('监听视频错误');
    })                 

## **快速开始**

* [快速开始](../player.md)

