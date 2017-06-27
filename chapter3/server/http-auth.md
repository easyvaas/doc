## HTTP鉴权
易视云服务端API需要对每次接口访问进行身份验证，即每个请求都需要提供签名以验证用户身份，易视云API接口鉴权信息均在HTTP请求的Header中传输，Header字段描述如下：

| Headers | 含义 | 必选 |
|:--|:--|:--|
| X-EV-KEY      | AccessKey | 是 |
| X-EV-TIMESTAMP| 时间戳      |   是 |
| X-EV-SIGNATURE| 计算后的签名串 |    是 |
| X-EV-VERSION  | 算法版本，默认为1 | 是 |
| X-EV-NONCE    | 随机数      |   是 |
| X-EV-STAGE    | 默认为release,测试环境为test|    否 |
| X-EV-ACCEPT   | 参与运算的HEADERS，使用半角「,」分割，全部小写 | 否 |
| X-EV-ACCEPTQS | 参与运算的QueryString，使用半角「,」分割，全部小写|   否 |
| X-EV-ALGO     | hmac算法，枚举为: sha1, md5，默认sha1|    否 |

### 申请安全凭证
在第一次使用易视云服务端API之前，服务使用者需要申请安全凭证，安全凭证包括AccessKey和SecretKey，即AK和SK，其中AK用于标识API调用者身份，SK是用于加密签名字符串和服务器端验证签名字符串的密钥。用户应严格保管其SK，避免泄露。
### 生成签名串
* 生成待签名字符串
	* string_to_sign = HttpMethod + '\n' +  query + '\n' + Headers
	* HttpMethod = HTTP请求方法(大写)，例如GET POST PUT DELETE
	* query = key1 +'=' + value1 + '&' + key2 +'=' + value2 + '&' + ... + keyn +'=' + valuen。其中key1到keyn是经过字典排序的， value是utf-8再经过了url encode(rfc 3986)编码
	* Headers =  HeaderKey1 + ":" + HeaderValue1 + "\n" +
            HeaderKey2 + ":" + HeaderValue2 + "\n" +
            ...
            HeaderKeyN + ":" + HeaderValueN + "\n"。其中HeaderKey1到HeaderKeyN都是上表中定义的Headers，且使用大写，即X-EV-开头的(不包含X-EV-SIGNATURE项),经过字典排序。
* 计算签名
	* signature = hmac(string_to_sign,SecretKey)

### 错误码
服务端API接口访问返回json字符串中state不为0，说明接口访问出错，下表中列出的是鉴权错误码

| state | 错误描述 | 
|:--|:--|
| -1     | 未知错误 | 
| -2     | 系统错误 |
| -3     | 参数错误 |
| -4     | 应用不存在      |
| -5     | 应用处于非正常状态 |
| -6     | 鉴权参数错误 |
| -7     | 鉴权失败 |

