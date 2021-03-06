### 删除消息
#### 接口说明

| 名称 | 内容 | 
|:--|:--|
| 接口名称     | /msg/delete | 
| 功能说明|   删除消息   |
| 服务器地址| http://api.msg.easyvaas.com/v1/server/ |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| channel| 是      |   string | 所要创建的通道名称，注意全局唯一 |
| msgid      | 是 | String | 消息ID |

* 请求示例

	```
	http://api.msg.easyvaas.com/v1/server/msg/delete?appid=evdev&channel=test&msgid=18xSvU0ZP9K
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
 * 说明


#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 | 
|:--|:--| 
| 1     | 参数错误 | 
| 2     | 系统内部错误 |
| 101   | appid错误 |
| 102   | channel不存在 |




