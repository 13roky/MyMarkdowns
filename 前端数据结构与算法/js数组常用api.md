## 创建数组

```js
// 1. 通过字面量创建数组
let array = []
let array2 = ["test", 1, true]
// 2. 通过数组的构造函数创造
let array3 = new Array()
let array4 = new Array("test", 2, false)

// 3. 基本操作
console.log(array2[0])
// > "test"
array2[array2.length] = "new"
console.log(array2)
// > Array ["test", 1, true, "new"]
console.log(array2 instanceof Array)
// > true
```

## Array 的内置 API 操作

### Array.isArray()

判断一个变量是否是数组。

```js
let arr = []
console.log(Array.isArray(arr))
// > true
```

### Array.prototype.length

记录了数组长度的属性。数组里面有几个数就是几，不用考虑从 0 开始。

```js
let arr = [1, 2, 3]
console.log(arr.length)
// > 3
```

### Array.prototype.at()

用于返回数组中的数据，如果我们使用字面量的方法返回数组那么只能够通过索引值来返回。如`array[3]`，但是如果我们想返回倒数第一个数据，上面的方法就不能够实现了。我们就可以使用`array.at(-1)`来输出倒数第一个数据。

```js
let arr = [1, 2, 3]
console.log(arr.at(-1))
// > 3
```

### Array.prototype.push()

在数组尾部添加新元素，尾插入。

- 返回该数组的新长度。
- 修改原数组。

```js
let arr = [1, 2, 3]
let newLength = arr.push(4, 5)
console.log(arr, newLength)
// > Array [1, 2, 3, 4, 5] 5
```

### Array.prototype.pop()

删除数组最后一个元素，尾删除。

- 返回被删除的元素。
- 修改原数组。

```js
let arr = [1, 2, 3]
let del = arr.pop()
console.log(arr, del)
// > Array [1, 2] 3
```

### Array.prototype.unshift()

在数组头部添加新元素，首插入。

- 返回该数组的新长度。
- 修改原数组。

```js
let arr = [1, 2, 3]
let newLength = arr.unshift(4, 5)
console.log(arr, newLength)
// > Array [4, 5, 1, 2, 3] 5
```

### Array.prototype.shift()

删除数组第一个元素，首删除。

- 返回被删除的元素。
- 修改原数组。

```js
let arr = [1, 2, 3]
let del = arr.shift()
console.log(arr, del)
// > Array [2, 3] 1
```

### Array.prototype.reverse()

将数组中元素的位置颠倒。数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个。

- 返回反转后的数组。
- 修改原数组。

```js
let arr = [1, 2, 3]
let reversed = arr.reverse()
console.log(reversed)
// > Array [3, 2, 1]
```

### Array.prototype.sort()

对数组的元素进行排序，使用冒泡排序的方法。默认只根据首字符进行排序。数字的排序在字符串之前。

- 返回排序后的数组。
- 修改原数组。

```js
let arr = [1, 30, 'March', 'Jan', 'Feb', 'Dec',4, 21, 100000]
// 默认排序
console.log(arr.sort())
// > Array [1, 100000, 21, 30, 4, "Dec", "Feb", "Jan", "March"]

// 升序排序
console.log(arr.sort(function(a,b){
    return a-b
}))
// > Array [1, 4, 21, 30, 100000, "Dec", "Feb", "Jan", "March"]

// 降序排序
console.log(arr.sort(function(a,b){
    return a-b
}))
// > Array [100000, 30, 21, 4, 1, "Dec", "Feb", "Jan", "March"]
```

其中 a，b 是处于冒泡中的两个值：

- 如果回调函数返回值大于 0 ， b 会被排列到 a 之前。
- 如果回调函数返回值小于 0 ，那么 a 会被排列到 b 之前。
- 如果回调函数返回值等于 0 ， a 和 b 的相对位置不变。

### Array.prototype.indexOf()

查找数组中指定元素的索引。`indexof()`从前往后查找，`lastIndexOf()`从后往前查找。

- 返回查找到的元素在数组中的**索引**。只返回查找到的第一个元素。**如果没有查找到则返回 -1。**
- 不修改原数组。

```js
let arr = [1,2,3,4,5,1,2]
console.log(arr.indexOf(2))
// > 1
console.log(arr.lastIndexOf(2))
// > 6
```

### Array.prototype.toString()

将数组转换成字符串。

- 返回转换结果。
- 不修改原数组。

```js
let arr = ["Hello", "World", "!"]
console.log(arr, arr.toString())
// > Array ["Hello", "World", "!"] "Hello,World,!"
```

### Array.prototype.join()

将数组转换成字符串。可以指定拼接的字符，默认为`,`。比`toString()`更加实用。

- 返回转换结果。
- 不修改原数组。

```js
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// > "Fire,Air,Water"

console.log(elements.join(''));
// > "FireAirWater"

console.log(elements.join('-'));
// > "Fire-Air-Water"
```

### Array.prototype.forEach()

