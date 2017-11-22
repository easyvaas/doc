### 查询cdn线路
#### 接口说明

| 名称 | 内容 |
|:--|:--|
| 接口名称     | /cdn/support |
| 功能说明|   查询服务支持cdn线路    |
| 服务器地址| http://video.api.easyvaas.com/v2/server |
| 请求方式| GET |

#### 参数说明

| 参数名称 | 必填 | 类型 |说明 |
|:--|:--|:--|:--|

* 请求示例

	```
	http://video.api.easyvaas.com/v2/server/cdn/support
	```

#### 接口应答

* 应答示例

```
{
  "state": 0,
  "message": "OK",
  "content": {
    "list": [
      {
        "cdn": "ws",
        "desc": "网宿"
      }
    ]
  }
}
```

* 说明

| 参数名称 | 类型 |说明 |
|:--|:--|:--|
| cdn  | String | cdn缩写 |
| desc|   String | cdn名称 |

#### 错误码说明
接口访问返回json字符串中state不为0，接口访问出错，错误说明如下

| state | 错误描述 |
|:--|:--|
| 1     | 参数错误 |
| 2     | 系统内部错误 |
