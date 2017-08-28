## Easyvaas JS SDK 更新日志

* 版本 v1.2  2017-08-28

    *优化：*  
    
    1. 播放器更换新皮肤 
    2. 增加全屏和退出全屏事件
    3. 为了不出bug，差一点点就自杀祭天了
      
    *更新：*  
        
        1. 全屏事件
    
           ysyPlayer.onFullScreen(function(){
               console.log('全屏');
           });
           
        2. 退出全屏事件
    
           ysyPlayer.onQuitScreen(function(){
               console.log('退出全屏');
           });



* 版本 v1.1  2017-06-07   
    
    *优化：*  
    
    1. 录播视频控制条 
    2. 更新播放、暂停事件以及视频事件监听的方法  
    
    *更新：*  
    
    1. 播放事件

        el.onclick = function(){
            ysyPlayer.onPlay(function(){
                console.log('点击自定义按钮 播放视频');
            });
        }  

    2. 暂停事件
    
        el.onclick = function(){
            ysyPlayer.onPause(function(){
                console.log('点击自定义按钮 暂停视频');
            });
        }  

    3. 监听播放事件
      
        ysyPlayer.onPlayHandler(function(){
            console.log('监听视频播放');
        }) 

    4. 监听暂停事件
      
        ysyPlayer.onPauseHandler(function(){
            console.log('监听视频暂停');
        }) 

    5. 监听结束事件
      
        ysyPlayer.onEndedHandler(function(){
            console.log('监听视频结束');
        }) 

    6. 监听错误事件
      
        ysyPlayer.onErrorHandler(function(){
            console.log('监听视频错误');
        }) 
    
    *废弃：* 
    
    1. 监听播放事件
    
        ~~playHandler(){}~~
   
    2. 监听暂停事件
    
        ~~pauseHandler(){}~~
        
    3. 监听结束事件

        ~~endedHandler(){}~~
        
    4. 监听错误事件

        ~~errorHandler(){}~~

