## 目录

- [API接口签名规则](#api接口签名规则)

- [销毁Session](#销毁session)

- [获取Token](#获取token)

- [注册](#注册)

- [API方法返回值说明](#api方法返回值说明待更新)

- [发送好友请求](#发送好友请求)

- [接受好友请求](#接受好友请求)

- [拒绝好友请求](#拒绝好友请求)

- [查找已发出的好友申请](#查找已发出的好友申请)

- [查找已收到的好友申请](#查找已收到的好友申请)

- [查找好友列表](#查找好友列表)

- [删除好友关系](#删除好友关系包括好友申请和好友拒绝记录)

- [对象属性](#对象属性只列出可能用到的属性)

### 建议设置一个全局变量保存url前缀，目前的url前缀：http://139.199.193.34/AndroidApp

## API接口签名规则

每次请求API接口前，均需要请求/getToken方法获取Token。
第一次获取Cookie时，可通过请求/getToken方法，以后每一次都通过上一次调用返回的响应头获取。
第一次发送请求时，Cookie可以随便设值。

**请求头**

一般情况下，每次请求API接口时，均需要提供 4 个 HTTP Request Header，具体如下：

名称|类型|说明
---|---|---
AC-Token | String | 32位，服务器生成。
Cookie | String | session识别标识。
AC-Signature | String | 数据签名。

获取Token时，需要提供 3 个 HTTP Request Header，具体如下：

名称|类型|说明
---|---|---
AC-Non | String | 随机数字符串，不限位数。
Cookie | String | session识别标识。
AC-Signature | String | 数据签名。

**Signature (数据签名)计算方法**：一般情况下，将Token、"HiJoy"两个字符串按先后顺序拼接成一个字符串并进行 SHA1哈希计算；获取Token时，将Non、"HiJoy"两个字符串按先后顺序拼接成一个字符串并进行 SHA1哈希计算。注意将加密得到的字符串转化为大写形式

**响应头**

名称|类型|说明
---|---|---
CheckInt | String | 可能取值：200：成功；1：请求头信息不全；2：Token失效；3：Token错误
Cookie | String | session识别标识。

## 销毁Session

方法名：/killSession

HTTP方法：Get

**参数**

无

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

此方法在用户关闭app时调用

## 获取Token

方法名：/getToken

HTTP方法：Get

**参数**

无

**返回值**

名称|类型|说明
---|---|---
token | String | 服务器随机生成的32位字符串。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 注册

方法名：/register

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
status | int | 可能取值：200；400：信息不全，无法注册；4031：5秒内不能重复注册；4032：账号已存在，不能重复注册；4033：注册人数超过限制；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

注册流程为：调用第三方接口进行手机验证，验证成功后调用接口进行注册。
同一用户10秒内不能注册第二次。

    注意：开发环境只支持100个注册名额，更多的需要申请。

## 发送好友请求

方法名：/friendApplication

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
applicantId | String | 发送人id。
targetId | String | 接收人id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；4031：已经发送过请求，请等待回应；4032：已经添加过好友；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 接受好友请求

方法名：/addFriend

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
applicantId | String | 发送人id。
targetId | String | 接收人id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；4031：请先发送好友申请；4032：已经添加过好友，不能重复添加；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 拒绝好友请求

方法名：/refuseFriend

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
applicantId | String | 发送人id。
targetId | String | 接收人id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；4031：请先发送好友申请；4032：已经同意此申请，无法拒绝；4033：已经拒绝过此申请；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找已发出的好友申请

方法名：/findFriendsApplicationByMe

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
applicantId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
applicationByMe | 对象集合 | User列表
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找已收到的好友申请

方法名：/findFriendsApplicationByOthers

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
applicantId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
applicationByOthers | 对象集合 | User列表
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找好友列表

方法名：/findFriends

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
friendList | 对象集合 | User列表
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除好友关系（包括好友申请和好友拒绝记录）

方法名：/deleteFriend

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId1 | String | 用户id。
userId2 | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## API方法返回值说明（待更新）

### HTTP状态码

status|描述|详细解释
---|---|---
200 | 成功 | 成功。
400 | 错误请求 | 该请求是无效的，详细的错误信息会说明原因。
403+? | 服务器拒绝请求 | 被拒绝调用，详细的错误信息会说明原因,?为数字，如4031,4032。
500 | 服务器内部错误 | 未知错误。

## 对象属性（只列出可能用到的属性）

### User

名称|类型|说明
---|---|---
id | String | 用户编号，手机号，融云账号。
username | String | 用户名。
token | String | 融云token。
email | String | 邮箱。
head | String | 头像。
