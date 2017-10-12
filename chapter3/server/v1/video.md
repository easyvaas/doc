## 视频服务API
易视云视频服务API提供视频相关的API接口，帮助服务使用者快速搭建自己的视频后台服务。
### 调用方式
#### 服务域名
	video.api.easyvaas.com
#### 服务版本号
	v1
#### 通信协议
	HTTP
#### 请求方式
	GET
#### 字符编码
	UTF-8
#### 返回值定义
服务端API的返回值统一为以下格式的json串

```
{state : 0,  message : '', content :  {}}
```

* state, 接口返回的状态码，0-接口访问正常，其他非零值表示接口访问出错
* message, Human-Readable的错误描述，没有错误时为空。
* content, 业务数据结构

### 服务端API概览

| 接口功能 | 接口名称 |
|:--|:--|
| 创建流     | /live/create |
| 禁播流| /live/stop      |
| 查询流列表| /live/list |



