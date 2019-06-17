# API 设计

通用响应格式

```
{
    "status": bool,
    "msg": str
}
```



## 用户

**登录**

```
PUT /api/login

Request(application/json):

{
    "email": "",
    "password": ""
}
```



**注册**

```
PUT /api/register

Request(application/json):

{
    "email": "",
    "nickname": "",
    "password": "",
}
```



**获取用户信息**

```
GET /api/user
```



**修改用户信息**

```
PUT /api/user

Request(application/json):

{
    "nickname": "",
    "password": ""
}
```



**用户头像**

```
GET /api/user/avatar

PUT /api/user/avatar

Request(multipart/form-data):

{
    "avatar": ...
}
```



**获得与当前用户的任务（发布，接受）**

```
GET /api/user/tasks?role=

role: 'publisher', 'recipient', ''
```



**获得用户参与的组**

```
GET /api/user/groups
```





## 任务

**任务列表**

```
GET /api/tasks
```

**任务详情**

```
GET /api/tasks/:tid
```

**发布任务**

```
POST /api/tasks

Request(multipart/form-data):

{
  title: ...,
  description: ...,
  type: 'TASK_TYPE_SURVEY' | 'TASK_TYPE_CUSTOM',
  content: ..., // extra content, useless to custom tasks
  reward: ...,
  limit: ...
}
```

**任务结束**
```
PUT /api/tasks/:tid
```

**领取任务**
```
POST /api/assignments
{
 "task_id": ...
}
```

**查看自己领取的任务列表**
```
GET /api/assignments
```

**查看自己领取任务的详情**
```
GET /api/assignments/assign_id
```

**提交一个答案**
```
POST /api/assignments/:assign_id
{
  "payload": ...
}
```

**查看某一个提交**
```
GET /api/assignments/:assign_id
```

**获取某个任务的所有提交**
```
GET /api/tasks/:tid/assignments
```

**审核某个任务的某一次提交**
```
PUT /api/tasks/:task_id/assignments/:assgn_id
{
  "pass": true/false
}
```

## 兴趣组

**获得公开的组**

```
GET /api/groups
```



**创建某个组**

```
POST /api/groups

{
    name: "",
    summary: "",
    "is_public": ""
}
```



**获得某个组的信息**

```
GET /api/groups/:gid
```



**修改某个组的信息（群主）**

```
PUT /api/groups/:gid
```



**解散某个组**

```
DELETE /api/groups/:gid
```



**将某个用户加入（群主） /  加入某个组**

```
POST /api/groups/:gid/users/:uid
```



**将某个用户移出（群主） / 退出某个组**

```
DELETE api/groups/:gid/users/:uid
```



