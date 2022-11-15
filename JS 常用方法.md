# JS 常用方法技巧

## 一、数组常用方法

### 检测数组：

#### 1. `Array.isArray(arr)`

> 示例：

~~~JS
const arr = [12,20,30];
console.log(Array.isArray(arr)); // true
~~~

------

#### 2. `arr instanceof Array`

> 示例：

~~~JS
const arr = [12,20,30];
console.log(arr instanceof Array); // true
~~~

------

### 转换成数组：

#### 1. `Array.of(element1, …, elementN)`

> 作用：

将一组值(传入的参数)，转为数组

> 参数

可传入任意类型的参数

> 示例：

~~~JS
Array.of(7); // [7]

Array.of(3,11,8) // [3,11,8]

Array.of(3,11,8,{name: 'Tom'},[1,2]) // [3,11,8,{name: 'Tom'},[1,2]]
~~~

------

#### 2. `Array.from(arrayLike, mapFn, thisArg)`

> 作用：

将 类似数组的对象 或 可迭代对象 转为真正的数组

- 伪数组对象：拥有一个 `length` 属性和若干索引属性的任意对象
- 可迭代对象：可以获取对象中的元素，如 Map 和 Set 等

> 参数

1. `arrayLike`： 必选，想要转换成数组的伪数组对象或可迭代对象
2. `mapFn` ：可选，新数组中的每个元素会执行该回调函数
3. `thisArg` ：可选，执行回调函数 `mapFn` 时 `this` 对象

也就是说 `Array.from(obj, mapFn, thisArg)` 就相当于 `Array.from(obj).map(mapFn, thisArg)`

> 示例：

~~~JS
// 将 string 类型转化成数组
Array.from('foo'); // [ "f", "o", "o" ]

// 将 伪数组对象 转化成数组
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']


// 将 可迭代对象 Set 转化成数组
const set = new Set(['foo', 'bar', 'baz', 'foo']);
Array.from(set); // [ "foo", "bar", "baz" ]

// 将 可迭代对象 map 转化成数组
const map = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(map); // [[1, 2], [2, 4], [4, 8]]

// 添加一个回调函数
Array.from([1, 2, 3], x => x + x); // [2, 4, 6]

// 使用场景：数组去重合并
function combine(){
    let arr = [].concat.apply([], arguments); 
    return Array.from(new Set(arr));
}
var m = [1, 2, 2], n = [2,3,3];
console.log(combine(m,n)); // [1, 2, 3]


~~~

------

### 数组转换成字符串：

#### 1. `join()`

> 作用：

- 将一个数组的所有元素连接成一个字符串并返回这个字符串；
- 如果传入了参数，就用该参数作为分隔符，将字符串分隔；
- 如果数组只有一个元素，那么将返回该元素而不使用分隔符

> 返回类型：

返回一个字符串

> 示例：

~~~JS
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());// expected output: "Fire,Air,Water"

console.log(elements.join(''));// expected output: "FireAirWater"

console.log(elements.join('-'));// expected output: "Fire-Air-Water"
~~~

------

### 在数组后面 添加 或 删除 元素：

#### 1. `push(element1, …, elementN)`

> 作用：

将一个或多个元素添加到数组的**末尾**

> 返回类型

返回该数组的新长度，<number> 

> 示例：

~~~JS
const animals = ['pigs', 'goats', 'sheep'];
const count = animals.push('cows');

console.log(count); //  4
console.log(animals); // ["pigs", "goats", "sheep", "cows"]
~~~

------

#### 2. `pop()`

> 作用：

从数组中删除**最后一个**元素，此方法会更改数组的长度，数组长度加 1

> 返回类型：

返回数组中最后一个元素，也就是删除的元素的值，

> 示例：

~~~JS
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato'];

console.log(plants.pop()); // "tomato"
console.log(plants); // ["broccoli", "cauliflower", "cabbage", "kale"]
~~~

------

### 在数组前面 添加 或 删除 元素：

#### 1. `shift() `

> 作用：

从数组中删除**第一个**元素，此方法更改数组的长度，数组长度减 1

> 返回类型

返回数组中第一个元素，也就是删除的元素的值

> 示例：

~~~JS
const arr = [10, 20, 30];
const firstElement = arr.shift();

console.log(array1);// [20, 30]
console.log(firstElement); // 10
~~~

------

#### 2. `unshift(element1, …, elementN)`

> 作用

将一个或多个元素添加到数组的**开头**

> 返回类型

返回该数组的新长度，<number> 

> 示例

~~~JS
const arr = [10, 20, 30];

console.log(arr.unshift(40, 50)); // 5
console.log(arr); // [4, 5, 1, 2, 3]
~~~

------

### 数组排序：

#### 1. `reverse()`

> 作用：

将数组中元素的位置颠倒，数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个，该方法**会改变原数组**

> 返回：

返回该数组

> 示例：

~~~JS
const arr = ['one', 'two', 'three'];
const reversed = arr.reverse();

console.log('reversed:', reversed); // ["three", "two", "one"]
console.log('arr:', arr); // ["three", "two", "one"]
~~~

------

#### 2. `sort(compareFn)`

> 作用：

对数组的元素进行排序，该方法**会改变原数组**

默认的元素比较方式：按照升序排列数组项，调用每个数组项的 toString() 方法，然后对得到的字符串比较它们的 `UTF-16` 代码单元值序列，也就是说按照字母顺序来进行排序的

也支持传入一个比较函数 `compareFn` 作为参数，自定义元素的比较方式

> 返回类型

返回排序后的该数组

> 参数

- 不传参数，默认排序方式

- `compareFn(a,b)`，可选，用来指定按某种顺序进行排列的函数，也可以用箭头函数的方式

  - `a`：第一个用于比较的元素；`b`：第二个用于比较的元素

  - 排序规则如下：

    | `compareFn(a, b)` 返回值 | 排序顺序               |
    | :----------------------- | :--------------------- |
    | > 0                      | `a` 在 `b` 后          |
    | < 0                      | `a` 在 `b` 前          |
    | === 0                    | 保持 `a` 和 `b` 的顺序 |

  - 如果 `return a - b`， 升序；如果 `return b - a`， 降序；

> 实例：

~~~JS
// 自定义降序
const numbers2 = [4, 2, 5, 1, 3];
numbers2.sort((a, b) => b - a);
console.log(numbers2); // [5, 4, 3, 2, 1]

// 自定义升序
const numbers2 = [4, 2, 5, 1, 3];
numbers2.sort((a, b) => a - b);
console.log(numbers2); // [1, 2, 3, 4, 5]
~~~

------

### 操作数组：

#### 1. `Array.prototype.concat(element1, …, elementN)`

> 作用：

用于合并两个或多个数组，不会更改现有数组

它会先创建当前数组的一个副本，然后将接受到的参数添加到这个副本的末尾，最后返回数新构建的数组

> 参数：

可接受任意参数类型

> 返回类型：

返回一个新数组

> 示例：

~~~JS
let colors1 = ['red','yellow', 'blue'];
let colors2 = colors1.concat('white', ['black', 'green']);

console.log(colors1); // ["red", "yellow", "blue"]
console.log(colors2); // ["red", "yellow", "blue", "white", "black", "green"]
~~~

------

#### 2. `String.prototype.concat(value1, ..., valueN)`

将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回，该方法不会改变原字符串

如果参数不是字符串类型，它们在连接之前将会被转换成字符串

出于性能的考虑，**强烈建议**使用赋值操作符 (`+`,  `+=`) 代替字符串的 `concat` 方法

------

#### 3. `Array.prototype.slice(begin, end)`

> 作用

基于当前数组中的一个或多个项创建一个新数组，不会改变原数组

> 返回类型

返回一个**浅复制**了原数组中的元素的一个新数组

> 参数

- 不传参，返回一个原数组的副本
- 只有一个参数 `begin` 时，返回从 `begin` 索引开始的后面所有元素(包含 `begin`)
-  有2个参数 `begin` 和 `end` 时，返回从 `begin` ~ `end` 索引之间的所有元素(包含 `begin` 不包括 `end`)
  - `begin`、`end` 可以是负数，表示倒数的第几个索引



> 示例

~~~JS
let colors1 = ['red','yellow', 'blue'];
let colors2 = colors1.slice();
let colors3 = colors1.slice(1);
let colors4 = colors1.slice(0, 1);

console.log(colors2); // ['red','yellow', 'blue'];
console.log(colors3); // ['yellow', 'blue'];
console.log(colors4); // ['red'];

