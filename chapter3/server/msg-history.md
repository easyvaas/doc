### 消息历史记录
#### 接口说明

| 名称 | 内容 |
|:--|:--|
| 接口名称     | /msg/history |
| 功能说明|   获取某个channel的消息历史记录    |
| 服务器地址| http://api.msg.easyvaas.com/v1/server/ |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| channel| 是      |   string | 所要创建的通道名称，注意全局唯一 |
| type      | 否 | String | 消息类型，逗号间隔 |
| start|  否     |   Int | 分页用,默认0|
| count|  否     |   Int | 分页用,默认10|

* 请求示例

	```
	http://api.msg.easyvaas.com/v1/server/msg/history?appid=evdev&channel=test
	```

#### 接口应答

* 应答示例

```
{
  "state": 0,
  "message": "OK",
  "content": {
    "next": 191202,
    "count": 10,
    "list": [
      {
        "msgid": "18xSvU0ZP9K",
        "userid": "system",
        "create_time": "2017-06-12T21:50:33+08:00",
        "type": "msg",
        "content": "{}"
      }
   }
}
```
 * 说明

| 参数名称 | 类型 |说明 |
|:--|:--|:--|
| next  | Int | 分页用，下次请求作为start参数传入 |
| count|   Int | 返回channel数目 |
| list|   Array | 消息列表 |
| msgid|   String | 消息ID |
| userid|   String | 发送者ID |
| create_time|   String | 创建时间 |
| type|   String | 消息类型 |
| content|   String | 消息内容 |


#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 |
|:--|:--|
| 1     | 参数错误 |
| 2     | 系统内部错误 |
| 101   | appid错误 |
| 102   | channel不存在 |




