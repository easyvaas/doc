# JS消息SDK使用说明

## **快速开始**
本节提供快速集成易视云消息系统的步骤和示例代码。

### 配置项目
引入sdk相关的js文件后，依次进行如下配置：

### 示例代码
#### 在body闭合前引用js
```
<script type="text/javascript" src="//h5.api.easyvaas.com/ysyPlayer/ysyChat-1.1.min.js"></script>
<script type="text/javascript">
    //聊天初始化
    ysyChat.init({
        appid : 'appid',//application唯一标识码
        uid : 'uid',//用户uid
        topic : 'topic'  //聊天室id
    }, function(data) {
        //根据返回的data做dom操作
    }) 
        
    //发送消息
    ysyChat.sendMessage({
        content: '评论的文字',
        userdata: '自定义拓传的',
        // userdata: '{"nk":"昵称","logourl":"用户头像","uid":"用户的id"}',
        type: 'msg'//消息类型可分为msg和system
    });

    //获取历史消息
    var page = 0;
    var pageFlag = true;
    var count = 20;
    if(pageFlag==true){
        ysyChat.gethistorymsg({
            start: page,
            count: count,
            type: 'msg'
        },function(data){
            //根据返回的data 做相应的dom操作,且data.next字段为下次请求的start的值
            if(data.count<count){
                pageFlag=false;
            }
            page = data.next;
        })    
    }


</script>
```  



