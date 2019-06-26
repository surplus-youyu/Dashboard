# 架构设计、详细设计（BCE 方法）到应用程序框架映射指南

## 逻辑架构

逻辑架构由三层模型（表示层、业务层、持久化层）构成

## 1.1.表示层

用户使用 Web 作为表示层，提供任务子系统，用户子系统，评论子系统，通知子系统

## 1.2.业务层

服务器充当业务层的角色，为表示层的各个子系统提供相应的服务模块

## 1.3.持久化层

MySQL 提供了数据的持久化服务



## 框架目录设计

### Web

```
.
├── babel.config.js
├── ChangeLog.md
├── default.conf
├── dist
│   ├── css
│   ├── favicon.ico
│   ├── fonts
│   ├── img
│   ├── index.html
│   └── js
├── LICENSE
├── mock
│   ├── index.js
│   ├── questionnaire.js
│   └── user.js
├── package.json
├── package-lock.json
├── postcss.config.js
├── public
│   ├── favicon.ico
│   └── index.html
├── README.md
├── src
│   ├── App.vue
│   ├── assets
│   ├── components
│   ├── global-variables.less
│   ├── global-variables.ts
│   ├── layouts
│   ├── main.ts
│   ├── shims-module.d.ts
│   ├── shims-tsx.d.ts
│   ├── shims-vue.d.ts
│   ├── stores
│   ├── typings
│   ├── utils
│   └── views
├── theme
│   └── index.less
├── tsconfig.json
├── tslint.json
└── vue.config.js
```





### 后端

```
.
├── configs
│   └── config.go
├── controllers
│   ├── login.go
│   ├── task.go
│   └── user.go
├── docker
│   └── docker-compose.yml
├── go.mod
├── go.sum
├── LICENSE
├── main.go
├── models
│   ├── answer.go
│   ├── db.go
│   ├── task.go
│   └── user.go
├── README.md
├── routes
│   └── route.go
├── utils
│   ├── handle_error.go
│   └── types.go
└── vendor
    ├── github.com
    ├── golang.org
    ├── google.golang.org
    ├── gopkg.in
    └── modules.txt
```



## 与 ECB 关系

ECB中：

- Entity：代表系统数据，如：任务，用户等
- Boundary：与用户的接口，如：界面、网关、代理等
- Controller：在 Boundary 和 Entity 中衔接的媒介，编排来自 Boundary 的命令的执行

在本系统中，Boundary有三个部分：

- 用户端 Web 程序用户界面
- 服务端 Nginx 反向代理服务器

Controller 包含：

- 服务器框架目录设计中，`controllers` 目录下定义了所有的相关内容，它们接收来自上述 Boundary 的命令，并执行相应逻辑

Entity 包含：

- 服务器框架目录设计中，`models` 目录下定义了所有服务器与 MySQL 相关的实体，承载系统数据