# 小程序常见问题

#### 失效问题
* 小程序的mastache语法不支持js的方法。
* 即在页面标签中，使用以下js方法无效：

```
Object.keys() 
toString() 
indexOf()
```

解决办法： 

* 通过`wxs`解决wxml页面失效问题。定义filters.wxs在utils文件夹内

```filters.wxs
function indexOf(arr, value) {
  if (arr.indexOf(value) < 0) {
    return false;
  } else {
    return true;
  }
}
module.exports = {
  indexOf: indexOf
}
```
* 引入使用

```wxml引入
<wxs src="../../utils/filters.wxs" module="filters" />
```
```使用
<view wx:if="{{filters.indexOf(certificateInfo.color, '蓝')}}" class="colorItem blue"></view>
```

#### input设置字符最大长度问题