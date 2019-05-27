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





## 任务

**任务列表**

```
GET /api/surveys
```



**任务详情**

```
GET /api/surveys/:sid
```



**发布任务**

```
POST /api/surveys

{
  "title": "",
  "content": "",
  "bounty": 0
}
```



**领取任务**

```
POST /api/surveys/:sid/pull
```



**提交任务**

```
POST /api/surveys/:sid/submit

{
  "content": ""
}
```





