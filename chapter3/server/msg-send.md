### 发送消息
#### 接口说明

| 名称 | 内容 |
|:--|:--|
| 接口名称     | /msg/send |
| 功能说明|   发送消息   |
| 服务器地址| http://api.msg.easyvaas.com/v1/server/ |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| channel| 是      |   string | 所要创建的通道名称，注意全局唯一 |
| type      | 是 | String | 消息类型 |
| userdata      | 是 | String | 用户数据 |
| userid      | 否 | String | 用户id |
| level      | 否 | Int | 优先级[0,10]，值越大越重要，缺省为4 |
| save      | 否 | Bool | 是否保存，缺省为true |
| filter      | 否 | Bool | 是否关键词过滤，缺省为false |


* 请求示例

	```
	POST_URL
	http://112.126.85.64:8081/v1/server/msg/send?appid=yizhibo&channel=test&userid=xyk&type=msg&level=5&save=true&filter=false

    POST_BODY
    {"one":1}
	```

#### 接口应答

* 应答示例

```
{
  "state": 0,
  "message": "OK",
  "content": {
  }
}
```


#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 |
|:--|:--|
| 1     | 参数错误 |
| 2     | 系统内部错误 |
| 101   | appid错误 |
| 102   | channel不存在 |




