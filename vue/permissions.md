# 路由权限

> 路由权限多用于后台系统，主要区分显示的功能、组件。这里是借鉴 vue-element-admin 自己的总结

* [vue-element-admin文档](https://panjiachen.github.io/vue-element-admin-site/zh/)

### 固定路由，区分角色

流程讲解：
  1. 定义多个角色路由，将不同路由与角色关联。
  2. 登陆账号获取`token`，通过`token`获取用户信息区分角色`roleId`,将`token`、`roleId`存储到状态管理中`vuex`
  3. 定义全局路由拦截器(路由守卫)，在`mian.js`程序入口文件引入。
    3-1. 通过状态管理中的`token`判断登陆状态，`roleId`判断用户信息获取。
    3-2. 声明路由对象`routerObj`，对象属性为角色Id，值为角色对应的路由
    3-3. 通过`roleId`获取对应的路由，使用`router.addRoutes()`将动态路由添加到静态路由中

### 分配路由，区分用户

流程讲解：
  1. 前后端配合定义好路由数据参数、格式，登陆账号获取用户路由数据
  2. 将后台返回的路由数据处理成正确路由格式
    2-1. 一级菜单，二级目录，将`component`为`Layout`单独处理
    2-2. `component`组件路径处理，创建开发环境`_import_development.js`，生产环境`_import_production`两个js文件，用于`component`引入文件