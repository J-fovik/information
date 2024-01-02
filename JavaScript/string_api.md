# 字符串的常用方法

## 一、操作方法

### 增

这里增的意思并不是说直接增添内容，而是创建字符串的一个副本，再进行操作

除了常用`+`以及`${}`进行字符串拼接之外，还可通过`concat`



#### concat

用于将一个或多个字符串拼接成一个新字符串

```js
let stringValue = "hello ";
let result = stringValue.concat("world");
console.log(result); // "hello world"
console.log(stringValue); // "hello"
```



### 删

这里的删的意思并不是说删除原字符串的内容，而是创建字符串的一个副本，再进行操作

常见的有：

- slice()
- substr()
- substring()





#### slice()

**返回截取出来的部分字符串**

**特点: 包前不包后, 填写负整数**

**语法: 字符串.slice(开始索引, 结束索引)**

```js
let stringValue = "hello world";
console.log(stringValue.slice(3)); // "lo world"
console.log(stringValue.slice(3, 7)); // "lo w"
```



#### substr()

**返回截取出来的部分字符串**

**语法: 字符串.substr(开始索引, 多少个)**

```js
let stringValue = "hello world";
console.log(stringValue.substr(3)); // "lo world"
console.log(stringValue.substr(3, 7)); // "lo worl"
```



#### substring()

**返回截取出来的部分字符串**

**特点: 包前不包后**

 语法: 字符串.substring(开始索引, 结束索引)

```js
let stringValue = "hello world";
console.log(stringValue.substring(3)); // "lo world"
console.log(stringValue.substring(3,7)); // "lo w"
```







### 改

这里改的意思也不是改变原字符串，而是创建字符串的一个副本，再进行操作

常见的有：

- trim()、trimLeft()、trimRight()

- repeat()
- padStart()、padEnd()
- toLowerCase()、 toUpperCase()



#### trim()、trimLeft()、trimRight()、trimStart()、trimEnd()

删除前、后或前后所有空格符，**再返回新的字符串**

```js
let stringValue = " hello world ";
let trimmedStringValue = stringValue.trim();
console.log(stringValue); // " hello world "
console.log(trimmedStringValue); // "hello world"
```



#### repeat()

**接收一个整数参数**，表示要将字符串**复制多少次**，然后**返回拼接所有副本后的结果**

```js
let stringValue = "na ";
let copyResult = stringValue.repeat(2) // na na 
```



#### padEnd()

**复制字符串**，如果小于指定长度，则在相应一边填充字符，直至满足长度条件

```js
let stringValue = "foo";
console.log(stringValue.padStart(6)); // " foo"
console.log(stringValue.padStart(9, ".")); // "......foo"
```



#### toLowerCase()、 toUpperCase()

大小写转化

```js
let stringValue = "hello world";
console.log(stringValue.toUpperCase()); // "HELLO WORLD"
console.log(stringValue.toLowerCase()); // "hello world"
```



### 查

除了通过索引的方式获取字符串的值，还可通过：

- chatAt()

- indexOf()

- startWith()

- includes()

  

#### charAt()

**返回给定索引位置的字符**=>如果没有该索引位置, 返回的是 空字符串

语法: 字符串.charAt(索引)

```js
let message = "abcde";
console.log(message.charAt(2)); // "c"
```



#### charCodeAt()

**返回该索引位置字符的 unicode 编码** => 如果没有该索引位置, 返回的是 NaN

语法: 字符串.charCodeAt(索引)

```js
let str = "Hello";
// 获取索引为 1 的字符的 UTF-16 代码单元值
let charCode = str.charCodeAt(1);
console.log(charCode); // 输出 101，因为字符 'e' 的 UTF-16 代码单元值是 101
```







#### indexOf()

从字符串开头去搜索传入的字符串，**并返回位置（如果没找到，则返回 -1 ）**

```js
let stringValue = "hello world";
console.log(stringValue.indexOf("o")); // 4
console.log(stringValue.indexOf("z")); // -1
```





#### includes()

从字符串中搜索传入的字符串，并**返回一个表示是否包含的布尔值**

```js
let message = "foobarbaz";
console.log(message.includes("bar")); // true
console.log(message.includes("bar")); // true
console.log(message.includes("qux")); // false
```





#### startWith()

**以...开头**，从字符串中搜索传入的字符串，并**返回一个表示是否包含的布尔值**

```js
let message = "foobarbaz";
console.log(message.startsWith("foo")); // true
console.log(message.startsWith("bar")); // false
```



#### endsWith()

**以...结尾，从字符串中搜索传入的字符串，返回一个表示是否包含的布尔值**

作用: 判断该字符串是否以该字符串片段结尾

**语法: 字符串.endsWith(字符串片段)**

```js
let message = "foobarbaz";
console.log(message.endsWith("foo")); // false
console.log(message.endsWith("baz")); // true
```









## 二、转换方法

#### split

把字符串按照**指定的分割符**，拆**分成数组中的每一项**

```js
let str = "12+23+34"
let arr = str.split("+") // [12,23,34]
```



#### Array.from()

将类数组对象或可迭代对象转换为一个新的数组。字符串是可迭代的，因此可以使用它将字符串转换为数组。

```js
let stringValue = "hello world";
let arrayValue = Array.from(stringValue);
console.log(arrayValue); // 输出：["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
```



#### 使用扩展运算符 ...

扩展运算符可以将字符串转换为一个包含每个字符的数组。

```js
let stringValue = "hello world";
let arrayValue = [...stringValue];
console.log(arrayValue); // 输出：["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
```











## 三、模板匹配方法

针对正则表达式，字符串设计了几个方法：

- match()
- search()
- replace()



#### match()

接收一个参数，可以是一个正则表达式字符串，也可以是一个` RegExp `对象，返回数组

```js
let text = "cat, bat, sat, fat";
let pattern = /.at/;
let matches = text.match(pattern);
console.log(matches[0]); // "cat"
```



#### search()

接收一个参数，可以是一个正则表达式字符串，也可以是一个` RegExp `对象，**找到则返回匹配索引，否则返回 -1**

```js
let text = "cat, bat, sat, fat";
let pos = text.search(/at/);
console.log(pos); // 1
```



#### replace()

**接收两个参数，第一个参数为匹配的内容，第二个参数为替换的元素（可用函数）**

**注意: 只能替换一个**

```js
let text = "cat, bat, sat, fat";
let result = text.replace("at", "ond");
console.log(result); // "cond, bat, sat, fat"
```