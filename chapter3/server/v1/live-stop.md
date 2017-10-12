### 禁播流
#### 接口说明

| 名称 | 内容 | 
|:--|:--| 
| 接口名称     | /live/stop | 
| 功能说明|   服务端关闭一个流，使用者在自己实现的视频业务后台中调用，可以对特定流进行禁播操作    |
| 服务器地址| http://video.api.easyvaas.com/v1 |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| lid| 是      |   String | 流id |
| key| 是 |    String | 创建流时产生的key值|

* 请求示例

	```
	http://video.api.easyvaas.com/v1/live/stop?appid=evdev&lid=LjkVa686hM11UKEK&key=yb9sNbznx89U1N8e22Rd2uInGquG5FmJ9zVSnwZm7SJd1ylrTElE06h33BFF
	```

#### 接口应答

* 应答示例

```
{
	"state":0,
	"message":"ok",
	"content":{}
}
```

#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 | 
|:--|:--| 
| 1     | 参数错误 | 
| 2     | 系统内部错误 |
| 101   | 需要禁播的流不存在 |
| 102   | appid错误 |
| 103   | key错误 |




