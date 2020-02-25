# Zhihu API 笔记

## 一、Overview

### 1. 接口baseURL

目前zhihu混合使用v3、v4两个版本的API：

1. `https://www.zhihu.com/api/v3`
2. `https://www.zhihu.com/api/v4`

### 2. 返回格式

API返回结果格式为：

1. 出错示例：

```json
{
  "code": "100010",
  "'message'": "Unsupported auth type oauth"
}
```

2. 成功示例：

```javascript
{
  "paging": { // 分页方案
    "is_end": false,
    "is_start": false,
    "next": "url",
    "previous": "url",
    "totals": 590
  },
  "data": [ // 数组
    {
    }
  ],
  "ad_info": { // “随机”出现
    "ad": {
    },
    "adjson": "",
    "position": 12
  }
}
```

### 3. 分页

请求时在querystring中加参数：

1. limit：返回数据的最大数量
2. offset: 偏移
3. sort_by：排序方式，default

返回的数据中，`paging`字段包含分页信息。

### 4. 鉴权

TODO

### 5. 订制返回结果

请求时在querystring中加参数`include`。格式如下

```
include=
  data[*]
    .is_normal,
    .admin_closed_comment,
    .excerpt,
    .question;
  data[*]
    .author
      .follower_count
      badge[*]
        .topics
```

TODO：`include`参数的解析方式不明，还需要进一步分析代码

## 二、具体API

### 1. 热榜

`/api/v3/feed/topstory/hot-lists/{:topic}?limit=50&desktop=true`

#### (1) 权限

不需要登录

#### (2) 参数

**topic**:

1. total：全部
2. science：科学
3. digital：数码
4. sport：体育
5. fashion：时尚
6. film：影视
7. ...

#### (3) 返回

[示例](/data/hot-lists-total.json)

### 2. 问题

`/api/v4/questions/{:question_id}?include={:include}`

#### (1) 权限

不需要登录

#### (2) 参数

```
include=detail,excerpt
include=question.detail,excerpt
include=data[*].question.detail,excerpt
// TODO: 以上三种参数居然等价，解析方式不明，需进一步分析代码
```

#### (3) 返回

[示例](/data/questions.json)

### 3. 问题 + 回答

`/api/v4/questions/{:question_id}/answers?include={:include}&limit={:limit}&offset={:offset}&platform=desktop&sort_by=default`

#### (1) 权限

不需要登录

#### (2) 参数



#### (3) 返回

[示例一](/data/questions-answers1.json)

[示例二](/data/questions-answers2.json)

