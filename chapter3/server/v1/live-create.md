### 创建流
#### 接口说明

| 名称 | 内容 |
|:--|:--|
| 接口名称     | /live/create |
| 功能说明|   在服务端创建一个流，使用者在自己实现的视频业务后台中调用，获取流id、key、推流地址    |
| 服务器地址| http://video.api.easyvaas.com/v1 |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| repush| 否      |   int | 流是否支持重推，默认为1-支持重推，0-不支持重推 |
| delay| 否 |    int | 异常流自动退出时间，默认是60s， 0是不自动退出|
| record| 否 |    int | 是否开启录制，默认为0-不开启录制，1-开启录制|

* 请求示例

	```
	http://video.api.easyvaas.com/v1/live/create?appid=evdev
	```

#### 接口应答

* 应答示例

```
{
	"state":0,
	"message":"ok",
	"content":{
		"addr":"rtmp:\/\/wspush.easyvaas.com\/live\/LjkVa686hM11UKEK?key=5twfPVr222IWsn78K0FZ3qr6ch8oFRy7g8L9DICUQqB54yLiYejVXxF3JnvC",
		"key":"yb9sNbznx89U1N8e22Rd2uInGquG5FmJ9zVSnwZm7SJd1ylrTElE06h33BFF",
		"lid":"LjkVa686hM11UKEK"
	}
}
```

* 说明

| 参数名称 | 类型 |说明 |
|:--|:--|:--|
| lid  | String | 创建的流id |
| key|   String | key值 |
| addr|   String | 推流地址 |

#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 |
|:--|:--|
| 1     | 参数错误 |
| 2     | 系统内部错误 |
| 102   | appid错误 |