// slice 方法还可以用来将一个类数组（Array-like）对象/集合转换成一个新数组
function list() {
    // 写法：Array.prototype.slice.call(arguments)
    // return Array.prototype.slice.call(arguments);
    
    // 简化写法：[].slice.call(arguments)
    return [].slice.call(arguments);
}
var list1 = list(1, 2, 3); 
console.log(list1); // // [1, 2, 3]
~~~

------

#### 4. `String.prototype.slice(begin, end)`

用于提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串

> 示例：

~~~~JS
var str1 = 'The morning is upon us.', // str1 的长度 length 是 23。
    str2 = str1.slice(1, 8),
    str3 = str1.slice(4, -2),
    str4 = str1.slice(12),
    str5 = str1.slice(30);

console.log(str2); // 输出：he morn
console.log(str3); // 输出：morning is upon u
console.log(str4); // 输出：is upon us.
console.log(str5); // 输出：""
~~~~

------

#### 5.  `splice(start, deleteCount, item1, ..., itemN)`

> 作用

通过 **删除** 或 **替换现有元素** 或者 **原地添加新的元素** 来修改数组，此方法**会改变原数组**

> 返回类型

返回一个被删除的元素组成的一个数组

如果只删除了一个元素，则返回只包含一个元素的数组；如果没有删除元素，则返回空数组。

> 参数

- `start`：必选，指定修改的开始位置

  - 如果是正值，超出了数组的长度，则从数组末尾开始添加内容；
  - 如果是负值，负数的绝对值大于数组的长度，则从数组前面开始添加内容；

- `deleteCount`，可选，表示要移除的数组元素的个数

  - 如果 `deleteCount` 大于 `start` 之后的元素的总数，从 `start` 后面的元素都将被删除(含第 `start` 位)

  - 如果 `deleteCount` 是 0 或者负数，则不移除元素，这种情况下，至少应添加一个新元素

- `item1, item2, ...` ，可选，表示要添加进数组的元素，从 `start` 位置前面插入元素

> 示例：

~~~JS
// 从索引 3 的位置开始删除所有元素
var myFish = ['angel', 'clown', 'drum', 'mandarin', 'sturgeon'];
var removed = myFish.splice(3); 
console.log(myFish); // ["angel", "clown"]
console.log(removed); // ['mandarin', 'sturgeon']

// 从索引 3 的位置开始删除 1 个元素
var myFish = ['angel', 'clown', 'drum', 'mandarin', 'sturgeon'];
var removed = myFish.splice(3, 1); 
console.log(myFish); // ["angel", "clown", "drum", "sturgeon"]
console.log(removed); // ["mandarin"]

// 从索引 2 的位置开始删除 1 个元素，插入“trumpet”
var myFish = ['angel', 'clown', 'drum', 'sturgeon'];
var removed = myFish.splice(2, 1, "trumpet");
console.log(myFish); // ["angel", "clown", "trumpet", "sturgeon"]
console.log(removed); // ["drum"]
~~~

------

#### 6. `copyWithin(target, start, end)`

> 作用

浅复制数组的一部分到同一数组中的另一个位置，不会改变原数组的长度，**会改变原数组**

> 返回值

返回改变后的数组

> 参数

- `target`：必选，0 为基底的索引，复制序列到该位置
  - 如果是负数，`target` 将从末尾开始计算；
  - 如果 `target` >= `arr.length`，将不会发生拷贝；
  - 如果 `target` 在 `start` 之后，复制的序列将被修改以符合 `arr.length；`；
- `start`：可选，0 为基底的索引，开始复制元素的起始位置
  - 如果是负数，`start` 将从末尾开始计算；
  - 如果 `start` 被忽略，`copyWithin` 将会从 0 开始复制；

- `end`：可选，0 为基底的索引，开始复制元素的结束位置，`copyWithin` 将会拷贝到该位置，但不包括 `end` 这个位置的元素
  - 如果是负数， `end` 将从末尾开始计算
  - 如果 `end` 被忽略，`copyWithin` 方法将会一直复制至数组结尾（默认为 `arr.length`）

> 示例：

~~~JS
[1, 2, 3, 4, 5].copyWithin(-2); // [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3); // [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4); // [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1); // [1, 2, 3, 3, 4]
~~~

------

#### 7. ` fill(value, start, end)`

> 作用：

用一个固定值填充一个数组中从 起始索引 到 终止索引 内的全部元素，不包括终止索引，**会改变原数组**

> 返回类型

返回改变后的数组

> 参数

- `value`： 必选，用来填充数组元素的值
- `start`： 可选，起始索引，默认值为 0，也可以是负数
- `end`： 可选，终止索引，默认值为 `arr.length`，也可以是负数

> 示例：

~~~JS
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
~~~

------

### 查找数组中某个元素的位置：

####  1. `indexOf(searchElement, fromIndex)`

> 作用：

返回在数组中可以找到给定元素的 **第一个** 索引，查找顺序是 **从前向后** 查询数组

> 返回类型：

返回一个 <number>

 如果存在该元素，就返回该元素的索引值；如果不存在，则返回 -1

> 参数：

- `searchElement`：必选，要查找的元素
- `fromIndex`：可选，开始查找的位置，包含 `fromIndex`，默认值为 0
  - 如果该索引值大于或等于数组长度，意味着不会在数组里查找，返回 -1；
  - 如果参数是一个负值，则将其作为数组末尾的一个抵消，例 -2 表示从倒数第二个元素开始查找，如果抵消后的索引值仍小于 0，则整个数组都将会被查询

> 示例：

~~~JS
const array = [2, 9, 9];
array.indexOf(7);     // -1
array.indexOf(9);     // 1
array.indexOf(9, 2);  // 2
~~~

------

#### 2. `lastIndexOf(searchElement, fromIndex)`

> 作用：

返回指定元素在数组中的 **最后一个** 的索引，查找顺序是从 **后往向前** 查询数组

> 返回类型：

返回一个 <number>

 如果存在该元素，就返回该元素的索引值；如果不存在，则返回 -1

> 参数

- `searchElement`：必选，要查找的元素
- `fromIndex`：可选，从此位置开始逆向查找，默认值为 `arr.length - 1`，即整个数组都被查找
  - 如果该值大于或等于数组的长度，则整个数组会被查找
  - 如果参数是负值，将其视为从数组末尾向前的偏移，如果其绝对值大于数组长度，则方法返回 -1，即数组不会被查找

> 示例：

~~~JS
var array = [2, 5, 9, 2];
var index = array.lastIndexOf(2);    // index is 3
index = array.lastIndexOf(7);        // index is -1
index = array.lastIndexOf(2, 2);     // index is 0
index = array.lastIndexOf(2, -1);    // index is 3
~~~

------

#### 3. `findIndex(callbackFn, thisArg)`

> 作用：

返回数组中满足提供的测试函数的 第一个 元素的索引

> 返回类型：

返回一个 <number>

 如果通过提供测试函数，立即就返回 第一个元素 的索引；否则，返回 -1

> 参数

- `callbackFn`：必选，针对数组中的每个元素，都会执行该回调函数，也可以使用箭头函数的形式，

  执行时会自动传入下面三个参数：

  - `element`：当前元素
  - `index`：当前元素的索引
  - `array`：调用 `findIndex` 的数组，就是被遍历的数组

- `thisArg`：可选，执行 `callbackFn` 时作为 `this` 对象的值，如果没有被提供，将会使用 `undefined`

> 示例：

~~~JS
// 查找数组中首个质数元素的索引

// 定义回调函数，用于对每一个元素进行测试
function isPrime(element, index, array) {
  var start = 2;
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false;
    }
  }
  return element > 1;
}

console.log([4, 6, 8, 12].findIndex(isPrime)); // -1
console.log([4, 6, 7, 12].findIndex(isPrime)); // 2
~~~

------

### 查找数组中某个元素的值：

####  1. `find(callbackFn, thisArg)`

> 作用：

返回数组中满足提供的测试函数的 第一个 元素的值

> 返回类型：

如果通过提供测试函数，立即就返回 第一个元素 的值；否则，返回 `undefined`

> 参数

- `callbackFn`：必选，针对数组中的每个元素，都会执行该回调函数，也可以使用箭头函数的形式，

  执行时会自动传入下面三个参数：

  - `element`：当前元素
  - `index`：当前元素的索引
  - `array`：调用 `findIndex` 的数组，就是被遍历的数组

- `thisArg`：可选，执行 `callbackFn` 时作为 `this` 对象的值，如果没有被提供，将会使用 `undefined`

> 示例：

~~~JS
// 寻找数组中的第一个质数

// 定义回调函数，用于对每一个元素进行测试
function isPrime(element, index, array) {
  let start = 2;
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false;
    }
  }
  return element > 1;
}

console.log([4, 6, 8, 12].find(isPrime)); // undefined
console.log([4, 5, 8, 12].find(isPrime)); // 5

