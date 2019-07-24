# MUI 总结

> 高性能前端APP框架
### 上拉加载

[上拉加载](http://dev.dcloud.net.cn/mui/pullup/)

```html盒子
<div id="inventorys" class="mui-content mui-scroll-wrapper">
  <div class="mui-scroll">
    <!--数据列表-->
    <ul class="mui-table-view mui-table-view-chevron" id="list"></ul>
  </div>
</div>
```

```js逻辑
var current = 1; // 当前页数
var lastPage; // 总共页数
// 初始化配置
mui.init({
  pullRefresh: {
    container: "#inventorys", //待刷新区域标识，querySelector能定位的css选择器均可，比如：id、.class等
    up: {
      height: 50, //可选.默认50.触发上拉加载拖动距离
      auto: true, //可选,默认false.自动上拉加载一次
      contentrefresh: "正在加载...", //可选，正在加载状态时，上拉加载控件上显示的标题内容
      contentnomore: "没有更多数据了", //可选，请求完毕若没有更多数据时显示的提醒内容；
      callback: pullupRefresh //必选，刷新函数，根据具体业务来编写，比如通过ajax从服务器获取新数据；
    }
  }
});
// 逻辑：通过endPullupToRefresh()判断是否继续加载
function pullupRefresh() {
  setTimeout(function() {
    getList();
    mui("#inventorys")
      .pullRefresh()
      .endPullupToRefresh(current > lastPage); // false还要加载数据 true为无数据
  }, 1500);
}
// 数据处理
function getList() {
  lastPage = 3
  current++;
}
```

!> 问题：MUI 上拉加载 click 事件失效

?> 解决办法： 使用 tap 事件

```解决click事件失效
mui("#list").on("tap","li",function(){
  
})
```

### 下拉刷新

[下拉刷新](http://dev.dcloud.net.cn/mui/pulldown/)

```
// 初始化配置
var current = 1; // 当前页数
mui.init({
  pullRefresh : {
    container:"#inventorys",//下拉刷新容器标识，querySelector能定位的css选择器均可，比如：id、.class等
    down : {
      height:50,//可选,默认50.触发下拉刷新拖动距离,
      auto: true,//可选,默认false.首次加载自动下拉刷新一次
      contentdown : "下拉可以刷新",//可选，在下拉可刷新状态时，下拉刷新控件上显示的标题内容
      contentover : "释放立即刷新",//可选，在释放可刷新状态时，下拉刷新控件上显示的标题内容
      contentrefresh : "正在刷新...",//可选，正在刷新状态时，下拉刷新控件上显示的标题内容
      callback :pullDownRefresh //必选，刷新函数，根据具体业务来编写，比如通过ajax从服务器获取新数据；
    }
  }
})
// 逻辑：页数为第一页
function pullupRefresh() {
  setTimeout(function() {
    current = 1
    getList()
  }, 1000)
}
// 数据处理
function getList() {
  
}
```