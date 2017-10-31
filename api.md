## 目录

- [API接口签名规则](#api接口签名规则)

- [注册](#注册)

- [API方法返回值说明](#api方法返回值说明待更新)



## API接口签名规则

每次请求 API 接口时，均需要提供 3 个 HTTP Request Header，具体如下：

名称|类型|说明
---|---|---
AppToken | String | 开发者识别码。
Timestamp | String | 时间戳，从 1970 年 1 月 1 日 0 点 0 分 0 秒开始到现在的秒数。
Signature | String | 数据签名。

**Signature (数据签名)计算方法**：将AppToken（后台系统分配，暂为"1234567"）、Timestamp、"HiJoy"三个字符串按先后顺序拼接成一个字符串并进行 SHA1哈希计算。

## 注册

方法名：/register

url：http://123.207.189.56:8080/AndroidApp/register

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
username | String | 用户名。
password | String | 密码，md5散列过一次。
phone | String | 手机号，11位。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,400,403,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

注册流程为：调用第三方接口进行手机验证，验证成功后调用接口进行注册。

    注意：开发环境只支持100个注册名额，更多的需要申请。
    
## API方法返回值说明（待更新）

### HTTP状态码

status|描述|详细解释
---|---|---
200 | 成功 | 成功
400 | 错误请求 | 该请求是无效的，详细的错误信息会说明原因
403 | 服务器拒绝请求 | 被拒绝调用，详细的错误信息会说明原因
500 | 服务器内部错误 | 未知错误