~~~

------

#### 2. `Array.prototype.includes(searchElement, fromIndex)`

> 作用：

用来判断一个数组中是否包含一个指定的值

另外，`includes()` 被有意设计为通用方法，它不要求 `this` 值是数组对象，所以它可以被用于其他类型的对象 ，比如类数组对象

> 返回类型：

返回一个 Boolean 类型，如果包含则返回 `true`，否则返回 `false`

> 参数

- `searchElement`：必选，需要查找的元素值
- `fromIndex`： 可选，从`fromIndex` 索引处开始查找 `searchElement`，默认为 0，
  - 参数可以为负值，如果 `array.length + fromIndex` 小于 0，则整个数组都会被搜索
  - 如果 `fromIndex` 大于等于数组的长度，则将直接返回 `false`，且不搜索该数组

> 示例：

~~~JS
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true
~~~

------

####  3. `String.prototype.includes(searchString, position)`

用于判断一个字符串是否包含另外一个字符串，根据情况返回 true 或 false。

注意，该方法是区分大小写的

> 参数

- `searchString`：必选，要在此字符串中搜索的字符串
- `position` ：可选，从当前字符串的哪个索引位置开始搜寻子字符串，默认值为 0

> 示例：

~~~JS
var str = 'To be, or not to be, that is the question.';

console.log(str.includes('To be'));       // true
console.log(str.includes('question'));    // true
console.log(str.includes('nonexistent')); // false
console.log(str.includes('To be', 1));    // false
console.log(str.includes('TO BE'));       // false
~~~

------

####  4. `flat(depth)`

> 作用：

将所有元素与遍历到的子数组中的元素合并为一个新数组返回，用于 **扁平化嵌套数组** 和 **移除数组中的空项**

> 返回类型

返回一个新数组

> 参数

- `depth` ：可选，指定要提取嵌套数组的结构深度，默认值为 1，设置为 Infinity，可展开任意深度的嵌套数组

> 示例：

~~~JS
var arr1 = [1, 2, [3, 4]];
arr1.flat(); // [1, 2, 3, 4]

var arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat(); // [1, 2, 3, 4, [5, 6]]

var arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2); // [1, 2, 3, 4, 5, 6]

//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// 移除数组中的空项
var arr4 = [1, 2, , 4, 5];
arr4.flat(); // [1, 2, 4, 5]
~~~

------

###  数组迭代：

#### 1. `every(callbackFn, thisArg)`

> 作用：

测试一个数组内的 **所有元素** 是否都能通过某个指定函数的测试

注意：若收到一个空数组，此方法在任何情况下都会返回 `true`

> 返回类型：

它返回一个布尔值，true 或者 false

> 参数

- `callbackFn`：必选，用来测试每个元素的函数，它可以接收三个参数：
  - `element`：必选，用于测试的当前值
  - `index`：可选，用于测试的当前值的索引
  - `array`：可选，调用 `every` 的当前数组，也就是被遍历的数组
- `thisArg`：可选，执行 `callback` 时使用的 `this` 值

> 示例：

~~~JS
// 检测所有数组元素是否都大于 10

// 定义一个作用于每个元素的测试回调函数
function isBigEnough(element, index, array) {
  return element >= 10;
}
[12, 5, 8, 130, 44].every(isBigEnough);   // false

// 箭头函数简写
[12, 5, 8, 130, 44].every(x => x >= 10); // false
~~~

------

####  2. `some(callbackFn, thisArg)`

> 作用：

测试一个数组内是不是至少有 1 个元素通过了某个指定函数的测试

> 返回类型：

它返回一个布尔值，true 或者 false

> 参数

- `callbackFn`：必选，用来测试每个元素的函数，它可以接收三个参数：
  - `element`：数组中正在处理的元素
  - `index`：可选，数组中正在处理的元素的索引值
  - `array`：可选，调用 `every` 的当前数组，也就是被遍历的数组
- `thisArg`：可选，执行 `callback` 时使用的 `this` 值

> 示例：

~~~JS
// 检测在数组中是否有元素大于 10
function isBiggerThan10(element, index, array) {
  return element > 10;
}
[2, 5, 8, 1, 4].some(isBiggerThan10);  // false

// 箭头函数简写
[2, 5, 8, 1, 4].some(x => x > 10);  // false
~~~

------

####  3. `forEach(callbackFn, thisArg)`

> 作用：

对数组的每个元素执行一次给定的函数

注意：不对未初始化的值进行任何操作（稀疏数组，例如：`[1, 3, , 7]`）

> 返回类型：

返回 `undefined`

> 参数

- `callbackFn`：必选，为数组中每个元素执行的函数，它可以接收三个参数：
  - `element`：必选，数组中正在处理的元素
  - `index`：可选，数组中正在处理的元素的索引值
  - `array`：可选，正在操作的数组，也就是被遍历的数组
- `thisArg`：可选，执行 `callback` 时使用的 `this` 值

> 示例：

~~~JS
// 遍历打印数组元素
const array1 = ['a', 'b', 'c'];
array1.forEach(element => console.log(element));

// 在迭代期间修改数组
const words = ['one', 'two', 'three', 'four'];
words.forEach((word) => {
  if (word === 'two') {
    words.shift(); //'one' 将从数组中删除
  }
});
console.log(words); // ['two', 'three', 'four']
~~~

> 备注：

- 除了抛出异常以外，没有办法中止或跳出 `forEach()` 循环

- 如果你需要中止或跳出循环，`forEach()` 方法不是应当使用的工具
- 若你需要提前终止循环，你可以使用：
  - 一个简单的 for 循环
  - for...of / for...in 循环

------

####  4. `map(callbackFn, thisArg)`

> 作用：

创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成，不改变原数组

如果有以下情形，则不该使用 `map`：

- 你不打算使用返回的新数组
- 你没有从回调函数中返回值

> 返回类型：

返回一个新数组，每个元素都是回调函数的返回值

> 参数：

- `callbackFn`：必选，生成新数组元素的函数，函数会被自动传入三个参数：
  - `currentValue`：必选，数组中正在处理的元素
  - `index`：可选，数组中正在处理的元素的索引值
  - `array`：可选，正在操作的数组，也就是被遍历的数组
- `thisArg`：可选，执行 `callback` 时使用的 `this` 值

> 示例：

~~~JS
// 求数组中每个元素的平方根
const numbers = [1, 4, 9];
const roots = numbers.map((num) => Math.sqrt(num)); // [1, 2, 3]

// 使用 map 重新格式化数组中的对象
const kvArray = [
  { a: 1, b: 10 },
  { a: 2, b: 20 },
  { a: 3, b: 30 },
];
const reformattedArray = kvArray.map(({ a, b }) => ({ [a]: b })); 
// [{1: 10}, {2: 20}, {3: 30}],
~~~

------

#### 5. `flatMap(callbackFn, thisArg)`

> 作用：

对原数组的每个成员执行一个函数，并对返回值执行 `flat()` 将数组嵌套降少一层，不改变原数组

> 返回类型：

返回一个新数组

> 参数：

- `callbackFn`：必选，生成一个新数组中的元素的函数，可以传入三个参数：
  - `currentValue`：必选，数组中正在处理的元素
  - `index`：可选，数组中正在处理的元素的索引值
  - `array`：可选，正在操作的数组，也就是被遍历的数组
- `thisArg`：可选，执行 `callback` 时使用的 `this` 值

> 示例：

~~~JS
var arr1 = [1, 2, 3, 4];

arr1.map(x => [x * 2]); // [[2], [4], [6], [8]]

arr1.flatMap(x => [x * 2]); // [2, 4, 6, 8]

arr1.flatMap(x => [[x * 2]]);// [[2], [4], [6], [8]]
~~~

------

#### 6. `filter(callbackFn, thisArg)`

> 作用：

又称为筛选函数，它会创建一个数组，能够通过提供的测试函数的元素的浅拷贝的数组

> 参数：

- `callbackFn`：必选，用来测试数组中每个元素的函数

  返回 `true` 表示该元素通过测试，保留该元素，`false` 则不保留；它接受以下三个参数：

  - `element`：数组中当前正在处理的元素
  - `index`：正在处理的元素在数组中的索引
  - `array`：调用了 `filter()` 的数组本身

- `thisArg`：可选，执行 `callbackFn` 时，用于 `this` 的值

> 返回类型：

返回一个新数组

> 示例：

~~~JS
// 找出数组中所有的素数
const array = [-3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13];
const filterd = array.filter((element, index, array) => {
  for (let i = 2; element > i; i++) {
    if (element % i === 0) {
      return false;
    }
  }
  return element > 1;
})
console.log(filterd); // [2, 3, 5, 7, 11, 13]

