## 目录

- [API接口签名规则](#api接口签名规则)

- [销毁Session](#销毁session)

- [获取Token](#获取token)

- [注册](#注册)

- [API方法返回值说明](#api方法返回值说明待更新)

- [对象属性](#对象属性只列出可能用到的属性)

- [发送好友请求](#发送好友请求)

- [接受好友请求](#接受好友请求)

- [拒绝好友请求](#拒绝好友请求)

- [查找已发出的好友申请](#查找已发出的好友申请)

- [查找已收到的好友申请](#查找已收到的好友申请)

- [查找好友列表](#查找好友列表)

- [删除好友关系](#删除好友关系包括好友申请和好友拒绝记录)

- [更新任务描述图](#更新任务描述图)

- [发布任务](#发布任务)

- [删除任务](#删除任务)

- [领取任务](#领取任务)

- [放弃任务](#放弃任务)

- [更新类别、赏金、简介、详情](#更新类别赏金简介详情)

- [查找已发布的任务](#查找已发布的任务)

- [查找已领取的任务](#查找已领取的任务)

- [根据主键查找任务](#根据主键查找任务)

- [根据类别查找任务，一次五篇](#根据类别查找任务一次五篇)

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


## 更新任务描述图

方法名：/updateTaskImages

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskImages | MultipartFile | 图片组，png或jpg。
userId | String | 用户id。
taskId | int | 任务id。

编码格式：multipart/form-data

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 发布任务

方法名：/addTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
task | Task | 必需字段：userId,category,value,summary,details

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,403:信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json
如果有描述图，先调用此方法再调用updateTaskImages方法。

## 删除任务

方法名：/deleteTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
taskId | int | 任务id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 领取任务

方法名：/receiveTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
taskId | int | 任务id。
receivedUserId | String | 任务领取者id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 放弃任务

方法名：/abandonTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
taskId | int | 任务id。
receivedUserId | String | 任务领取者id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 更新类别、赏金、简介、详情

方法名：/updateTaskInf

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
task | Task | 必需字段：category,value,summary,details中一个或多个

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找已发布的任务

方法名：/findReleasedTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
releasedTasks | Task | Task数组，已发布的任务。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找已领取的任务

方法名：/findReceivedTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
receivedId | String | 任务领取者id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
receivedTasks | Task | Task数组，已领取的任务。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 根据主键查找任务

方法名：/findTask

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
taskId | int | 任务id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
task | Task | 任务。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 根据类别查找任务，一次五篇

方法名：/findTaskByCategory

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
pageId | int | 页数，最小为1。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
tasks | Task | Task数组，任务组。
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
password | String | 密码。
username | String | 用户名。
token | String | 融云token。
email | String | 邮箱。
head | String | 头像。

### Task
名称|类型|说明
---|---|---
userId | String | 用户编号，手机号，融云账号。
taskId | int | 任务编号。
receivedUserId | String | 任务领取者id。
category | String | 类别。
time | long | 发布时间。
status | int | 0代表未被接受，1代表已被接受。
value | float | 悬赏金。
summary | String | 简介。
image | String | 图片组链接，用分号隔开，如"/a/1;/a/2;"。
details | String | 详情。
