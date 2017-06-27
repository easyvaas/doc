### 获取禁言列表
#### 接口说明

| 名称 | 内容 | 
|:--|:--|
| 接口名称     | /user/shutuplist | 
| 功能说明|   服务端通过该接口获取禁言列表    |
| 服务器地址| http://api.msg.easyvaas.com/v1/server/ |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| channel    | 否 | String | 所要获取禁言列表的channel，为空表示获取全局禁言列表 |
| start|  否     |   Int | 分页用,默认0|
| count|  否     |   Int | 分页用,默认10|

* 请求示例

	```
	http://api.msg.easyvaas.com/v1/server/user/shutuplist?appid=yizhibo&channel=test&start=0&count=10
	```

#### 接口应答

* 应答示例

```
{
  "state": 0,
  "message": "OK",
  "content": {
    "next": 2,
    "count": 4,
    "list": [
      {
        "channel": "",
        "userid": "shutupTester1"
      },
      {
        "channel": "",
        "userid": "shutupTester2"
      },
      {
        "channel": "test",
        "userid": "shutupTester3"
      },
      {
        "channel": "test",
        "userid": "shutupTester4"
      }
    ]
  }
}
```
 * 说明

| 参数名称 | 类型 |说明 |
|:--|:--|:--|
| next  | Int | 分页用，下次请求作为start参数传入 |
| count|   Int | 返回channel数目 |
| list|   Array | channel列表 |
| channel|   String | channel名称 |
| userid|   String | 被禁言用户id |


#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 | 
|:--|:--| 
| 1     | 参数错误 | 
| 2     | 系统内部错误 |
| 101   | appid错误 |
| 102   | channel不存在 |




