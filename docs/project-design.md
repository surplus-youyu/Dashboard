## 技术选型理由

### 前端

#### 形式

一共有三种方案：App、Web与小程序

* App：在移动端上能得到最好的用户体验，但对新用户来说安装一个新App的成本较高。
* Web：相比App来说，Web更加轻量，不需要用户安装额外的应用。但由于浏览器厂家、版本等不同带来的兼容性问题为开发带来了难度，而且通常Web端的流畅度体验一般不佳。
* 小程序：小程序是基于微信的一种服务形式，体验比起Web更接近原生应用。而且基于用户基本都安装有、且已打开微信的场景，省去了用户安装App的时间成本，更加方便。

##### 最终选择

对三种形式的优缺点以及团队技术栈进行考虑分析，最终选择Web作为我们主开发路线，后期如有时间再开发小程序版本。

#### 框架

目前Web的话单页面应用（SPA）比较流行，其中三个很火的框架：React、Vue 和 Augular。

* React：React + Redux的搭配加上不可变的数据减少了出 bug 的可行性，但 jsx 里代码与 HTML 耦合程度较高。
* Vue：MVVM 模式，组件化开发，通过 setter 与 getter 以及 VDOM 提供了较好的性能。而且其文档更完整。
* Angular：MVC 模式，有依赖注入等优势，但脏值检查性能较差。

##### 最终选择

基于开发难度与社区活力，我们选择 Vue 进行 SPA 应用开发。

## 模块划分

### 前端

* VueJS
  * 可复用组件
  * 页面
* Vue-Router
  * 前端路由
* Vuex
  * 全局状态管理
* Axios
  * 网络请求

## 模块内设计

### 前端

#### VueJS

组件树如下：

```
└─App：挂载整个应用
   ├─ErrorModal: 全局错误提示框组件
   ├─auth:【权限界面】
   │  ├─Login:【登录界面】
   │  ├─Register:【注册界面】
   │  └─Modal: 权限界面提示框组件
   └─main:【主界面】
      ├─SideMenu: 主界面导航组件
      ├─TopLine: 主界面顶部组件
      ├─Main: 【概览界面】
      ├─RequestHall: 请求大厅
      │  ├─InformationRetrievalBar: 请求页面信息导航组件
      │  ├─RequestCard: 请求简介组件
      │  └─PageSwitcher: 换页组件
      ├─PersonalPage: 【个人中心页面】
      ├─AssignmentPage: 【我的请求页面】
      │  ├─ReceivedAssignmentPage: 【已接受的请求页面】
      │  │  ├─InformationRetrievalBar: 请求页面信息导航组件
      │  │  ├─RequestTable: 已接受请求的表格组件
      │  │  └─PageSwitcher: 换页组件
      │  ├─PublishedAssignmentPage: 【已发送的请求页面】
      │  │  ├─InformationRetrievalBar: 请求页面信息导航组件
      │  │  ├─RequestTable: 已发送请求的表格组件
      │  │  └─PageSwitcher: 换页组件
      │  └─AssignmentStatisticsPage: 【问卷统计页面】
      └─PublishPage: 【发布请求页面】
         ├─PublishQuestionnairePage:【发布问卷页面】
         |  └─QuestionCreater: 问题添加组件 
         └─PublishTaskPage: 【发布自定义任务页面】
            └─AttachmentUpload: 附件上传组件
```

#### Vuex：

- Mutation：定义了对`State`中数据的修改操作。组件使用`State`中的数据的时候并不能直接对数据进行修改操作，需要调用`Mutation`定义的操作来实现对数据的修改。这也是Vuex定义中所说的用相应的规则来让数据发生变化的具体实现。
- Action：`Mutation`中定义的操作只能执行同步操作，Vuex中的异步操作在`Action`中进行，`Action`最终通过调用`Mutation`的操作来更新数据
- Module：`Store`和`State`之间的一层，便于大型项目管理，`Store`包含多个`Module`，`Module`包含`State`、`Mutation`和`Action`