// 对数组每一项进行操作和筛选
let words = ['spray', 'limit', 'exuberant', 'destruction', 'elite', 'present'];
const modifiedWords = words.filter((word, index, arr) => {
  arr[index + 1] += ' extra';
  return word.length < 6;
});

console.log(modifiedWords); // ["spray"]
~~~

------

### 数组归并累加

#### 1. `reduce(callbackFn, initialValue)`

> 作用：

又称为累加函数，对数组中的每个元素按序执行一个由您提供的 **reducer** 函数，每一次运行 **reducer** 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值

注意：如果数组仅有一个元素且没有提供初始值 `initialValue`，或者有提供 `initialValue` 但数组为空，那么此唯一值将被返回且 `callbackfn` 不会被执行

使用 `.reduce()` 替换 `.filter().map()`：

- 使用 [`Array.filter()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 和 [`Array.map()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 会遍历数组两次，而使用具有相同效果的 [`Array.reduce()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 只需要遍历一次，这样做更加高效。
- 如果你喜欢 `for` 循环，你可用使用 [`Array.forEach()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 以在一次遍历中实现过滤和映射数组

> 返回类型：

使用 `reducer` 回调函数遍历整个数组后的结果，也就说最后一次调用 `reducer` 返回的结果

> 参数：

- `callbackFn`：必选，一个 reducer 函数，包含四个参数：
  - `previousValue`：上一次调用 `callbackFn` 时的返回值。在第一次调用时，若指定了初始值 `initialValue`，其值则为 `initialValue`，否则为数组索引为 0 的元素 `array[0]`
  - `currentValue`：数组中正在处理的元素。在第一次调用时，若指定了初始值 `initialValue`，其值则为数组索引为 0 的元素 `array[0]`，否则为 `array[1]`
  - `currentIndex`：数组中正在处理的元素的索引。若指定了初始值 `initialValue`，则起始索引号为 0，否则从索引 1 起始
  - `array`：用于遍历的数组
- `initialValue` ：可选，作为第一次调用 `callback` 函数时参数 `previousValue` 的值。若指定了初始值 `initialValue`，则 `currentValue` 则将使用数组第一个元素；否则 `previousValue` 将使用数组第一个元素，而 `currentValue` 将使用数组第二个元素

> 示例：

~~~JS
// 求数组所有值的和
let total = [ 0, 1, 2, 3 ].reduce(
  ( previousValue, currentValue ) => previousValue + currentValue
    , 0
)
console.log(total) // 6

// 累加对象数组里的值
let sum = [{x: 1}, {x: 2}, {x: 3}].reduce(
    (previousValue, currentValue) => previousValue + currentValue.x
    , 0
)
console.log(sum) // 6

// 将二维数组转化为一维
let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  ( previousValue, currentValue ) => previousValue.concat(currentValue)
    , []
)

// 计算数组中每个元素出现的次数
let names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice']
let countedNames = names.reduce((allNames ,name) => {
    if(name in allNames) allNames[name]++
    else allNames[name] = 1
    return allNames
}, {})
console.table(countedNames); // { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }

// 按属性对 object 分类
let people = [
  { name: 'Alice', age: 21 },
  { name: 'Max', age: 20 },
  { name: 'Jane', age: 20 }
];

function groupBy(objectArray, property) {
  return objectArray.reduce(function (acc, obj) {
    let key = obj[property]
    if (!acc[key]) {
      acc[key] = []
    }
    acc[key].push(obj)
    return acc
  }, {})
}

let groupedPeople = groupBy(people, 'age')
// groupedPeople is:
// {
//   20: [
//     { name: 'Max', age: 20 },
//     { name: 'Jane', age: 20 }
//   ],
//   21: [{ name: 'Alice', age: 21 }]
// }

// 数组去重
let myArray = ['a', 'b', 'a', 'b', 'c', 'e', 'e', 'c', 'd', 'd', 'd', 'd']
let myArrayWithNoDuplicates = myArray.reduce(function (previousValue, currentValue) {
  if (previousValue.indexOf(currentValue) === -1) {
    previousValue.push(currentValue)
  }
  return previousValue
}, [])
console.log(myArrayWithNoDuplicates); //  ['a', 'b', 'c', 'e', 'd']
~~~

------

### 数组遍历

#### 1. `forEach(item => {})`

#### 2. `for(let i=0; i<arr.length; i++) {}`

#### 3. `for(let item of arr) {}` (推荐)

#### 4. `for(let index in arr) {}`

## 二、对象常用方法

#### 1. `Object.assign(target, ...sources)`

> 作用：

将对象所有的 **可枚举的**（`Object.propertyIsEnumerable()` 返回 true） **自有属性**（`Object.hasOwnProperty()` 返回 true）从一个或多个源对象 **浅拷贝** 到目标对象

**自身可枚举属性**：即其自身定义的属性，而不是其原型链上的枚举属性

> 返回类型：

返回目标目标对象，target

> 参数：

- `target`：目标对象，接收源对象属性的对象，也是修改后的返回值
- `sources`： 源对象，可有多个，包含将被合并的属性

> 示例：

~~~JS
// 复制对象
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }

// 对象合并
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };
const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.

// 合并具有相同属性的对象，属性会被后续参数中具有相同属性的其他对象覆盖
const o1 = { a: 1, b: 1, c: 1 };
const o2 = { b: 2, c: 2 };
const o3 = { c: 3 };
const obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }

// 基本类型会被包装为对象，例如 string
const v1 = 'abc';
const v2 = true;
const v3 = 10;
const v4 = Symbol('foo');
const obj = Object.assign({}, v1, null, v2, undefined, v3, v4);
// Primitives will be wrapped, null and undefined will be ignored.
// Note, only string wrappers can have own enumerable properties.
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
~~~

------

#### 2. `Object.is(value1, value2)`

> 作用：

判断两个值是否为同一个值

注意：

1. `Object.is()` 与 `==` 不同， `Object.is` 不会强制转换两边的值
2. `Object.is()` 与 `===`也不相同，它们对待有符号的 0 和 NaN 不同

如果满足以下任意条件则两个值相等：

- 都是 `undefined`
- 都是 `null`
- 都是 `true` 或都是 `false`
- 都是相同长度、相同字符、按相同顺序排列的字符串
- 都是相同对象（意味着都是同一个对象的值引用）
- 都是数字且
  - 都是 `+0`
  - 都是 `-0`
  - 都是 `NaN`
  - 都是同一个值，非零且都不是 `NaN`

> 返回类型：

返回一个 Boolean 值

> 参数：

- `value1`： 被比较的第一个值
- `value2`：被比较的第二个值

> 示例：

~~~JS
// Case 1: Evaluation result is the same as using ===
Object.is(25, 25);                // true
Object.is('foo', 'foo');          // true
Object.is('foo', 'bar');          // false
Object.is(null, null);            // true
Object.is(undefined, undefined);  // true
Object.is(window, window);        // true
Object.is([], []);                // false
var foo = { a: 1 },
    bar = { a: 1 };
Object.is(foo, foo);              // true
Object.is(foo, bar);              // false

// Case 2: Signed zero
Object.is(0, -0);                 // false
Object.is(+0, -0);                // false
Object.is(-0, -0);                // true
Object.is(0n, -0n);               // true

// Case 3: NaN
Object.is(NaN, 0/0);              // true
Object.is(NaN, Number.NaN)        // true
~~~

------

#### 3. `Object.keys(obj)`

> 作用：

返回一个由一个给定对象的 **自身可枚举属性** 组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致

> 返回类型：

返回一个 数组

> 参数：

- `obj`： 要返回其枚举自身属性的对象

> 示例：

