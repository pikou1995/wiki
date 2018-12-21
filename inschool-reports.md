host:

    http://192.168.2.130:8080

headers:

    X-User-Id

    X-School-Id


# 1. 获取举报的微博

简要描述:
    获取举报的微博

请求URL:

    /reports/blogs/

请求方式:

    GET

参数：

| 参数名  |  必填  |  类型   |  默认值 |  说明  |
| ------ | ------ | ------ | ------ | ------ |
| page   |  否    | int    | 1       | 第几页 |
| per_page | 否   | int    | 10      | 每页多少个微博 |

返回数据类型:

    json

返回数据:

|  参数名 |  类型  |  说明    |
| ------ | ------ | ------ |
|  page  |  int   | 当前第几页 |
|  per_page | int | 一页多少个 |
|  total  | int   | 总共有多少个微博 |
|  items  | array | 微博列表 |

备注:

    一个微博数据示例(字段都有, 顺序不一定):
```js
{
    "id" : 4,
    "type" : 1, // 0: 纯文字 1:图片 2:小视频 3:附件 4:文章 5:活动
    "content" : "<p>这是内容</p>",
    "imgs" : [
        "//inschool-oss.incourse.com.cn/FkVhaibm02R6PwIKVqv0Uv4Ys38U.jpg"
    ],
    "payload" : {}, // 暂时存放活动详细信息, 如开始时间,描述,地点等
    "report_count": 9, // 有多少人举报
    "forward_num" : 0,
    "comment_num" : 0,
    "like_num" : 0,
    "user_id" : 5193,
    "school_id" : 63,
    "club_id" : 1, // 社团内发的才会有
    "created_at" : "Thu, 29 Nov 2018 08:40:17 GMT"
}
```
    一个活动示例:
```js
{
    "_id" : 7,
    "type" : 5,
    "content" : "<p>圣诞节party</p>",
    "imgs" : null,
    "payload" : {
        "title" : "圣诞节party",
        "content" : "圣诞节party圣诞节party圣诞节party圣诞节party圣诞节party圣诞节party",
        "time" : "2018.12.25",
        "place" : "梦想小镇2号楼",
        "limit" : "111",
        "contact" : "111111111111",
        "img" : "//inschool-oss.incourse.com.cn/FgvR8rNH0OR2gnMGcgXU4UBmVXfC.jpg"
    },
    "forward_num" : 0,
    "comment_num" : 0,
    "like_num" : 0,
    "members" : [],
    "user_id" : 5234,
    "school_id" : 63,
    "created_at" : ISODate("2018-12-17T09:30:44.978Z"),
    "deleted_at" : ISODate("2018-12-21T06:50:02.744Z")
}
```

# 2. 获取举报的列表

简要描述:
    获取举报的微博

请求URL:

    /reports/

请求方式:

    GET

参数：

| 参数名  |  必填  |  类型   |  默认值 |  说明  |
| ------ | ------ | ------ | ------ | ------ |
| blog_id |  是   | int    | 1       | 微博的id |
| page   |  否    | int    | 1       | 第几页 |
| per_page | 否   | int    | 10      | 每页多少个举报 |

返回数据类型:

    json

返回数据:

|  参数名 |  类型  |  说明    |
| ------ | ------ | ------ |
|  page  |  int   | 当前第几页 |
|  per_page | int | 一页多少个 |
|  total  | int   | 总共有多少个举报 |
|  items  | array | 举报列表 |

备注:

    一个举报数据示例(字段都有, 顺序不一定):
```js
{
    "id" : 2,
    "type" : 0, // 预留字段
    "reason" : "引起强烈不适，建议永封！！！",
    "blog_id" : 7,
    "user_id" : 5010, // 举报人
    "school_id" : 63,
    "created_at" : "Thu, 29 Nov 2018 08:40:17 GMT"
}
```

# 3. 处理举报

简要描述:
    处理举报的微博

请求URL:

    /reports/check/

请求方式:

    POST

参数：

| 参数名  |  必填  |  类型   |  默认值 |  说明  |
| ------ | ------ | ------ | ------ | ------ |
| blog_id |  是   | int    | 1      | 微博的id |
| action |  是    | string |        | 'delete' => 会删除此微博, 'ignore' => 微博不作处理  |

返回数据:

    无

备注:

    状态码 204 表示成功
