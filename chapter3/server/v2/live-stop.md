### 直播停止
#### 接口说明

| 名称 | 内容 |
|:--|:--|
| 接口名称     | /live/stop |
| 功能说明|   直播恢复  |
| 服务器地址| http://video.api.easyvaas.com/v2/server |
| 请求方式| GET |

#### 参数说明

| 参数名称        | 必填           | 类型 |说明 |
|:--|:--|:--|:--|
| appid      | 是 | String | 服务使用者对应的appid |
| lid      | 是 | String | 流id |

* 请求示例

	```
	http://online.video.easyvaas.com/v2/server/live/stop?appid=x&lid=x

	```


#### 接口应答

* 应答示例

```
{
  "state": 0,
  "message": "ok",
  "content": {


  }
}
```

* 返回参数说明

| 参数名称        | 类型 |说明 |
|:--|:--|:--|


#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state        | 错误描述           |
|:--|:--|
| 1     | 参数错误 |
| 2     | 系统内部错误 |
| 102   | appid错误 |
| 210   | 流不存在|

