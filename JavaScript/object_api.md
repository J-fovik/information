#  对象的常用方法

## 一、属性操作

#### 1、属性访问

使用点号（`.`）或方括号（`[]`）来访问对象的属性

```js
let person = { name: 'John', age: 30 };
console.log(person.name); // 输出: 'John'
console.log(person['age']); // 输出: 30
```





#### 2、属性赋值

使用点号或方括号来给对象的属性赋值。

```js
let person = {};
person.name = 'John';
person['age'] = 30;
```



#### 3、删除属性

使用 `delete` 关键字来删除对象的属性。

```js
let person = { name: 'John', age: 30 };
delete person.age;
```



## 二、属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性。

- for...in：循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）

- Object.keys(obj)：返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名

- Object.getOwnPropertyNames(obj)：回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名

- Object.getOwnPropertySymbols(obj)：返回一个数组，包含对象自身的所有 Symbol 属性的键名

- Reflect.ownKeys(obj)：返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举

上述遍历，都遵守同样的属性遍历的次序规则：

- 首先遍历所有数值键，按照数值升序排列
- 其次遍历所有字符串键，按照加入时间升序排列
- 最后遍历所有 Symbol 键，按照加入时间升序排

```js
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```





## 三、对象的方法

关于对象新增的方法，分别有以下：

- Object.is()
- Object.assign()
- Object.getOwnPropertyDescriptors()
- Object.setPrototypeOf()，Object.getPrototypeOf()
- Object.keys()，Object.values()，Object.entries()
- Object.fromEntries()



#### Object.is()

严格判断两个值是否相等，与严格比较运算符（===）的行为基本一致，不同之处只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身

```js
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```







#### Object.getOwnPropertyDescriptors()

返回指定对象所有自身属性（非继承属性）的描述对象

```js
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```



#### Object.setPrototypeOf()

`Object.setPrototypeOf`方法用来设置一个对象的原型对象

```js
Object.setPrototypeOf(object, prototype)

// 用法
const o = Object.setPrototypeOf({}, null);
```



#### Object.getPrototypeOf()

用于读取一个对象的原型对象

```js
Object.getPrototypeOf(obj);
```



#### Object.keys()

返回自身的（不含继承的）所有可遍历（enumerable）属性的键名的数组

```js
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj)
// ["foo", "baz"]
```

```js
data() {
	return {
        // 参数
		dataForm: {
			images: [],
			etypeId: '',
			wbtext:''
		},
        // 规则
		rules: {
			etypeId: '请选择情绪标签',
			wbtext: '请输入内容',
			images: '请添加图'
		}
	}
},
methods: {
	submitFn() {
		let that = this
		let flag = true
		Object.keys(that.rules).forEach(key => {
			if (!that.dataForm[key]) {
				flag = false;
				that.$modal.msg(that.rules[key])
			}
		})
		if (!flag) {
			return;
		}
	}
}
```





#### Object.values()

返回自身的（不含继承的）所有可遍历（enumerable）属性的键对应值的数组

```js
const obj = { foo: 'bar', baz: 42 };
Object.values(obj)
// ["bar", 42]
```







#### Object.entries()

返回一个对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对的数组

```js
const obj = { foo: 'bar', baz: 42 };
Object.entries(obj)
// [ ["foo", "bar"], ["baz", 42] ]
```

```js
let subjects = [];
let scores = [];
const list = [{ Math: '99', History: '78' }, { Math: '99', History: '88' }];
// 遍历数组
list.forEach(entry => {
  Object.entries(entry).forEach(([subject, score]) => {
    subjects.push(subject);
    scores.push(score);
  });
});
console.log(subjects); // ["Math", "History", "Math", "History"]
console.log(scores);   // ["99", "78", "99", "88"]
```

```js
const object1 = {
  a: 'somestring',
  b: 42,
};
for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}
// "a: somestring"
// "b: 42"

```



#### Object.assign()

`Object.assign()`方法用于对象的合并，将源对象`source`的所有可枚举属性，复制到目标对象`target`

`Object.assign()`方法的第一个参数是目标对象，后面的参数都是源对象

```javascript
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

注意：`Object.assign()`方法是浅拷贝，遇到同名属性会进行替换





#### Object.fromEntries()

用于将一个键值对数组转为对象

```js
Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42]
])
// { foo: "bar", baz: 42 }
```



#### Object.groupBy()

语法：Object.groupBy(items, callbackFn)

静态方法根据提供的回调函数返回的字符串值对给定可迭代对象中的元素进行分组。返回的对象具有每个组的单独属性，其中包含组中的元素的数组。

```js
const inventory = [
  { name: "芦笋", type: "蔬菜", quantity: 5 },
  { name: "香蕉", type: "水果", quantity: 0 },
  { name: "山羊", type: "肉", quantity: 23 },
  { name: "樱桃", type: "水果", quantity: 5 },
  { name: "鱼", type: "肉", quantity: 22 },
];

```

```js
const result = Object.groupBy(inventory, ({ type }) => type);
// const result = Object.groupBy(inventory, o=>o.type);
/* 结果是：
{
  蔬菜: [ { name: "芦笋", type: "蔬菜", quantity: 5 },],
  水果: [{ name: "香蕉", type: "水果", quantity: 0 },{ name: "樱桃", type: "水果", quantity: 5 }],
  肉: [{ name: "山羊", type: "肉", quantity: 23 },{ name: "鱼", type: "肉", quantity: 22 }]
}
*/
```

```js
function myCallback({ quantity }) {
  return quantity > 5 ? "ok" : "restock";
}
const result2 = Object.groupBy(inventory, myCallback);
/* 结果是：
{
  restock: [ { name: "芦笋", type: "蔬菜", quantity: 5 },
  			{ name: "香蕉", type: "水果", quantity: 0 },
    		{ name: "樱桃", type: "水果", quantity: 5 } ],
  ok: [   { name: "山羊", type: "肉", quantity: 23 },
  		 { name: "鱼", type: "肉", quantity: 22 }]
}
*/
```





#### 扩展运算符**...**

在解构赋值中，未被读取的可遍历的属性，分配到指定的对象上面

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```

注意：解构赋值必须是最后一个参数，否则会报错

```js
// 展开对象
let obj1 = { a: 1, b: 2 };
let obj2 = { ...obj1, c: 3, d: 4 };
console.log(obj2); // 输出: { a: 1, b: 2, c: 3, d: 4 }
```





## 参考文献

- https://es6.ruanyifeng.com/#docs/object