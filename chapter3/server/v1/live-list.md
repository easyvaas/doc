### 查询流列表
#### 接口说明

| 名称 | 内容 | 
|:--|:--| 
| 接口名称     | /live/list | 
| 功能说明|   服务使用者在自己实现的视频业务后台中调用该接口获取当前直播流列表    |
| 服务器地址| http://video.api.easyvaas.com/v1 |
| 请求方式| GET |

#### 参数说明

| 参数名称        | 必填           | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |

* 请求示例

	```
	http://video.api.easyvaas.com/v1/live/list?appid=evdev
	```

#### 接口应答

* 应答示例

```
{
	"state":0,
	"message":"ok",
	"content":{
		"streams":[
        {
            "lid":"123456",
            "state":1
        }，
        {
            "lid":"1234567",
            "state":1
        }，
        {
            "lid":"1234568",
            "state":1
        }
    ]
	}
}
```

* 返回参数说明

| 参数名称        | 类型 |说明 |
|:--|:--|:--|
| streams  | List | 直播流列表 |
| streams[].lid|   String | 直播流id |
| streams[].state|   int | 直播流状态，详细见直播流状态说明 |

* 直播流状态说明

| 状态值       | 说明 |
|:--|:--|
| -1  |  EXITE，流退出 |
| 0|  PREPAREE，流开始前 |
| 1|STREAME，流开始 |
| 2  |  BREAKE，流断开 |
| 3|  PAUSEE，流暂时停止推送|

#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state        | 错误描述           | 
|:--|:--| 
| 1     | 参数错误 | 
| 2     | 系统内部错误 |
| 102   | appid错误 |




