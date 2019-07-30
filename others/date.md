# Date 相关总结

```基本方法
  var d = new Date()
  var year = d.getFullYear() // 年
  var month = d.getMonth() // 月
  var date = d.getDate() // 日
  var day = d.getDay() // 周
  var hours = d.getHours() // 时
  var minutes = d.getMinutes() //分
  var seconds = d.getSeconds() // 秒
  var milliseconds = d.getMilliseconds() // 毫秒
  var time = d.getTime() // 总毫秒-时间戳
  d.setDate(date) // 设置日 date为1~31
  d.setMonth(month) // 设置月 month为1~11
  d.setFullYear(year) // 设置年 year为四位数
  d.setHours(hours) // 设置小时 hours为0～23
  d.setMinutes(minutes) // 设置分钟 minutes为0～59
  d.setSeconds(seconds) // 设置秒钟 seconds为0～59
  d.setMilliseconds(milliseconds) // 设置毫秒 milliseconds为0～999
  d.setTime() // 以毫秒设置 Date 对象
```
* 方法一：原型

```时间格式化
  Date.prototype.format = function (fmtStr) {
    let fmt = fmtStr || 'yyyy-MM-dd'
    const o = {
      'M+': this.getMonth() + 1, // 月份
      'd+': this.getDate(), // 日
      'h+': this.getHours(), // 小时
      'm+': this.getMinutes(), // 分
      's+': this.getSeconds(), // 秒
      'q+': Math.floor((this.getMonth() + 3) / 3), // 季度
      'S': this.getMilliseconds() // 毫秒
    };
    if (/(y+)/.test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (this.getFullYear() + '').substr(4 - RegExp.$1.length))
    }
    for (const k in o) {
      if (new RegExp('(' + k + ')').test(fmt)) {
        fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
      }
    }
    return fmt
  }
  // 使用
  new Date().format('yyyy-MM-dd')
```
* 方法二：函数

```时间格式化
function format(fmtStr, date){
  const time = date ? new Date(date) : new Date()
  let fmt = fmtStr || 'yyyy-MM-dd'
  const o = {
    'M+': time.getMonth() + 1, // 月份
    'd+': time.getDate(), // 日
    'h+': time.getHours(), // 小时
    'm+': time.getMinutes(), // 分
    's+': time.getSeconds(), // 秒
    'q+': Math.floor((time.getMonth() + 3) / 3), // 季度
    'S': time.getMilliseconds() // 毫秒
  };
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (time.getFullYear() + '').substr(4 - RegExp.$1.length))
  }
  for (const k in o) {
    if (new RegExp('(' + k + ')').test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
    }
  }
  return fmt
}
```
```获取当天0点时间戳
new Date(new Date().toLocaleDateString()).getTime()
```

```获取当前这一周的时间
// format为上面的时间格式化函数
const weeks = []
for (let i = 0; i < 7; i++) {
  weeks.push(format('MM.dd', new Date(new Date(timesStamp + 24 * 60 * 60 * 1000 * (i - (currenDay + 6) % 7)).toLocaleDateString().replace(/[年月]/g, '-').replace(/[日上下午]/g, ''))))
}
```