~~~JS
// 简单数组
const arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// 类数组对象
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// 具有随机键顺序的类数组对象
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo 是一个不可枚举的属性
const myObj = Object.create({}, {
  getFoo: {
    value() { return this.foo; }
  }
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
~~~

------

#### 4. `Object.create(proto, propertiesObject)`

> 作用：

用于创建一个新对象，使用现有的对象来作为新创建对象的原型（prototype）

> 返回类型：

返回一个新对象，带着指定的原型对象及其属性

> 参数：

- `proto`： 必选，新创建对象的原型对象，参数可以是 `null` 或 除基本类型包装对象以外的 对象
- `propertiesObject` ：可选，如果该参数被指定且不为 `undefined`，则该传入对象的 自有可枚举属性 将为新创建的对象添加指定的属性值和对应的属性描述符，如果 propertiesObject 参数不是 null 也不是对象，则抛出一个 TypeError 异常

> 示例：

~~~JS
var obj = {
    a: 1, b: 2
}
var baz = Object.create(obj);
console.log(baz);    // {} 空对象
console.log(baz.a);  // 1，可以访问到原对象中的属性，则说明，原对象 obj 就是新对象 baz 的原型
~~~

------

#### 5. `Object.defineProperties(obj, props)`

> 作用：

直接在一个对象上定义新的属性或修改现有属性

> 返回类型：

返回该对象

> 参数：

- `obj`：在其上定义或修改属性的对象

- `props`：要定义其可枚举属性或修改的属性描述符的对象，对象中存在的属性描述符主要有两种：数据描述符和访问器描述符，描述符具有以下键：

  - `configurable`：`true` 

    只有该属性描述符的类型可以被改变，并且该属性可以从对应对象中删除， 默认为 `false`

  - `enumerable`： `true` 

    只有在枚举相应对象上的属性时该属性显现， 默认为 `false`

  - `value`：属性的值

    与属性关联的值，可以是任何有效的 JavaScript 值（数字，对象，函数）， 默认为 `undefined`

  - `writable`：`true`

    只有与该属性相关联的值被 assignment operator (en-US) 改变时， 默认为 `false`

  - `get`：该属性的 getter 函数

    如果没有 getter 则为 `undefined`，函数返回值将被用作属性的值， 默认为 `undefined`

  - `set`： 该属性的 setter 函数

    如果没有 setter 则为 `undefined`，函数将仅接受参数赋值给该属性的新值， 默认为 `undefined`

> 示例：

~~~JS
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
});
console.table(obj); // { property1: true, property2: 'Hello' }
~~~

### 遍历对象

#### 1. `for(let val in obj) {}` (推荐)

#### 2. `Object.values(obj)`

#### 3. `Object.keys(obj).forEach(key => {});`

#### 4. `Object.getOwnPropertyNames(obj).forEach(key => {})`

### 三、字符串常用方法

### 查找字符串

#### 1. `charAt(index)`

> 作用：

从一个字符串中返回指定的字符

> 返回类型：

返回指定索引位置的字符

> 参数：

- `index`：可选，一个介于 0 和字符串长度减 1 之间的整数。默认为 0

> 示例：

~~~JS
var myString = 'jQuery FTW!!!';
console.log(myString.charAt(7)); // F
~~~

------

#### 2.`indexOf(searchString, position)`

> 作用：

给定一个参数：要搜索的子字符串，搜索整个调用字符串，并返回指定子字符串第一次出现的索引；

给定第二个参数：一个数字，该方法将返回指定子字符串在大于或等于指定数字的索引处的第一次出现的索引；

查找顺序：**→ 从左到右 **

> 返回类型：

返回查找的字符串 `searchValue` 的第一次出现的索引，如果没有找到，则返回 `-1`

> 参数：

- `searchValue`：可选，要搜索的子字符串

  如果不带参数调用方法，`searchString` 将被强制转换为 字符串 `"undefined"`

  当使用空字符 ` ` 串搜索时的返回值时：

  - 如果没有第二个实参，或者有第二个实参的值小于调用字符串的长度，返回值与第二个实参的值相同
  - 如果有第二个参数，其值大于或等于字符串的长度，则返回值为字符串的长度

- `position` ：可选，指定一个开始查找的索引起点，默认为 `0`

  - 如果 `position` 大于调用字符串的长度，则该方法根本不搜索调用字符串

  - 如果 `position` 小于零，该方法的行为就像 `position` 为 `0` 时一样

> 示例：

~~~JS
const str = 'Brave new world';
console.log(str.indexOf('new')); // 6

// 检测是否存在某字符串
'Blue Whale'.indexOf('Blue') !== -1  // true;

// 统计一个字符串中某个字母出现的次数
const str = 'To be, or not to be, that is the question.';
let count = 0;
let position = str.indexOf('e');
while (position !== -1) {
  count++;
  position = str.indexOf('e', position + 1);
}
console.log(count); // 4
~~~

------

#### 3. `lastIndexOf(searchString, position)`

> 作用：

返回指定值最后一次出现的索引

查找顺序：**→ 从左向右 **

注意：该方法是区分大小写的

> 返回类型：

返回指定值最后一次出现的索引 (该索引仍是以从左至右 0 开始记数的)，如果没找到则返回 -1

> 参数：

- `searchValue`：可选，一个字符串，表示被查找的值

  如果`searchValue`是空字符串，则返回 `fromIndex`

- `fromIndex`：可选，指定一个结束查找的索引终点，默认值为  `+Infinity` ，相当于 `str.length`

  - 如果 `fromIndex >= str.length` ，则会搜索整个字符串

  - 如果 `fromIndex < 0` ，则等同于 `fromIndex == 0`

> 示例：

~~~JS
'canal'.lastIndexOf('a');     //  3
'canal'.lastIndexOf('a', 2);  //  1
'canal'.lastIndexOf('a', 0);  // -1
'canal'.lastIndexOf('c', -5); //  0
'canal'.lastIndexOf('');      //  5
'canal'.lastIndexOf('', 2);   //  2
~~~

------

#### 4. `match(regexp)`

> 作用：

检索返回一个字符串匹配正则表达式的结果

> 返回类型

- 如果使用 g 标志，则将返回一个数组，包含正则表达式匹配的所有结果，但不会返回捕获组

- 如果未使用 g 标志，则仅返回第一个完整匹配及其相关的捕获组（`Array`）

  在这种情况下，返回的项目将具有如下所述的其他属性：

  - `groups`: 一个命名捕获组对象

    其键是捕获组名称，值是捕获组，如果未定义命名捕获组，则为 `undefined`

  - `index`: 匹配的结果的开始位置

  - `input`: 搜索的字符串

- 如果没有找到匹配项，则返回一个信息数组或`null`

> 参数：

- `regexp`：一个 正则表达式 对象 
  - 如果传入一个非正则表达式对象，则会隐式地使用 `new RegExp(obj)` 将其转换为一个 `RegExp`
  - 如果没有给出任何参数并直接使用 match() 方法，你将会得到一 个包含空字符串的数组 ：`[""]` 

> 示例：

~~~JS
// 使用 g 标志
var str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
var regexp = /[A-E]/gi;
var matches_array = str.match(regexp);
console.log(matches_array); // ['A', 'B', 'C', 'D', 'E', 'a', 'b', 'c', 'd', 'e']

// 未使用 g 标志
var str = 'For more information, see Chapter 3.4.5.1';
var re = /see (chapter \d+(\.\d)*)/i;
var found = str.match(re);
console.log(found);

// [ 'see Chapter 3.4.5.1',
//        'Chapter 3.4.5.1',
//        '.1',
//        index: 22,
//        input: 'For more information, see Chapter 3.4.5.1' 
// ]

// 'see Chapter 3.4.5.1' 是整个匹配。
// 'Chapter 3.4.5.1' 被'(chapter \d+(\.\d)*)'捕获。
// '.1' 是被'(\.\d)'捕获的最后一个值。
// 'index' 属性 (22) 是整个匹配从零开始的索引。
// 'input' 属性是被解析的原始字符串。
~~~

------

#### 5. `search(regexp)`

> 作用：

用于检索与正则表达式相匹配的子字符串

> 返回类型

如果找到，返回与 `regexp` 相匹配的子串的首次匹配项的索引，否则返回 `-1`

> 参数：

- `regexp`：一个 `正则表达式` 对象

  如果传入一个非正则表达式对象 `regexp`，则会隐式地将其转换为正则表达式对象

> 示例：

~~~JS
var intRegex = /[0-9 -()+]+$/;  
var myNumber = '999';
var isInt = myNumber.search(intRegex);
console.log(isInt); // 0
~~~

------

#### 6. `includes(searchString, position)`

> 作用：

用于判断一个字符串是否包含另外一个字符串

注意，该方法是区分大小写

> 返回类型

根据情况返回 true 或 false

> 参数：

- `searchString`：必选，要在此字符串中搜索的字符串
- `position` ：可选，从当前字符串的哪个索引位置开始搜寻子字符串，默认值为 0

> 示例：

~~~JS
var str = 'To be, or not to be, that is the question.';

console.log(str.includes('To be'));       // true
console.log(str.includes('question'));    // true
console.log(str.includes('nonexistent')); // false
console.log(str.includes('To be', 1));    // false
console.log(str.includes('TO BE'));       // false
~~~

------

#### 7. `endsWith(searchString, length)`

> 作用：

用来判断当前字符串是否是以另外一个给定的子字符串“结尾”的

这个方法是大小写敏感的

> 返回类型

根据判断结果返回 `true` 或 `false`

> 参数：

- `searchString`：要搜索的子字符串
- `length` ：可选，表示一个结束搜索的索引位置，默认为 `str.length`

> 示例：

