# 数组的常用方法

## 一、基本操作方法

### 增 

下面前三种是对原数组产生影响的增添方法，第四种则不会对原数组产生影响

- push()
- unshift()
- splice()
- concat()


#### push()

`push()`方法接收任意数量的参数，并将它们**添加到数组末尾**，**返回数组的最新长度**

```js
let colors = ['yellow','purple']; // 创建一个数组
let count = colors.push("red", "green"); // 推入两项
console.log(count) // 4
console.log(colors) // ['yellow','purple',"red", "green"]
```



#### unshift()

unshift()在**数组开头添加**任意多个值，然后**返回新的数组长度**

```js
let colors = ['yellow','purple']; // 创建一个数组
let count = colors.unshift("red", "green"); // 从数组开头推入两项
console.log(count); // 4
console.log(colors) // ["red", "green",'yellow','purple']
```



#### splice

传入三个参数，分别是**开始位置、0（要删除的元素数量）、插入的元素**，**返回删除数组**

```js
let colors = ["red", "green", "blue"];
let removed = colors.splice(1, 0, "yellow", "orange")
console.log(colors) // ["red", "yellow", "orange", "green", "blue"]
console.log(removed) // []
```



#### concat()

首先会创建一个当前数组的副本，然后再把它的参数**添加到副本末尾**，最后**返回这个新构建的数组**，不会影响原始数组

```js
let colors = ["red", "green", "blue"];
let colors2 = colors.concat("yellow", ["black", "brown"]);
console.log(colors); // ["red", "green","blue"]
console.log(colors2); // ["red", "green", "blue", "yellow", "black", "brown"]
```



#### 扩展元素符`...`

```js
// 展开数组
let array1 = [1, 2, 3];
let array2 = [...array1, 4, 5, 6];
console.log(array2); // 输出: [1, 2, 3, 4, 5, 6]
```

```js
// 在函数调用中展开数组
function sum(a, b, c) {
  return a + b + c;
}
let numbers = [1, 2, 3];
let result = sum(...numbers);
console.log(result); // 输出: 6
```

```js
// 展开对象
let obj1 = { a: 1, b: 2 };
let obj2 = { ...obj1, c: 3, d: 4 };
console.log(obj2); // 输出: { a: 1, b: 2, c: 3, d: 4 }
```

```js
// 在函数调用中展开对象
function mergeObjects(obj1, obj2) {
  return { ...obj1, ...obj2 };
}
let objA = { a: 1, b: 2 };
let objB = { b: 3, c: 4 };
let mergedObj = mergeObjects(objA, objB);
console.log(mergedObj); // 输出: { a: 1, b: 3, c: 4 }
```





### 删

下面三种都会影响原数组，最后一项不影响原数组：

- pop()
- shift()
- splice()
- slice()



#### pop()

 `pop()` 方法**用于删除数组的最后一项**，同时减少数组的` length` 值，**返回被删除的项**

```js
let colors = ["red", "green"]
let item = colors.pop(); // 取得最后一项
console.log(item) // green
console.log(colors.length) // 1
```



#### shift()

` shift() `方法**用于删除数组的第一项**，同时减少数组的` length` 值，**返回被删除的项**

```js
let colors = ["red", "green"]
let item = colors.shift(); // 取得第一项
console.log(item) // red
console.log(colors.length) // 1
```



#### splice()

传入两个参数，分别是**开始位置，删除元素的数量**，**返回包含删除元素的数组**

```js
let colors = ["red", "green", "blue"];
let removed = colors.splice(0,1); // 删除第一项
console.log(colors); // ["green", "blue"]
console.log(removed); //  ["red"]，删除的数据
```



#### slice()

 slice() 用于创建**一个包含原有数组中一个或多个元素的新数组**，不会影响原始数组

```js
let colors = ["red", "green", "blue", "yellow", "purple"];
let colors2 = colors.slice(1); // 从索引 1 开始到数组末尾的切片
let colors3 = colors.slice(1, 4); // 从索引 1 到索引 4（不包括索引 4）的切片
console.log(colors)   // ["red", "green", "blue", "yellow", "purple"]
concole.log(colors2); // ["green", "blue", "yellow", "purple"]
concole.log(colors3); // ["green", "blue", "yellow"]
```





#### filter()

创建一个新数组，仅包含满足指定条件的元素。

```js
let fruits = ['apple', 'orange', 'banana'];
let filteredFruits = fruits.filter(fruit => fruit !== 'orange');
console.log(filteredFruits); // 输出: ["apple", "banana"]
```





### 改
即修改原来数组的内容，常用`splice`
#### splice() 

传入三个参数，分别是**开始位置，要删除元素的数量，要插入的任意多个元素**，**返回删除元素的数组**，对原数组产生影响

```js
let colors = ["red", "green", "blue"];
let removed = colors.splice(1, 1, "red", "purple"); // 插入两个值，删除一个元素
console.log(colors); // ["red", "red", "purple", "blue"]
console.log(removed); //  ["green"]，删除元素的数组
```



