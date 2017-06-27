### 创建channel
#### 接口说明

| 名称 | 内容 | 
|:--|:--|
| 接口名称     | /channel/create | 
| 功能说明|   在服务端创建一个通道，客户端加入该通道，并基于该通道进行消息的收发    |
| 服务器地址| http://api.msg.easyvaas.com/v1/server/ |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| channel| 是      |   string | 所要创建的通道名称，注意全局唯一 |

* 请求示例

	```
	http://api.msg.easyvaas.com/v1/server/channel/create?appid=evdev&channel=test
	```

#### 接口应答

* 应答示例

```
{
	"state":0,
	"message":"ok",
	"content":{
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