~~~JS
var str = "To be, or not to be, that is the question.";
alert( str.endsWith("question.") );  // true
alert( str.endsWith("to be") );      // false
alert( str.endsWith("to be", 19) );  // true
~~~

------

#### 1

### 操作字符串

#### 1. `concat(str, str1, ..., strN)`

> 作用：

将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回，该方法不会改变原字符串

注意：

- 如果参数不是字符串类型，它们在连接之前将会被转换成字符串
- 该方法是区分大小写的

> 返回类型：

返回一个新的字符串

> 参数：

- `strN`： 需要连接到 `str` 的字符串

> 示例：

~~~JS
let hello = 'Hello, '
console.log(hello.concat('Kevin', '. Have a nice day.'));
// Hello, Kevin. Have a nice day.

let greetList = ['Hello', ' ', 'Venkat', '!']
"".concat(...greetList)  // "Hello Venkat!"
~~~

------

#### 2. `replace(regexp|substr, newSubStr|function)`

> 作用：

用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串，不会改变原字符串

> 返回类型

返回一个新的替换后的字符串

> 参数：

- `regexp` (pattern)：一个`RegExp`对象或者其字面量该正则所匹配的内容会被第二个参数的返回值替换掉

  `substr` (pattern)：一个将被 `newSubStr` 替换的 `字符串`，仅第一个匹配项会被替换

