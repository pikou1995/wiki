host:  
    http://192.168.2.130:8082  

headers:  
    X-User-Id  
    X-School-Id  


# 1. 获取社团列表
简要描述:
    获取学校内所有的社团

请求URL:
    /clubs/

请求方式:
    GET

参数：

| 参数名  |  必填  |  类型   |  默认值 |  说明  |
| ------ | ------ | ------ | ------ | ------ |
| group  |  是    | string |         | 请填 'admin' |
| status |  否    | int    |         | 社团的状态: -1 => 已拒绝, 0 => 待审核, 1 => 审核通过, 2 => 申请解散, 3 => 已解散 .不填则默认查询 0, 2 |
| page   |  否    | int    | 1       | 第几页 |
| per_page | 否   | int    | 10      | 每页多少个社团 |

返回数据类型:
    json

返回数据:

|  参数名 |  类型  |  说明    |
| ------ | ------ | ------ |
|  page  |  int   | 当前第几页 |
|  per_page | int |   一页多少个 |
|  total  | int   | 总共有多少个社团 |
|  items  | array | 社团列表 |

备注:
    一个社团数据示例(字段都有, 顺序不一定):
```js
{
    "id": 14,
    "avatar": "https://inschool-oss.incourse.com.cn/FthFwMG0CQIKgCEZqNxcPKwHMxzi.jpeg", // 社团封面
    "name": "学习大过天", //社团名称
    "introduction": "在一起学习, 从黑头到白头", // 社团介绍
    "tags": [],
    "members": [
        45
    ], // 社团成员, 数组长度就是现在社团有多少人. 数字为内部id, 不是user_id.
    "members_limit": 40, // 人员限制
    "status": 1, // 社团审核状态
    "school_id": 63,
    "user_id": 5193, // 创建人
    "created_at": "Thu, 29 Nov 2018 08:40:17 GMT"
}
```

# 2. 获取成员列表
简要描述:
    获取社团里面的所有成员

请求URL:
    /members/

请求方式:
    GET

参数：

| 参数名   |  必填 |  类型  |  默认值  |  说明  |
| ------- | ----- | ----- | ------- | ------ |
| club_id |  是   |  int  |         | 社团id |

返回数据类型:
    json

返回数据:
    [...members]

备注:
    一个成员member数据示例(字段都有, 顺序不一定):
```js
{
    "id": 45, // 上面提到的内部 id
    "user_id": 5193, // 用户id
    "club_id": 14, // 社团id
    "position": 2, // 职位 0 普通成员 1 副社长 2 社长 3 管理员
    "created_at": "Thu, 29 Nov 2018 08:40:17 GMT",
}
```


# 3. 获取申请详情
简要描述:
    查询社团申请的事件, 可以获取申请的信息

请求URL:
    /clubs/{club_id}/events/

请求方式:
    GET

参数：

|  参数名 | 必填 |  类型  | 默认值  |  说明  |
| ------ | --- | ----- | ------- | ------ |
|  type  |  否  |  int  |        | type 同 status, 0 => 申请创建, 2 => 申请解散. 不填则默认获取最近的一个请求事件. |

返回数据:
    [...events]

备注:
    一个事件数据示例(字段都有, 顺序不一定):
```js
{
    "id": 45, // 上面提到的内部 id
    "type": 0, // 0 => 申请创建, 2 => 申请解散
    "user_id": 5193, // 提出申请的用户id
    "club_id": 14, // 社团id
    "content": "申请的理由",
    "created_at": "Thu, 29 Nov 2018 08:40:17 GMT",
    // 已经处理过的事件
    "checked_at": "Fri, 30 Nov 2018 08:40:17 GMT", // 审核时间
    "checked_user": "Fri, 30 Nov 2018 08:40:17 GMT", // 审核人
    "checked_msg": "备注"
}
```

# 4. 审核社团
简要描述:
    审核创建社团

请求URL:
    /clubs/{club_id}/check/

请求方式:
    POST

参数：

|  参数名 | 必填  |  类型  |  默认值 |  说明   |
| ------ | ---- | ------ | ------ | ------ |
| event_id | 是  | int   |        | 事件id  |
| action |  是   | string |       | 'accepted' => 通过, 'rejected' => 不通过 |
| msg    |  否   | string |       | 备注信息 |

返回数据类型:
    json

返回数据:
    无

备注:
    状态码 204 表示成功
