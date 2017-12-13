## 目录

- [API接口签名规则](#api接口签名规则)

- [销毁Session](#销毁session)

- [获取Token](#获取token)

- [API方法返回值说明](#api方法返回值说明待更新)

- [对象属性](#对象属性只列出可能用到的属性)

### 用户

- [注册](#注册)

- [登录](#登录)

- [查找用户信息](#查找用户信息)

- [查找所有校区](#查找所有校区)

- [更新用户的常驻地址](#更新用户的常驻地址)

- [转账](#转账)

### 服务

- [更新用户订阅的服务](#更新用户订阅的服务)

- [查找用户订阅的服务](#查找用户订阅的服务)

- [根据serviceId查找服务](#根据serviceid查找服务)

- [根据name查找服务](#根据name查找服务)

- [根据category查找服务](#根据category查找服务)

### 好友关系

- [发送好友请求](#发送好友请求)

- [接受好友请求](#接受好友请求)

- [拒绝好友请求](#拒绝好友请求)

- [查找已发出的好友申请](#查找已发出的好友申请)

- [查找已收到的好友申请](#查找已收到的好友申请)

- [查找好友列表](#查找好友列表)

- [删除好友关系](#删除好友关系包括好友申请和好友拒绝记录)

### 任务

- [添加任务描述图](#添加任务描述图)

- [删除任务描述图](#删除任务描述图)

- [发布任务](#发布任务)

- [删除任务](#删除任务)

- [领取任务](#领取任务)

- [放弃任务](#放弃任务)

- [更新类别、赏金、简介、详情、地址](#更新类别赏金简介详情地址)

- [查找已发布的任务](#查找已发布的任务)

- [查找已领取的任务](#查找已领取的任务)

- [根据主键查找任务](#根据主键查找任务)

- [根据校区、类别、状态查找任务，一次五篇](#根据校区类别状态查找任务一次五篇)

- [根据任务简介模糊查找任务](#根据任务简介模糊查找任务)

- [确认任务完成](#确认任务完成)

### 任务评论

- [发布任务评论](#发布任务评论)

- [删除任务评论](#删除评论)

- [查找任务的单条评论](#查找任务的单条评论)

- [查找任务的所有评论](#查找任务的所有评论)

### 任务评论的回复

- [发布回复](#发布回复)

- [删除回复](#删除回复)

- [查找单条评论的单条回复](#查找单条评论的单条回复)

- [查找单条评论的所有回复](#查找单条评论的所有回复)

### 任务收藏

- [添加收藏](#添加收藏)

- [删除收藏](#删除收藏)

- [查找收藏状态](#查找收藏状态)

### 朋友圈

- [添加说说描述图](#添加说说描述图)

- [删除说说描述图](#删除说说描述图)

- [发布说说](#发布说说)

- [删除说说](#删除说说)

- [更新说说文字内容](#更新说说文字内容)

- [查找单篇说说](#查找单篇说说)

- [查找用户已发布的说说](#查找用户已发布的说说)

- [查找校区内的说说，一次五篇](#查找校区内的说说一次五篇)

### 朋友圈收藏

- [添加收藏](#添加收藏)

- [删除收藏](#删除收藏)

- [查找收藏状态](#查找收藏状态)

### 朋友圈评论

- [发布评论](#发布评论)

- [删除评论](#删除评论)

- [查找说说所有的评论](#查找说说所有的评论)

- [查找单条评论](#查找单条评论)

### 地址

- [添加地址](#添加地址)

- [删除地址](#删除地址)

- [更新地址](#更新地址)

- [查找单条地址](#查找单条地址)

- [查找用户所有地址](#查找用户所有地址)

### 建议设置一个全局变量保存url前缀，目前的url前缀：http://139.199.193.34/AndroidApp

## API接口签名规则

每次请求API接口前，均需要请求/getToken方法获取Token。
第一次获取Cookie时，可通过请求/getToken方法，以后每一次都通过上一次调用返回的响应头获取。
第一次发送请求时，Cookie可以随便设值。

**请求头**

一般情况下，每次请求API接口时，均需要提供 3 个 HTTP Request Header，具体如下：

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

## 用户

## 注册

方法名：/register

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
username | String | 用户名。
password | String | 密码，md5散列过一次。
phone | String | 手机号，11位。
campus | String | 校区。

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

## 登录

方法名：/login

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
phone | String | 用户id。
password | String | 密码，md5散列过一次。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200：验证成功；4031：用户名或密码不能为空；4032：账号不存在，请先注册；4033：密码错误；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找用户信息

方法名：/findUserById

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
id | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
user | User | 用户信息。
status | int | 可能取值：200，500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找所有校区

方法名：/findCampuses

HTTP方法：Post

**参数**

无

**返回值**

名称|类型|说明
---|---|---
campuses | String | String集合，匹配的校区信息。
status | int | 可能取值：200，500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 更新用户的常驻地址

方法名：/updateUserAddressId

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
addressId | int | 地址id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；403：信息不全，拒绝请求；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 转账

方法名：/transferAccounts

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 转账发起人id。
toUserId | String | 转账目标id。
transferAmount | BigDecimal | 转账金额。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
wallet | BigDecimal | 账户余额。
status | int | 可能取值：200；4031：信息不全，拒绝请求；4032：转账发起人或转账目标不存在；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 服务

## 更新用户订阅的服务

方法名：/updateUserServiceIds

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
serviceIds | String | 用户订阅的服务的id组，用";"隔开，如"1;2;3;"。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；403：信息不全，拒绝请求；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找用户订阅的服务

方法名：/findServicesByUserId

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
services | Service | Service集合，用户订阅的服务。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 根据serviceId查找服务

方法名：/findServiceByServiceId

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
serviceId | int | 服务id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
service | Service | 服务。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 根据name查找服务

方法名：/findServicesByName

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
name | String | 服务名称。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
services | Service | Service集合，服务。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 根据category查找服务

方法名：/findServicesByCategory

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
category | String | 服务类别。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
services | Service | Service集合，服务。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json


## 好友关系

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

## 任务

## 添加任务描述图

方法名：/addTaskImages

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskImages | MultipartFile | 图片组，png或jpg，必须是用户新添加的图片（之前未上传到服务器的）。
userId | String | 用户id。
taskId | int | 任务id。

编码格式：multipart/form-data

**返回值**

名称|类型|说明
---|---|---
size | int | 上传成功的图片数。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除任务描述图

方法名：/deleteTaskImages

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
remainingImages | String | 剩余的图片链接，分号隔开，如："15157576980101511622273271.png;15157576980111511622273271.png;"，必须是用户之前已上传到服务器的。
deletedImages | String | 删除的图片链接，分号隔开，必须是用户之前已上传到服务器的。
userId | String | 用户id。
taskId | int | 任务id。

编码格式：application/x-www-form-urlencoded

如果用户在编辑完成时，既有删除操作又有添加操作，先调用此方法删除之前已上传的图片中用户选择删除的，再调用addTaskImages方法上传用户新添加的图片。

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
task | Task | 必需字段：userId,category,value,summary,details；不能为空，NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
taskId | int | 任务id。
status | int | 可能取值：200,403:信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

如果有描述图，先调用此方法再调用addTaskImages方法。

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

## 更新类别、赏金、简介、详情、地址

方法名：/updateTaskInf

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
task | Task | 必需字段：category,value,summary,details,addressId；不能为空，NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

每次调用此方法后如果还有描述图（不管描述图是否有增删，只要还有一张），需要再调用updateTaskImages方法

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

## 根据校区、类别、状态查找任务，一次五篇

方法名：/findTasks

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
category | String | 类别。
campus | String | 校区。
status | int | 状态码。0代表未被接受，1代表正在进行，2代表已完成。
pageId | int | 页数，最小为1。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
tasks | Task | Task数组，任务组。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 根据任务简介模糊查找任务

方法名：/researchTasks

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
campus | String | 校区。
summary | String | 任务简介。
pageId | int | 页数，最小为1。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
tasks | Task | Task数组，任务组。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 确认任务完成

方法名：/finishTask

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
status | int | 可能取值：200,403：信息不全，拒绝请求；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 任务评论

## 发布任务评论

方法名：/addTaskComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskComment | TaskComment | 任务评论扩展对象，必需字段：taskUserId,taskId,commentUserId,content。

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
commentId | int | 评论id。
status | int | 可能取值：200,403：信息不全，拒绝请求；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除任务评论

方法名：/deleteTaskComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找任务的单条评论

方法名：/findTaskComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
comment | TaskCommentExtend | 任务评论。
replys | TaskReplyExtend | 任务评论对应的回复集合。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找任务的所有评论

方法名：/findTaskComments

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
commentsAndReplys | Map<String, Object> | Map集合，每个元素包含:"comment":单条任务评论；"replys":评论对应的回复集合。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 任务收藏

## 添加收藏

方法名：/addTaskCollection

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskCollection | TaskCollection | 必需字段：taskUserId，userId，taskId；不能为空，NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,403:信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除收藏

方法名：/deleteTaskCollection

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
userId | String | 收藏者id。
taskId | String | 任务id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找收藏状态

方法名：/findTaskCollection

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
userId | String | 收藏者id。
taskId | String | 任务id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
check | boolean | 如果用户收藏了任务，为true，否则为false。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 任务评论的回复

## 发布回复

方法名：/addTaskReply

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskReply | TaskReply | 任务回复对象。必需字段：taskUserId,taskId,commentUserId,commentId,replyUserId,content;不能为空，null

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
replyId | int | 回复id。
status | int | 可能取值：200；403:"信息不全，拒绝请求"；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除回复

方法名：/deleteTaskReply

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | String | 评论id。
replyUserId | String | 回复发布者id。
replyId | int | 回复id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200；403:"信息不全，拒绝请求"；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找单条评论的单条回复

方法名：/findTaskReply

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | String | 评论id。
replyUserId | String | 回复发布者id。
replyId | int | 回复id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
reply | TaskReplyExtend | 任务回复扩展对象。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找单条评论的所有回复

方法名：/findTaskReplyByComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | String | 评论id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
replys | TaskReplyExtend | 任务回复扩展对象集合。
status | int | 可能取值：200；500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 朋友圈

## 添加说说描述图

方法名：/addArticleImages

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleImages | String | 图片组，png或jpg，必须是用户新添加的图片（之前未上传到服务器的）。
userId | String | 用户id。
articleId | int | 说说id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
size | int | 上传成功的图片数。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除说说描述图

方法名：/deleteArticleImages

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
remainingImages | String | 剩余的图片链接，分号隔开，如："15157576980101511622273271.png;15157576980111511622273271.png;"，必须是用户之前已上传到服务器的。
deletedImages | String | 删除的图片链接，分号隔开，必须是用户之前已上传到服务器的。
userId | String | 用户id。
articleId | int | 说说id。

编码格式：application/x-www-form-urlencoded

如果用户在编辑完成时，既有删除操作又有添加操作，先调用此方法删除之前已上传的图片中用户选择删除的，再调用addArticleImages方法上传用户新添加的图片。

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 发布说说

方法名：/addArticle

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
article | Article | 必需字段：userId,content,time,image;不为空，不为NULL

编码格式：application/json 

**返回值**

名称|类型|说明
---|---|---
articleId | int | 说说id。
status | int | 可能取值：200,403：信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json 

## 删除说说

方法名：/deleteArticle

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
articleId | int | 说说id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json 

## 更新说说文字内容

方法名：/updateArticleInf

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
articleId | int | 说说id。
content | String | 说说文字内容。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,403：信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json 

## 查找单篇说说

方法名：/findArticle

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
articleId | int | 说说id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
article | Article | 说说。
comments | Comment | 评论集合。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json 

## 查找用户已发布的说说

方法名：/findArticleByUserId

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
articlesAndComments | Map | Map集合，每个Map元素包含："article"：Article对象，说说；"comments":Comment对象集合，说说对应的评论。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json 

## 查找校区内的说说，一次五篇

方法名：/findArticleByCampus

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
campus | String | 用户搜索时输入的信息。
pageId | int | 页数，最小为1。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
articlesAndComments | Map | Map集合，每个Map元素包含："article"：Article对象，说说；"comments":Comment对象集合，说说对应的评论。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json 

## 朋友圈收藏

## 添加收藏

方法名：/addArticleCollection

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleCollection | ArticleCollection | 必需字段：articleUserId，articleId，userId；不能为空，NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,403:信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除收藏

方法名：/deleteArticleCollection

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
userId | String | 收藏者id。
articleId | String | 说说id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找收藏状态

方法名：/findArticleCollection

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
userId | String | 收藏者id。
articleId | String | 说说id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
check | boolean | 如果用户收藏了说说，为true，否则为false。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 朋友圈评论

## 发布评论

方法名：/addComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
comment | Comment | 必需字段:articleUserId,articleId,commentUserId,content;不为空，不为NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
commentId | int | 评论id。
status | int | 可能取值：200,403：信息不全，拒绝请求，500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除评论

方法名：/deleteComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
articleId | int | 说说id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找说说所有的评论

方法名：/findCommentsByArticle

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
articleId | int | 说说id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
comments | Comment | 对象集合，说说的所有评论。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找单条评论

方法名：/findComment

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
articleId | int | 说说id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
comment | Comment | 评论。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 地址

## 添加地址

方法名：/addAddress

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
address | Address | 必需字段:userId,address,name,contactWay,contactNumber;不为空，不为NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
addressId | int | 地址id。
status | int | 可能取值：200,403：信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 删除地址

方法名：/deleteAddress

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
addressId | int | 地址id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 更新地址

方法名：/updateAddress

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
address | Address | 必需字段:userId,addressId,address,name,contactWay,contactNumber;不为空，不为NULL

编码格式：application/json

**返回值**

名称|类型|说明
---|---|---
status | int | 可能取值：200,403：信息不全，拒绝请求,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找单条地址

方法名：/findAddress

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。
addressId | int | 地址id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
address | Address | 地址。
status | int | 可能取值：200,500。
exception | String | 错误信息，status为200时无此字段。

编码格式：application/json

## 查找用户所有地址

方法名：/findAddresses

HTTP方法：Post

**参数**

名称|类型|说明
---|---|---
userId | String | 用户id。

编码格式：application/x-www-form-urlencoded

**返回值**

名称|类型|说明
---|---|---
addresses | Address | 对象集合，用户所有的地址。
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
campus | String | 校区。
addressId | int | 常用地址id，如果暂无常驻地址，此字段值为0。
serviceIds | String | 服务编号组，用分号隔开，如"1;2;3;"。
wallet | BigDecimal | 账户余额。

### Task
名称|类型|说明
---|---|---
userId | String | 用户编号，手机号，融云账号。
taskId | int | 任务编号。
receivedUserId | String | 任务领取者id。
category | String | 类别。
time | long | 发布时间。
status | int | 0代表未被接受，1代表正在进行，2代表已完成。
value | BigDecimal | 悬赏金。
summary | String | 简介。
image | String | 图片组链接，用分号隔开，如"/a/1;/a/2;"。
details | String | 详情。
addressId | int | 地址id。

### TaskComment
名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。
content | String | 评论内容。
time | long | 发布时间。

### TaskCommentExtend
名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。
content | String | 评论内容。
time | long | 发布时间。
username | String | 评论发布者用户名。
head | String | 评论发布者的头像。

### TaskReply
名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。
replyUserId | String | 回复发布者id。
replyId | int | 回复id。
content | String | 评论内容。
time | long | 发布时间。
String | toUserId | 若回复的是评论主体，则此字段为空或null；若回复的是另一条回复，此字段为另一条回复发布人的id。

### TaskReplyExtend
名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
taskId | int | 任务id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。
replyUserId | String | 回复发布者id。
replyId | int | 回复id。
content | String | 评论内容。
time | long | 发布时间。
String | toUserId | 若回复的是评论主体，则此字段为空或null；若回复的是另一条回复，此字段为另一条回复发布人的id。
String | username | replyUserId对应的用户名。
String | toUsername | toUserId对应的用户名。

### TaskCollection
名称|类型|说明
---|---|---
taskUserId | String | 任务发布者id。
userId | String | 收藏者id。
taskId | String | 任务id。

### Article
名称|类型|说明
---|---|---
userId | String | 用户id。
articleId | int | 说说id。
content | String | 文字内容。
image | String | 图片组链接，用分号隔开，如"/a/1;/a/2;"。
time | long | 发布时间。

### ArticleCollection
名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
articleId | int | 说说id。
userId | String | 收藏者id。

### Comment
名称|类型|说明
---|---|---
articleUserId | String | 说说发布者id。
articleId | int | 说说id。
commentUserId | String | 评论发布者id。
commentId | int | 评论id。
toUserId | String | 所回复的用户的id，如果此评论不是回复，此字段为null或""
content | String | 评论内容。
time | long | 发布时间。

### Address
名称|类型|说明
---|---|---
userId | String | 用户id。
addressId | int | 地址id。
address | String | 地址。
name | String | 联系人姓名。
contactWay | String | 联系方式。
contactNumber | String | 联系号码。

### Service
名称|类型|说明
---|---|---
serviceId | int | 服务id。
name | String | 服务名称。
summary | String | 服务简介。
icon | String | 服务图标。
developer | String | 开发者。
school | String | 学校。
category | String | 服务类别。
usedNumber | int | 使用量，热度。
