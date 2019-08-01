# 数组、对象的方法

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
### 数组的方法


1、`find()` 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined

```例子
const arr = [1, 4, 3, 2]
const res1 = arr.find(v => v > 2)
const res2 = arr.find(v => v < 1)
console.log(res1); // 4
console.log(res2); // undefined
```
2、`includes()` 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false

!> 注意：对象数组不能使用includes方法来检测

```例子
const arr = ['cat', 'dog']
const res1 = arr.includes('cat')
const res2 = arr.includes('do')
console.log(res1); // true
console.log(res2); // false
```
3、`reduce()` 方法对数组中的每个元素执行一个由您提供的函数(升序执行)，将其结果汇总为单个返回值

```例子
const arr = [1, 2, 3, 4];
const res1 = arr.reduce((accumulator, currentValue) => accumulator + currentValue)
const res2 = arr.reduce((accumulator, currentValue) => accumulator + currentValue, 5)

// 1 + 2 + 3 + 4
console.log(res1); // 10

// 5 + 1 + 2 + 3 + 4
console.log(res2); // 15
```
4、`some()` 方法测试是否至少有一个元素可以通过被提供的函数方法。该方法返回一个Boolean类型的值

!> 对于空数组上的任何条件，此方法返回false

```例子
const arr = [1, 2, 3, 4, 5];
const res1 = arr.some(v => v > 4)
const res2 = arr.some(v => v < 1)
console.log(res1); // true
console.log(res2); // false
```

5、`every()` 方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值

!> 若收到一个空数组，此方法在一切情况下都会返回 true

```例子
const arr = [1, 2, 3, 4, 5];
const res1 = arr.every(v => v < 6)
const res2 = arr.every(v => v > 3)
console.log(res1); // true
console.log(res2); // false
```

6、`entries()` 方法返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对

```
const arr = ['a', 'b', 'c'];
var iterator = arr.entries();
for (const key of iterator) {
  console.log(key)
}
// [0, "a"]
// [1, "b"]
// [2, "c"]
```

7、`slice(begin, end)` 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变

```
const arr = ['ant', 'bison', 'camel', 'duck', 'elephant'];
const res1 = arr.slice(2)
const res2 = arr.slice(2, 4)
const res3 = arr.slice(1, 5)
console.log(res1) // ["camel", "duck", "elephant"]
console.log(res2) // ["camel", "duck"]
console.log(res3) // ["bison", "camel", "duck", "elephant"]
```

8、`splice(index, howmany, ...items)` 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组
  * index: 必需,要替换元素的开始位置
  * howmany: 必需,将被替换元素的个数
  * items: 可选,替换的元素
  即：从 `index` 开始替换；如果howmany为0，则没有`被替换元素`，直接添加`替换元素`,否则将items`替换元素`替换howmany个`被替换元素`；如果没有items则没有`替换元素`删除howmany个`被替换元素`

```
const arr = [1, 3, 4, 6]
arr.splice(1, 0, 2)
console.log(arr) // [1, 2, 3, 4, 6]
arr.splice(4, 1, 5)
console.log(arr) // [1, 2, 3, 4, 5]
arr.splice(3, 2)
console.log(arr) // [1, 2, 3]
```