遍历数组中的每个元素，并指定操作。**当使用 return 时，仅跳出本次，并继续遍历，相当于 continue**。

- 没有返回值。
- 是否修改原数组要看使用者如何操作。

```js
const array = ['a', 'b', 'c']

// 回调函数中 item 是遍历的数组元素，index 是当前元素的索引值，arr 是当前正在遍历的数组
array.forEach((item,index,arr) => {
	console.log(item, index, arr)
})
/* 没有修改原数组
> "a" 0 Array ["a", "b", "c"]
> "b" 1 Array ["a", "b", "c"]
> "c" 2 Array ["a", "b", "c"]
*/

array.forEach((item,index,arr) => {
	if(item == "b") arr[index] = 1
})
console.log(array)
// 修改了原数组
// > Array ["a", 1, "c"]
```

注意：如果数组中存的是对象，则可以直接修改其属性值。

### Array.prototype.filter()

过滤符合条件的元素。

- 返回过滤结果。
- 不修改原数组。

```js
let arr = [1, 2, 3, 4, 5, 6, 7, 8]

// 回调函数中 item 是遍历的数组元素，index 是当前元素的索引值，arr 是当前正在遍历的数组
let newArr = arr.filter((item, index, arr) => {
    return item > 5
})
console.log(arr, newArr)
// > Array [1, 2, 3, 4, 5, 6, 7, 8] Array [6, 7, 8]
```

### Array.prototype.some()

对数组进行遍历，如果其中一次有返回值，那么则返回 true，如果没有返回值或返回 undefinded 则最终返回 false。

**当出现一次返回为 true 时，就会停止之后的遍历。相当于 break**

```js
let array = ["a", "b", "c"]

let out = array.some((item, index, arr) => {
  console.log(item)
})
console.log(array, out)
/*
> "a"
> "b"
> "c"
> Array ["a", "b", "c"] false
*/
```

### Array.prototype.map()

方法创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成。

- 返回修改后的数组。
- 不修改原数组。

```js
const array1 = [1, 4, 9, 16];

const map1 = array1.map(x => x * 2);

console.log(map1);
console.log(array1);
// > Array [2, 8, 18, 32]
// > Array [1, 4, 9, 16]
```

### Array.prototype.concat()

数组拼接。

- 返回拼接后的数组。
- 不修改原数组。

```js
const array1 = ["a", "b", "c"]
const array2 = ["d", "e", "f"]
const array3 = ["g", "h", "i"]
const array4 = array1.concat(array2, array3)
const array5 = [].concat(array1,array2,array3)

console.log(array4)
// > Array ["a", "b", "c", "d", "e", "f", "g", "h", "i"]
console.log(array5)
// > Array ["a", "b", "c", "d", "e", "f", "g", "h", "i"]
```

### Array.prototype.slice()

返回指定区间的数组元素。

- 不修改原数组。

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]

console.log(animals.slice(-2));
// expected output: Array ["duck", "elephant"]

console.log(animals.slice(2, -1));
// expected output: Array ["camel", "duck"]

console.log(animals.slice());
// expected output: Array ["ant", "bison", "camel", "duck", "elephant"]
```

### Array.prototype.splice()

删除指定位置，向后指定个元素，并插入指定元素。

- 会修改原数组。

```js
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
// inserts at index 1
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May');
// replaces 1 element at index 4
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "May"]
```

## 数组遍历

```js
let arr = [1, 2, 3, 4, 5]
// 遍历数组函数
function travel(arr, fn) {
  for (let i = 0; i < arr.length; i++) {
    fn(arr[i], i, arr)
  }
}
// 使用函数方法
travel(arr, (item, index, arr) => {
  console.log(`travel: ${index}:${item}`)
})

// 递归遍历
function travel(arr, i) {
  if (!arr[i]) return
  console.log(arr[i])
  travel(arr, i + 1)
}

// 将遍历数组函数添加到 Array.prototype 上，手写 forEach
Array.prototype.travel = function (fn) {
  for (let i = 0; i < this.length; i++) {
    fn ? fn(this[i], i, this) : console.log(this[i])
  }
}
// 使用 Array 方法
arr.travel((item, index, arr) => {
  console.log(`arr.travel: ${index}:${item}`)
})
arr.travel()
```

## 数组去重（重点）

**自写函数**

```javascript
function unique(arr){
    var newArr = [];
    for (var i = 0; i < arr.length; i++) {
        if (newArr.indexOf(arr[i]) === -1) {
            newArr.push(arr[i]);
        }
    }
    return newArr;
}
```

**利用`Set`对象**

```javascript
var arr = [1, 2, 3, 1, 2, 3, 1, 2, 3]
var set = new Set(arr)
// arr = new Array(...set)
arr = [...set]
console.log(arr)
```

```js
// 不会对实参arr进行修改
function changeArr(arr){
  arr = 1
}
// 会对实参arr进行修改
function changeArr2(arr){
  arr[0] = 1
}
```