### 查

即查找元素，返回元素坐标或者元素值

- indexOf()
- includes()
- find()



#### indexOf()

**返回要查找的元素在数组中的位置，如果没找到则返回 -1**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
numbers.indexOf(4) // 3
numbers.indexOf(6) // -1
```





#### lastIndexOf()

查找数组中某个元素**最后一次出现**的位置，**如果没找到则返回 -1**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
numbers.lastIndexOf(4) // 5
numbers.lastIndexOf(6) // -1
```



#### includes()

**返回要查找的元素在数组中的位置，找到返回`true`，否则`false`**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
numbers.includes(4) // true
```



#### find()

**返回第一个匹配的元素**

1. **element（元素）**: 当前被处理的元素。
2. **index（索引）**: 当前元素在数组中的索引。
3. **array（数组）**: 调用 `find` 方法的数组本身。

```js
const people = [
    {
        name: "Matt",
        age: 27
    },
    {
        name: "Nicholas",
        age: 29
    }
];
people.find((element, index, array) => element.age < 28) // // {name: "Matt", age: 27}
```

```js
let numbers = [1, 2, 3, 4, 5];
// 使用 find 方法查找数组中第一个大于 2 的元素
let result = numbers.find(function(element) {
  return element > 2;
});
console.log(result);  // 输出: 3，第一个大于 2 的元素是 3
```



#### findIndex()

返回数组中满足提供的测试函数的**第一个元素**的索引。如果**找不到，返回 -1**。

```js
let numbers = [1, 2, 3, 4, 5];
let index = numbers.findIndex(num => num > 2);
let index1 = numbers.findIndex(num => num > 6);
console.log(index); // 输出: 2
console.log(index1); // 输出: -1
```







## 二、排序方法

数组有两个方法可以用来对元素重新排序：

- reverse() 
- sort()



#### reverse()

顾名思义，将**数组元素方向反转**

```js
let values = [1, 2, 3, 4, 5];
values.reverse();
alert(values); // 5,4,3,2,1
```



#### sort()

sort()方法接受一个比较函数，用于判断哪个值应该排在前面

```js
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}
let values = [4, 5, 2, 1, 3];
values.sort(compare);
alert(values); // Outputs: 1, 2, 3, 4, 5
```

```js
// 默认排序
let values = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];
values.sort();
console.log(values); // 输出: [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]
```

```js
// 自定义排序
let values = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];
values.sort(function(a, b) {
  return a - b; // 升序排序
});
console.log(values); // 输出: [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]
```







## 三、转换方法

常见的转换方法有：

#### join()

join() 方法接收一个参数，即**字符串分隔符**，**返回包含所有项的字符串**

```js
let colors = ["red", "green", "blue"];
alert(colors.join(",")); // red,green,blue
alert(colors.join("||")); // red||green||blue
```

#### map()

通过对数组的每个元素应用提供的函数来**创建一个新数组**。

```js
let numbers = [1, 2, 3, 4, 5];
let squaredNumbers = numbers.map(num => num * num);
// squaredNumbers 现在为 [1, 4, 9, 16, 25]
```



#### filter()

通过提供的函数测试数组的每个元素，创建一个新数组，其中仅**包含满足条件的元素**。

```js
let numbers = [1, 2, 3, 4, 5];
let evenNumbers = numbers.filter(num => num % 2 === 0);
// evenNumbers 现在为 [2, 4]
```



#### reduce()

对数组的所有元素执行一个提供的函数，将它们归并为单个值。

```js
let numbers = [1, 2, 3, 4, 5];
let sum = numbers.reduce((acc, num) => acc + num, 0);
// sum 现在为 15
```



#### forEach()

对数组的每个元素执行提供的函数，没有返回值。

```js
let colors = ['red', 'green', 'blue'];
colors.forEach(color => console.log(color));
// 输出: "red", "green", "blue"
```

```js
let colors = ['red', 'green', 'blue'];
colors.forEach((color, index, array) => {
    array[index] = color.toUpperCase();
});
console.log(colors); // 输出: ["RED", "GREEN", "BLUE"]
```



#### slice()

 从数组中提取指定位置的子数组。

```js
let numbers = [1, 2, 3, 4, 5];
let slicedNumbers = numbers.slice(1, 4);
// slicedNumbers 现在为 [2, 3, 4]
```



#### concat()

连接两个或多个数组，并返回一个新数组。

```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combinedArray = arr1.concat(arr2);
// combinedArray 现在为 [1, 2, 3, 4, 5, 6]
```





## 四、迭代方法

常用来迭代数组的方法（都不改变原数组）有如下：

- some()
- every()
- forEach()
- filter()
- map()



#### some()

对数组每一项都运行传入的测试函数，如果**至少有1个元素返回 true** ，则这个方法**返回 true**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let someResult = numbers.some((item, index, array) => item > 2);
console.log(someResult) // true
```