- `newSubStr` (replacement)：用于替换掉第一个参数在原字符串中的匹配部分的`字符串`

  该字符串中可以内插一些特殊的变量名，参考下面的[使用字符串作为参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#使用字符串作为参数)

  | 变量名    | 代表的值                                                     |
  | :-------- | :----------------------------------------------------------- |
  | `$$`      | 插入一个 "$"。                                               |
  | `$&`      | 插入匹配的子串。                                             |
  | `$``      | 插入当前匹配的子串左边的内容。                               |
  | `$'`      | 插入当前匹配的子串右边的内容。                               |
  | `$n`      | 假如第一个参数是 `RegExp` 对象，并且 n 是个小于 100 的非负整数，那么插入第 n 个括号匹配的字符串。提示：索引是从 1 开始。如果不存在第 n 个分组，那么将会把匹配到到内容替换为字面量。比如不存在第 3 个分组，就会用“$3”替换匹配到的内容。 |
  | `$<Name>` | 这里 `Name` 是一个分组名称。如果在正则表达式中并不存在分组（或者没有匹配），这个变量将被处理为空字符串。只有在支持命名分组捕获的浏览器中才能使用。 |

- `function` (replacement)：一个用来创建新子字符串的函数

  该函数的返回值将替换掉第一个参数匹配到的结果，参考下面的[指定一个函数作为参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#指定一个函数作为参数)

  | 变量名              | 代表的值                                                     |
  | :------------------ | :----------------------------------------------------------- |
  | `match`             | 匹配的子串。（对应于上述的$&。）                             |
  | `p1,p2, ...`        | 假如 replace() 方法的第一个参数是一个`RegExp`对象，则代表第 n 个括号匹配的字符串。（对应于上述的$1，$2 等。）例如，如果是用 `/(\a+)(\b+)/` 这个来匹配，`p1` 就是匹配的 `\a+`，`p2` 就是匹配的 `\b+`。 |
  | `offset`            | 匹配到的子字符串在原字符串中的偏移量。（比如，如果原字符串是 `'abcd'`，匹配到的子字符串是 `'bc'`，那么这个参数将会是 1） |
  | `string`            | 被匹配的原字符串。                                           |
  | `NamedCaptureGroup` | 命名捕获组匹配的对象                                         |

> 示例：

~~~JS
// 使用字符串
var str = 'Twas the night before Xmas...';
var newstr = str.replace("a", 'A');
console.log(newstr);  // TwAs the night before Xmas...

// 使用正则表达式
var str = 'Twas the night before Xmas...';
var newstr = str.replace(/xmas/i, 'Christmas');
console.log(newstr);  // Twas the night before Christmas...

var myString = '999 JavaScript Coders';
console.log(myString.replace(new RegExp( "999", "gi" ), "The")); 
// The JavaScript Coders

// 交换字符串中的两个单词
var re = /(\w+)\s(\w+)/;
var str = "John Smith";
var newstr = str.replace(re, "$2, $1");
console.log(newstr);// Smith, John 
~~~

------

#### 3. `slice(beginIndex, endIndex)`

> 作用：

用于提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串

注意：提取的新字符串包括 `beginIndex` 但不包括 `endIndex`

> 返回类型

返回一个从原字符串中提取出来的新字符串

> 参数：

- `beginIndex`：必选，从该索引处开始提取原字符串中的字符，默认为  0 

  如果值为负数，会被当做 `strLength + beginIndex` 看待

- `endIndex`：可选，在该索引处结束提取字符串

  - 如果省略该参数，`slice()` 会一直提取到字符串末尾
  - 如果该参数为负数，则被看作是 `strLength + endIndex`

> 示例：

~~~JS
var text="excellent"
text.slice(0,4) // "exce"
text.slice(2,4) // "ce"
~~~

------

#### 4. `split(separator, limit)`

> 作用：

使用指定的分隔符字符串将一个 `String` 对象分割成子字符串数组，以指定的分割字串来决定每个拆分的位置

> 返回类型

返回源字符串以分隔符出现位置分隔而成的一个 数组

> 参数：

- `separator`：指定表示每个拆分应发生的点的字符串，`separator` 可以是一个 字符串 或 正则表达式
  - 如果纯文本分隔符包含多个字符，则必须找到整个字符串来表示分割点
  - 如果在 str 中省略或不出现分隔符，则返回的数组包含一个由整个字符串组成的元素
  - 如果分隔符为空字符串，则将 str 原字符串中每个字符的数组形式返回
- `limit`：可选，一个整数，限定返回的分割片段数量
  - 当提供此参数时，split 会在指定分隔符的每次出现时分割该字符串，但在限制条目已放入数组时停止
  - 如果在达到指定限制之前达到字符串的末尾，它可能仍包含少于限制的条目，新数组中不返回剩下的文本

> 示例：

~~~JS
// 以空格作为分隔符，而且限制返回值个数
var myString = "Hello World. How are you doing?";
var splits = myString.split(" ", 3);
console.log(splits); // ["Hello", "World.", "How"]

// 使用一个数组来作为分隔符
const myString = 'ca,bc,a,bca,bca,bc';

const splits = myString.split(['a','b']);
// myString.split(['a','b']) is same as myString.split(String(['a','b']))
console.log(splits);  //["c", "c,", "c", "c", "c"]

// 用 split() 来颠倒字符串顺序
const str = 'asdfghjkl';
const strReverse = str.split('').reverse().join(''); // 'lkjhgfdsa'
~~~

------

#### 5. `substr(start, length)`

> 作用：

返回一个字符串中从指定位置开始到指定字符数的字符

注意：未来可能会被移除掉，如果可以的话，使用 `substring()` 替代它

> 返回类型

返回一个新字符串

> 参数：

- `start`：开始提取字符的位置，如果为负值，则被看作 `strLength + start`
  - 如果 `start` 为正值，且大于或等于字符串的长度，则 `substr` 返回一个空字符串
  - 如果 `start` 为负值，则 `substr` 把它作为从字符串末尾开始的一个字符索引
  - 如果 `start` 为负值且 `abs(start)` 大于字符串的长度，则 `substr` 使用 0 作为开始提取的索引
- `length`： 可选，提取的字符数
  - 如果 `length` 为 0 或负值，则 `substr` 返回一个空字符串
  - 如果忽略 `length`，则 `substr` 提取字符，直到字符串末尾

> 示例：

~~~JS
var str = "abcdefghij";
console.log("(1,2): "    + str.substr(1,2));   // (1,2): bc
console.log("(-3,2): "   + str.substr(-3,2));  // (-3,2): hi
console.log("(-3): "     + str.substr(-3));    // (-3): hij
console.log("(1): "      + str.substr(1));     // (1): bcdefghij
console.log("(-20, 2): " + str.substr(-20,2)); // (-20, 2): ab
console.log("(20, 2): "  + str.substr(20,2));  // (20, 2):
~~~

------

#### 6. `substring(indexStart, indexEnd)`

> 作用：

返回一个字符串在开始索引到结束索引之间的一个子集，或从开始索引直到字符串的末尾的一个子集

> 返回类型

包含给定字符串的指定部分的新字符串

> 参数：

- `indexStart`：需要截取的第一个字符的索引，该索引位置的字符作为返回的字符串的首字母
- `indexEnd`：可选，一个 0 到字符串长度之间的整数，以该数字为索引的字符不包含在截取的字符串内
  - 如果省略 `indexEnd`，`substring` 提取字符一直到字符串末尾
  - 如果 `indexStart` 等于 `indexEnd`，`substring` 返回一个空字符串
  - 如果 `indexStart` 大于 `indexEnd`，则 `substring` 的执行效果就像两个参数调换了一样
- 如果任一参数小于 0 或为 `NaN`，则被当作 0
- 如果任一参数大于 `stringName.length`，则被当作 `stringName.length`

> 示例：

~~~JS
var anyString = "Mozilla";
console.log(anyString.substring(0,3)); // "Moz"
console.log(anyString.substring(3,0)); // "Moz"
~~~

------

#### 7. `toLowerCase()`

> 作用：

将调用该方法的字符串值转为小写形式，不会改变原字符串，因为 JS 中字符串的值是不可改变的

> 返回类型

一个新的字符串，表示转换为小写的调用字符串

> 示例：

~~~JS
console.log( "ALPHABET".toLowerCase() ); // "alphabet"
~~~

------

#### 8. `toUpperCase()`

> 作用：

将调用该方法的字符串转为大写形式，不会改变原字符串，因为 JS 中字符串的值是不可改变的

如果调用该方法的值不是字符串类型会被强制转换

> 返回类型

一个新的字符串，表示转换为大写的调用字符串

> 示例：

~~~JS
console.log('alphabet'.toUpperCase()); // 'ALPHABET'
~~~

------

#### 9. `repeat(count)`

> 作用：

构造并返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本

> 返回类型

返回一个包含指定字符串的指定数量副本的新字符串

> 参数：

- `count`：介于 `0` 和 `+Infinity` 之间的整数，表示在新构造的字符串中重复了多少遍原字符串
  - 重复次数不能为负数。
  - 重复次数必须小于 infinity，且长度不会大于最长的字符串。

> 示例：

~~~JS
"abc".repeat(-1) // RangeError: repeat count must be positive and less than inifinity
"abc".repeat(0)  // ""
"abc".repeat(1)  // "abc"
"abc".repeat(2)  // "abcabc"
~~~

------

#### 10. `trim()`

> 作用：

会从一个字符串的两端删除空白字符，不会改变原字符串

> 返回类型

一个代表调用字符串两端去掉空白的新字符串。

> 示例：

~~~JS
const greeting = '   Hello world!   ';
console.log(greeting); // "   Hello world!   ";
console.log(greeting.trim()); // "Hello world!";
~~~

------

### 遍历字符串

#### 1. `for (let i of string)`

~~~JS
for (let codePoint of 'abc') {
  console.log(codePoint);
}
// "a"
// "b"
// 
~~~



## 四、补充知识点

#### 判断数据类型

1. 原始类型

   - 对于原始类型来说，`typeof`除了 `null` 都可以显示正确的类型；

   - 如果想判断 `null` 的话可以使用 `variable === null`

   ~~~JS
   typeof 1 // 'number'
   typeof '1' // 'string'
   typeof undefined // 'undefined'
   typeof true // 'boolean'
   typeof Symbol() // 'symbol'
   typeof 1n // bigint
   
   let a = null;
   a === null // true
   ~~~

2. 引用类型

   - `typeof`除了函数都会显示 `object`，无法显示具体的引用类型
   - `instanceof` 只可用于判断具体的自定义对象类型
   - **最终手段**：`Object.prototype.toString.call()`，无论原始类型还是数据类型都可以准确判断

   ~~~JS
   // typeof
   typeof [] // 'object'
   typeof {} // 'object'
   typeof console.log // 'function'
   
   // instanceof
   const Person = function() {}
   const p1 = new Person()
   p1 instanceof Person // true
   
   var str1 = new String('hello world')
   str1 instanceof String // true
   
   
   // Object.prototype.toString.call()
   Object.prototype.toString.call(null) // '[object Null]'
   Object.prototype.toString.call(18) // '[object Number]'
   Object.prototype.toString.call('Hello World!') // '[object String]'
   Object.prototype.toString.call([]) // '[object Array]'
   Object.prototype.toString.call({}) // '[object Object]'
   Object.prototype.toString.call(function() {}) // '[object Function]'
   ~~~

#### `valueOf()`

1. `Object.prototype.valueOf()`：返回指定对象的原始值

   - 很少需要自己调用 `valueOf` ，当遇到需要原始值的对象时，JavaScript 会自动调用它
   - 默认情况下，`valueOf` 方法由 `Object`后代的每个对象继承，如果对象没有原始值，则 `valueOf` 将返回对象本身，你可以重写 `Object.prototype.valueOf()` ，而不是使用默认的 `Object` 的方法

   ~~~JS
   var obj = {a: 1};
   console.log(obj.valueOf()); // {a: 1}
   // <==>
   console.log(obj); // {a: 1}
   ~~~

2. `String.prototype.valueOf()`：返回一个 `String`对象的原始值

   - 此方法通常由 JavaScript 在内部调用，而不是在代码中显式调用

   ~~~JS
   var x = new String('Hello world');
   console.log(x);              // String对象： {'Hello world'}
   console.log(x.valueOf());    // 'Hello world'
   ~~~

3. `Number.prototype.valueOf()`：返回一个被 `Number` 对象包装的原始值

   - 该方法通常是由 JavaScript 引擎在内部隐式调用的，而不是由用户在代码中显式调用的

   ~~~JS
   var num = 10;
   console.log(num.valueOf()); // 10
   // <=>
   console.log(num);           // 10
   console.log(typeof num);    // number
   ~~~

#### `toString()`

1. `Object.prototype.toString()`：返回一个表示该对象的字符串

   - 当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用
   - 默认情况下，`toString()` 方法被每个 `Object` 对象继承
   - 如果此方法在自定义对象中未被覆盖，`toString()` 返回 "[object *type*]"，其中 `type` 是对象的类型
   - 你也可以重写 `toString()`，去覆盖默认的 `toString` 方法

   ~~~JS
   var obj = {a:1, b: 2};
   console.log(obj); // {a: 1, b: 2}
   console.log(obj.toString()); // [object Object]
   ~~~

2. `Array.prototype.toString()`：返回一个字符串，表示指定的数组及其元素

   ~~~JS
   const array1 = [1, 2, 'a', '1a'];
   console.log(array1.toString()); //  "1,2,a,1a"
   ~~~

3. `String.prototype.toString()`：返回一个字符串，表示指定的字符串

   ~~~JS
   var x = new String('Hello world');
   console.log(x);              // String对象： {'Hello world'}
   console.log(x.toString());   // 'Hello world'
   ~~~

4. `Number.prototype.toString(radix)`：返回指定 `Number` 对象的字符串表示形式

   - `radix`：指定要用于数字到字符串的转换的基数 (从 2 到 36)
     - 如果未指定 `radix` 参数，默认值为 10
     - 如果转换的基数大于 10，则会使用字母来表示大于 9 的数字，比如基数为 16 的情况，则使用 a 到 f 的字母来表示 10 到 15
     - 进行数字到字符串的转换时，建议**用小括号将要转换的目标括起来**，防止出错

   ~~~JS
   var count = 10;
   console.log(count.toString());    // 输出 '10'
   console.log((17).toString());     // 输出 '17'
   console.log((17.2).toString());   // 输出 '17.2'
   ~~~

#### 展开运算符 `...`

该语法只能用于 可迭代对象，注意 Object 不是 可迭代对象，例如： `{ a: 1}`

- 原生可迭代对象（ JS 内置）
  1. String
  2. Array
  3. Set
  4. NodeList 类数组对象
  5. Arguments 类数组对象
  6. Map

1. 在函数调用时使用展开语法（等价于 apply 的方式）

   ~~~JS
   function myFunction(x, y, z) { }
   var args = [0, 1, 2];
   
   myFunction.apply(null, args);
   // <=>
   myFunction(...args);
   ~~~

2. 构造字面量数组时使用展开语法在对象中的使用

   ~~~JS
   // 数组的合并
   var a = [1, 2];
   var b = [0, ...a, 3]; // [0, 1, 2, 3]
   
   // 数组的分割
   var [a, ...b] = [0, 1, 2];
   console.log(b); // [1, 2]
   
   // 数组的拷贝（浅拷贝）
   var a = [1, 2];
   var b = [...a]; // [1, 2]
   
   // 数组合并
   var arr1 = [0, 1, 2];
   var arr2 = [3, 4, 5];
   var arr3 = [...arr1, ...arr2];
   ~~~

3. 构造字面量对象时使用展开语法

   ~~~JS
   var obj1 = { foo: 'bar', x: 42 };
   var obj2 = { foo: 'baz', y: 13 };
   
   // 对象浅拷贝
   var clonedObj = { ...obj1 }; // { foo: "bar", x: 42 }
   
   // 对象合并
   var mergedObj = { ...obj1, ...obj2 }; // { foo: "baz", x: 42, y: 13 }
   
   // 对象分隔
   let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
   console.log(z) // {a: 3, b: 4}
   ~~~

#### 深拷贝——递归实现

~~~JS
function deepCopy(obj) {
  if(typeof obj !== 'object') return obj;
  let newObj = (obj instanceof Array) ? [] : {};
  for(let key in obj) {
    if(typeof obj[key] !== 'object') newObj[key] = obj[key]
    else newObj[key] = deepCopy(obj[key])
  }
  return newObj;
}
~~~

#### 防抖——debounce

~~~JS
function debounce(fn, delay) {
  let timer = null;
  return function() {
    let _this = this;
    let args = arguments;
    timer && clearTimeOut(timer);
    timer = setTimeOut(function(){
      fn.apply(_this, args)
    }, delay)
  }
}
~~~

#### 节流——throttle

~~~JS
function throttle(fn, delay) {
  let timer = null;
  return funtion() {
    let _this = this;
    let args = arguments;
    if(!timer) {
       timer = setTimeOut(function() {
         timer = null;
         fn.apply(_this, args)
       }, delay)
    }
  }
}
~~~

#### 立即执行的函数（IIFE）

> 定义

1. 立即调用的匿名函数又被称作立即调用的函数表达式（IIFE）

2. 它类似于函数声明，但由于被包含在括号中，所以会被解释为函数表达式
3. 紧跟在第一组括号后面的第二组括号会立即调用前面的函数表达式，而且第二组括号里面可以传参数，然后传递给第一组括号，也就是传给匿名的函数使用
4. **根据 IIFE 的特点：位于IIFE中的代码在其外部是无法访问的，所以，常用来隔离变量作用域**
5. **==IIFE== 结合 ==闭包== 实现一个 私有变量**

> 作用：

1. IIFE 不会导致闭包相关的内存问题，因为不存在对这个匿名函数的引用，因此，只要函数执行完毕，其作用域链就可以被销毁
2. 第三方库会存在大量的变量和函数，在 ES5 环境下为了避免变量污染，开发者想到的解决办法就是使用立即执行函数来隔离变量作用域

> 实例：

~~~JS
// 先理解 for 循环
for(var i=0;i<5;i++) {
  console.log('i-',i) // i-0 i-1 i-2 i-3 i-4
  var j = 0;
  ++j;
  if(j<3) console.log('j-',j) // j-1 j-1 j-1 j-1 j-1
}

// IIFE
(function(val) {
  console.log(val);
})("我是自执行匿名函数");

// 使用 var 定义的变量是没有作用域的，也就是说它是一个全局变量
for(var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4 
}
console.log(i); // 5

// 使用 IIFE 隔离 var 变量的作用域 
(function() {
  // 块级作用域
  for (var i = 0; i < 5; i++) {
    console.log(i); // 0 1 2 3 4  
  }
})();
console.log(i); // ReferenceError: i is not defined

// 在 ES6 以后，新增了块级作用域的概念，我们也用let来声明变量，为其添加作用域
for (let i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4  
}
console.log(i); // ReferenceError: i is not defined

// IIFE 结合 闭包 实现一个 私有变量
const getOrderId = (function() {
  let count = 0;
  return function() {
    ++count;
    return `第${count}次 调用函数，当前 count 的值为：${count}`;
  };
})();
console.log(getOrderId()); // 0
console.log(getOrderId()); // 1
console.log(getOrderId()); // 2
console.log(getOrderId()); // 3
console.log(count);        // ReferenceError: count is not defined
~~~

> 用法：

立即执行函数有两种写法：

- `(function(){})()` 匿名函数包裹在一个括号运算符中，后面再跟一个小括号
- `(function(){}())` 匿名函数后面跟一个小括号，然后整个包裹在一个括号运算符中

要想立即执行函数做到立即执行，要注意两点：

- 函数体后面要有小括号 `()`
- 函数体不能是函数声明，必须是函数表达式，即函数体必须是有被括号 `()` 包裹起来的 
- 如果想将括号加在声明式函数后面如：`function test(){}`，运行之后会报错，因为不符合js的语法，想让其通过浏览器的语法检查，就必须添加符号，比如：
  - 用 () 将函数体包括起来、
  - 在函数 function 前面添加 `+` 、`-` 、`!` 、`~` 、`void` 、`new` 等符号



### 数组：

#### 数组去重

1. reduce + indexOf + push 

   ~~~JS
   function removeRepeated(arr) {
     return arr.reduce(function (previousValue, currentValue) {
       if (previousValue.indexOf(currentValue) === -1) {
          previousValue.push(currentValue)
       }
     }, [])
   }
   ~~~

2. for + indexOf + push 

   ~~~JS
   function removeRepeated {
     var temp = [];
     for (var i = 0; i < arr.length; i++) {
       if (temp .indexOf(arr[i]) === -1) {
         temp .push(arr[i])
       }
     }
     return temp;
   }
   ~~~

3. from + Set（ES6 中最常用）

   ~~~JS
   function removeRepeated(arr) {
     return Array.from(new Set(arr));
   }
   ~~~

4. for循环嵌套 + splice （ES5 中最常用）

   ~~~JS
   function removeRepeated(arr) {            
     for(var i=0; i<arr.length; i++){
       for(var j=i+1; j<arr.length; j++){
         if(arr[i]==arr[j]){
           arr.splice(j,1);
           j--;
         }
       }
     }
     return arr;
   }
   ~~~

#### 数组排序

1. 冒泡排序

   ~~~JS
   // 升序
   function arrSort(arr) {
     for (let i = 0; i < arr.length - 1; i++) {
       for (let j = 0; j < arr.length - 1 - i; j++) {
         if (arr[j] > arr[j + 1]) { // 升序用 > ; 降序用 < ;
           let temp = arr[j];
           arr[j] = arr[j + 1];
           arr[j + 1] = temp;
         }
       }
     }
     return arr;
   }
   ~~~

2. sort

   ~~~JS
   function arrSort(arr) {
     return arr.sort((a, b) => a - b) // 升序用 a - b ; 降序用 b - a ;
   }
   ~~~

#### 数组最大最小值

1. for遍历

   ~~~JS
   function arrMax(arr) {
     var max = arr[0];
     for(let i = 1; i < arr.length; i++) {
       arr[i] > max ? max = arr[i] : null
     } 
     return max
   }
   ~~~

2. sort

   ~~~JS
   function arrMax(arr) {
     let temp = arr.sort((a,b) => b - a)
     return temp[0]
   }
   ~~~

3. reduce + Math.max

   ~~~JS
   const arr = [1, 2, 3];
   const max = arr.reduce((a, b) => Math.max(a, b), -Infinity);
   ~~~

#### 数组最大最小值

------

#### 数组扁平化

1. flat(depth)

   ~~~JS
   const arr = [1, [2, [3, 4]]];
   const arr1 = arr.flat(1) // [1, 2, [3, 4]]
   const arr2 = arr.flat(2) // [1, 2, 3, 4]
   const arr3 = arr.flat(Infinity) // [1, 2, 3, 4]
   ~~~

2. 递归

   ~~~JS
   function flatten(arr) {
     let res = [];
     arr.forEach((item, i, arr) => {
       if (Array.isArray(item)) res = res.concat(flatten(item));
       else res.push(arr[i])
     })
     return res;  
   }
   ~~~

3. reduce

   ~~~JS
   function flatten(arr) {
       return arr.reduce(function(prev, next){
           return prev.concat(Array.isArray(next) ? flatten(next) : next)
       }, [])
   }
   ~~~

### 对象：

#### 动态获取设置对象的键值对

1. 在将一个 `变量` 设置为对象里面的一个 `键` 时，需要用 中括号 `[]` 把 `键` 包裹起来

   ~~~js
   let key = 'age';
   let value = 17;
   let odj = {}
   let obj = {[key]: value}
   console.log(obj); // {age: 17}
   ~~~

------

2. 已获取到对象里的某一个 键值对，用一个`变量` 获取该键值对的 `值` 时，需要用 中括号 `[]` 把 `键` 包裹起来

   ~~~JS
   let odj = {};
   let obj = {age: 17};
   let property = 'age';
   let value = obj[property];
   console.log(value); // 17
   ~~~



### 字符串：

#### 字符串模板

1. 模板字符串（template string）是增强版的字符串，用反引号（`）标识

2. 它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量或函数

3. 工作中用到比较多

~~~JS
// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 模板字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// 模板字符串之中还能调用函数
function fn() {
  return "Hello World";
}    
`foo ${fn()} bar`
// foo Hello World bar
~~~



