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

```通过roles处理路由
/**
 * Use meta.role to determine if the current user has permission
 * @param roles
 * @param route
 */
function hasPermission(roles, route) {
  if (route.meta && route.meta.roles) {
    return roles.some(role => route.meta.roles.includes(role))
  } else {
    return true
  }
}

/**
 * Filter asynchronous routing tables by recursion
 * @param routes asyncRoutes
 * @param roles
 */
export function filterAsyncRoutes(routes, roles) {
  console.log('routes')
  console.log(routes)
  const res = []
  routes.forEach(route => {
    const tmp = {
      ...route
    }
    console.log(tmp)
    if (hasPermission(roles, tmp)) {
      if (tmp.children) {
        tmp.children = filterAsyncRoutes(tmp.children, roles)
      }
      res.push(tmp)
    }
  })

  return res
}
```

```通过roleId获取对应路由
const actions = {
  generateRoutes({
    commit
  }, data) {
    return new Promise(resolve => {
      const { roles, roleId } = data
      const containerRouters = {
        1: componentsRouter,
        2: chartsRouter,
        3: tableRouter,
        4: nestedRouter
      }
      let accessedRoutes
      if (roles.includes('admin')) {
        accessedRoutes = asyncRoutes || []
      } else {
        accessedRoutes = filterAsyncRoutes(containerRouters[roleId], roles)
      }
      commit('SET_ROUTES', accessedRoutes)
      resolve(accessedRoutes)
    })
  }
}
```


### 分配路由，区分用户

流程讲解：
  1. 前后端配合定义好路由数据参数、格式，登陆账号获取用户路由数据
  2. 将后台返回的路由数据处理成正确路由格式
    2-1. 一级菜单，二级目录，将`component`为`Layout`单独处理
    2-2. `component`组件路径处理，创建开发环境`_import_development.js`，生产环境`_import_production`两个js文件，用于`component`引入文件

```
/**
 * @param routers 后台返回路由数据
 */
export function filterRouters(routers) {
  const result = []
  routers.forEach(route => {
    const tmp = {
      hidden: route.hidden,
      path: route.path,
      component: route.component,
      redirect: route.redirect ? route.redirect : 'noredirect',
      name: route.name,
      alwaysShow: route.alwaysShow,
      meta: {
        title: route.title,
        icon: route.icon,
        roles: route.roles,
        noCache: route.noCache,
        breadcrumb: route.breadcrumb
      }
    }
    if (route.children) {
      tmp.children = filterRouters(route.children)
    }
    result.push(tmp)
  })
  return result
}
```

```_import_development.js
module.exports = file => require('@/' + file + '.vue').default // vue-loader at least v13.0.0+
```

```_import_production.js
module.exports = file => () => import('@/' + file + '.vue')
```

```permission.js
const _import = require('./router/_import_' + process.env.NODE_ENV)
import Layout from '@/views/layout/Layout'
function filterAsyncRouter(asyncRouterMap) { // 遍历后台传来的路由字符串，转换为组件对象
  const accessedRouters = asyncRouterMap.filter(route => {
    if (route.component) {
      if (route.component === 'Layout') { // Layout组件特殊处理
        route.component = Layout
      } else {
        route.component = _import(route.component)
      }
    }
    if (route.children && route.children.length) {
      route.children = filterAsyncRouter(route.children)
    }
    return true
  })
  return accessedRouters
}
```

?> 最后都是通过router.addRoutes(accessRoutes)将动态路由添加到静态路由中