#### every()

对数组每一项都运行传入的测试函数，如果**所有元素都返回 true** ，则这个方法**返回 true**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let everyResult = numbers.every((item, index, array) => item > 2);
console.log(everyResult) // false
```



#### forEach()

对数组每一项都运行传入的函数，没有返回值

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
numbers.forEach((item, index, array) => {
    // 执行某些操作
});
```



#### filter()

对数组每一项都运行传入的函数，**函数返回 `true` 的项会组成数组之后返回**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let filterResult = numbers.filter((item, index, array) => item > 2);
console.log(filterResult); // [3,4,5,4,3]
```



#### map()

对数组每一项都运行传入的函数，返回由每次函数调用的结果**构成的数组**

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let mapResult = numbers.map((item, index, array) => item * 2);
console.log(mapResult) // [2, 4, 6, 8, 10, 8, 6, 4, 2]
```





## 五、去重方法

1. 第一种

   - 准备一个空数组，遍历原数组

   - 挨个判断原数组中的每个元素是否在新数组中，不在就把元素加进去

   - 新数组中最后一定是唯一的元素

     ```js
     var data = [];
     for(var v of nums){
     if(data.indexOf(v) == -1){
           data.push(v);
       }
      }
      console.log(data)
     ```

2. 第二种

   - 前后比较   如果相同 删除后一个  

   - 前提 得先排序   只有排序才能把相同的挨在一起

     ```js
     var res = quick_sort(nums);
     for(var i=0;i<res.length-1;i++){
        if(res[i]==res[i+1]){
              res.splice(i+1,1);
                 i--;
         }
     }
     console.log(res)
     ```

3. 第三种

   - 数组.indexOf(元素,从哪个位置开始往后查找)

   - 返回元素第一次出现的位置  找不到返回-1

   - 始终从下一个元素到最后 查找这个元素是否在这个范围内

     ```JS
     for(var i=0;i<nums.length-1;i++){     
        var index = nums.indexOf(nums[i],i+1);
         if(index != -1){
      nums.splice(index,1);
            i--;
          }
      var index = nums.lastIndexOf(nums[i]);
      if(index != -1){
          nums.splice(index,1);
      }       
     }
     console.log(nums)
     ```

4. 第四种

   - 使用集合数据结构  

   - 集合里边的元素 一定唯一的  

   - 数组转成集合  集合再转回数组   

     ```JS
     var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1];
     var data = [...new Set(nums)];
     console.log(data);
     ```





## 六、构造函数新增的方法

#### Array.from()

用于从类似数组或可迭代对象创建数组

```js
// 从字符串创建数组
let str = 'hello';
let charArray = Array.from(str);
console.log(charArray); // 输出: ["h", "e", "l", "l", "o"]
```





#### Array.of()

用于创建一个具有可变数量参数的新数组，确保参数的数量和类型与创建的数组一致。

```js
let numbers = Array.of(1, 2, 3, 4, 5);
console.log(numbers); // 输出: [1, 2, 3, 4, 5]
let strings = Array.of('a', 'b', 'c');
console.log(strings); // 输出: ["a", "b", "c"]
```





## 七、实例对象新增的方法

关于数组实例对象新增的方法有如下：

- copyWithin()
- find()、findIndex()
- fill()
- entries()，keys()，values()
- includes()
- flat()，flatMap()



#### copyWithin()

将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组

参数如下：

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。

```js
[1, 2, 3, 4, 5].copyWithin(0, 3) // 将从 3 号位直到数组结束的成员（4 和 5），复制到从 0 号位开始的位置，结果覆盖了原来的 1 和 2
// [4, 5, 3, 4, 5] 
```



#### find()、findIndex()

`find()`用于找出第一个符合条件的数组成员

参数是一个回调函数，接受三个参数依次为当前的值、当前的位置和原数组

```js
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

`findIndex`返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`

```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的`this`对象。

```js
function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26
```



#### fill()

使用给定值，填充一个数组

```javascript
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```

还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置

```js
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

注意，如果填充的类型为对象，则是浅拷贝



#### entries()，keys()，values()

**`keys()`是对索引的遍历、`values()`是对键值的遍历，`entries()`是对索引和键值的遍历**

```js
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```



#### includes()

用于判断数组是否包含给定的值

```js
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```

方法的**第二个参数表示搜索的起始位置，默认为`0`**

参数为负数则表示倒数的位置

```js
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```



#### flat()，flatMap()

将数组扁平化处理，返回一个新数组，对原数据没有影响

```js
[1, 2, [3, 4]].flat()
// [1, 2, 3, 4]
```

`flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1

```js
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
```

`flatMap()`方法对原数组的每个成员执行一个函数相当于执行`Array.prototype.map()`，然后对返回值组成的数组执行`flat()`方法。该方法返回一个新数组，不改变原数组

```js
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```

`flatMap()`方法还可以有第二个参数，用来绑定遍历函数里面的`this`