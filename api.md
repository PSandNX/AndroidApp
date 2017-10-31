## API 接口签名规则
每次请求 API 接口时，均需要提供 4 个 HTTP Request Header，具体如下：
|名称 | 类型 | 说明
|---|---|---
|AppToken | String | 开发者识别码。
|Timestamp | String | 时间戳，从 1970 年 1 月 1 日 0 点 0 分 0 秒开始到现在的秒数。
|Signature | String | 数据签名。
**Signature (数据签名)计算方法**：将AppToken（后台系统分配，暂为"1234567"）、Timestamp(时间戳)、"HiJoy"三个字符串按先后顺序拼接成一个字符串并进行 SHA1哈希计算。
| 水果        | 价格    |  数量  |
| --------   | -----:   | :----: |
| 香蕉        | $1      |   5    |
| 苹果        | $1      |   6    |
| 草莓        | $1      |   7    |