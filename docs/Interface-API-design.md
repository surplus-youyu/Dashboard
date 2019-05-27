# API 设计

通用响应格式

```json
{
    "status": bool,
    "msg": str
}
```



## 用户

**登录**

```http
PUT /api/login

Request(application/json):

{
    "email": "",
    "password": ""
}
```



**注册**

```http
PUT /api/login

Request(application/json):

{
    "email": "",
    "nickname": "",
    "password": "",
}
```



**获取用户信息**

```http
GET /api/user
```



**修改用户信息**

```http
PUT /api/user

Request(application/json):

{
    "nickname": "",
    "password": ""
}
```





## 任务

**任务列表**

```http
GET /api/surveys
```



**任务详情**

```http
GET /api/surveys/:sid
```



**发布任务**

```http
POST /api/surveys

{
  "title": "",
  "content": "",
  "bounty": 0
}
```



**领取任务**

```http
POST /api/surveys/:sid/pull
```



**提交任务**

```http
POST /api/surveys/:sid/submit

{
  "content": ""
}
```





