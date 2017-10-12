# JS播放端SDK使用说明

## **快速开始**
本节提供快速集成易视云WEB播放端SDK的步骤和示例代码。具体可 [参考Demo](http://static.easyvaas.com/sdk/h5/demo/demo.html) 中相关代码。

### 兼容性
主流pc浏览器，移动端浏览器，腾讯X5内核浏览器（手机微信，手机QQ）

### 配置项目
引入sdk相关的js文件后，依次进行如下配置：

### 示例代码

#### 在html中创建一个播放器容器

    <div id="player"></div>

#### 在body闭合前引用js，并且创建播放器
```
<script type="text/javascript" src="//static.easyvaas.com/sdk/h5/player/ysyPlayer-1.1.min.js"></script>
<script type="text/javascript">
    var ysyPlayer = new ysyPlayer({
        playerid:'player',//播放器id
        appid:'xxxxxxxxx', //appid
        lid:'QwEZQm1dHNzBiqJV',//视频流名称
        live:false,//是否是直播
        setOptions:{
            height:'100%',//可自定义高度，默认100%是原视频流高度，如果定义容器player定义了高度，100%则是继承player容器的高度
            width:'100%',//可自定义高度，默认100%是浏览器宽度，如果定义容器player定义了宽度，100%则是继承player容器的宽度
            poster:'#', //可设定视频封面,如不设定删掉此参数即可
            autostart:0//默认不自动播放参数为0,1为自动播放 
            errorMsg:'当前没有直播' //获取视频流失败显示的错误提示 
        }
    });
</script>
```
## **API列表**

* [API列表](player/api.md)



