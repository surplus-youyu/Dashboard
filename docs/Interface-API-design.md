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
PUT /api/login

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
  "title": "",
  "content": "",
  "type": "",
  "bounty": 0,
  "enclosure": ...
}
```



**删除任务**

```
DELETE /api/tasks/:tid
```



**领取任务**

```
POST /api/tasks/:tid/recieve
```



**提交任务**

```
POST /api/tasks/:tid/submit

Request(multipart/form-data):

{
  "content": "",
  "enclosure": ...
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



