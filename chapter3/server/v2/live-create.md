### 直播创建
#### 接口说明

| 名称 | 内容 |
|:--|:--|
| 接口名称     | /live/create |
| 功能说明|   直播创建  |
| 服务器地址| http://video.api.easyvaas.com/v2/server |
| 请求方式| GET |

#### 参数说明

| 参数名称        | 必填           | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| record      | 否 | bool | 是否支持录制，默认支持 |
| cdn      | 否 | String | 支持cdn厂商的缩写，默认ws |
| repush      | 否 | bool | 是否支持重推，默认不支持 |
| validtm      | 否 | long | 流的有效时间(默认606024) |
| billid      | 否 | String | 计费id 默认为空 |


* 请求示例

	```
	http://online.video.easyvaas.com/v2/server/live/create?appid=x

	```

#### 接口应答

* 应答示例

```
{
  "state": 0,
  "message": "OK",
  "content": {
    "lid": "xxx"
  }
}
```

* 返回参数说明

| 参数名称        | 类型 |说明 |
|:--|:--|:--|
|lid |list | 生成的流id|


#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state        | 错误描述           |
|:--|:--|
| 1     | 参数错误 |
| 2     | 系统内部错误 |
| 102   | appid错误 |
