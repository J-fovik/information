# 一、Typescript简介

## 1、概述

它是 JavaScript 的一个超集，支持 ECMAScript 6 标准，由微软公司开发的自由、开源的编程语言，扩展了javaScript 得语法，它的设计目标是开发大型应用，全部浏览器兼容

## 2、优点

- **它有更多的规则和类型限制：**代码具有更高的预测性、可控性，易于维护和调试；对模块、命名空间和面向对象的支持，更容易组织代码开发大型复杂程序。
- **它是一个超集**：是一个编译到纯 JS 的有类型定义的 JS 超集，遵循当前以及未来的 ECMAScript规范。不仅能兼容现有的 JS 代码，它也拥有兼容未来版本的
- **超强代码能力：**大多数 TS 的新增特性都是基于未来的 JS 提案。许多 TS 代码在将来很有可能会变成 ECMA 的标准。
- **面向对象增强：**TS是对面向对象思想进行了增强，使前端变成了强语言类型

## **3、为什么学习 ts**

- **JavaScript 发展迅速：**TypeScript 可以帮你降低 JavaScript 弱语言的脆弱性，帮你减少由于不正确类型导致错误产生的风险

- **需要强类型的 JavaScript**：TypeScript 把高级语言的强类型引入 JavaScript ，解决了防止在编写 JavaScript 代码时因为数据类型的转换造成的错误。

- **主流框架及最新特性的支持：**Angular 2.0 版本就开始集成 TypeScript，React 、Vue也要加入 TypeScript 的阵营，能用最新的语言特性，编写质量更高的 JavaScript。

  

# 二、安装ts

## 1、安装 ts

```js
 npm install -g typescript / cnpm install -g typescript 
```

## 2、查看版本

```js
tsc -v
```

![image-20211031133910195](https:i.loli.net/2021/10/31/qkTJ48frjVzgGho.png)

## 3、运行ts

1.新建ts 文件index.ts.

2.ts 语法在浏览器是无法运行得，所以需要编译成js 语法，使用如下命令 tsc index.ts    (缺点：每次更新，都需要重新编译）

3.初始化文件夹和自动监测,执行如下命令：

```js
tsc --init
```

4.自动监测同步 JS 文件,执行如下命令：

```js
tsc -w
```

5.所有的 TS 文件必须通过下列方式编译成 JS 文件后，才能必须页面执行

```js
使用终端指令自动编译 tsc --init  生成对应得 tsconfig.json文件
tsc -w  监测将ts文件编译成js文件。
```

# 三、ts的数据类型

![image-20211031134836670](https:i.loli.net/2021/10/31/JraSYnPXeuDq1ZA.png)

### 1、boolean类型：

前面是变量名称，后面是变量类型。布尔型是最简单的数据类型，它的值就是 true / false 值。

```js
let a:boolean= false;
```

### 2、number类型 

​     它不仅支持二进制、八进制和十进制及十六进制的格式

### 3、string 类型：

  使用 string 表示文本数据类型，使用双引号（ “）或单引号（‘）表示字符串，同时，还可以使用模版字符串，它           	可以定义多行文本和内嵌表达式。

```ts
let str:string = "过年好";
```

### 4、undefined /null

```ts
let u:undefined =undefined;
let n:null = null;
```

### 5、void 类型

 表示没有任何类型，常用于函数返回值

```js
function warnUser(): void {
    console.log("This is my warning message");
}

const fn1 = ():void=>{
    
    return // 或不写return
}
```

### 6、any 类型 

当一个变量的值源于用户输入、动态内容和第三方插件，在不清楚明确类型时，可以定义为 any 型；当只知道一部分值的类型时，也可定义为 any 型

```js
let a1: any = {}
```

### 7、unknown类型

它是 3.0 时引入类型，是 any 类型对应的安全类型，定义时比 any 类型更加严格，执行操作之前，必须进行某种形式的检查

```
let u1: unknown = 100
u1 = '新年快乐'
u1 = true
```

### 8、never类型

它是其它类型的子类型，代表从不会出现的值，never 变量只能被 never 类型所赋值，函数中它表现为抛出异常或无法执行到终止点

```js
 情形1
function loop(): never {
    while (true) { }
}
 情形2
function error(m: string): never {
    throw new Error(m)
}
```

### 9、数组类型

如下表示声明number 类型数组，成员都是number类型

```js
 第一种声明方式(字面量方式)
let arr1: any[] = [1, 3, 5, 6, 100, '100'];
 第二种声明方式(泛型方式)
let arr2: Array<any> = [1, 3, 5, 6, 100, '100', [10, 20]];
```

### 10、元组类型:

该类型是指定数组中每一个值具体类型，在赋值时必须遵循定义的顺序和类型，它是一种严格的数组

```js
let arr3: [string, number, boolean] = ['', 1, false]
```

### 11、object类型

表示非原始类型，也就是除number，string，boolean，symbol，null 或 undefined 之外的类型，如下代码

```js
let obj: object = {
    name: "李焕英",
    age: 20
}
```

### 12、枚举类型

```js
enum sex {
    male='男',
    female='女',
    unknown='未知'
}

let gender:sex= sex.male
```

### 13、多个类型,也叫联合类型

```js
let a：number|string|undefined = 100

```

### 14、 symbol 类型

 自ECMAScript 2015起，`symbol`成为了一种新的原生类型，就像`number`和`string`一样 

 `symbol`类型的值是通过`Symbol`构造函数创建的。  Symbols最大的特点是**不可改变且唯一的**。 

```ts
let sym1 = Symbol();
let sym2 = Symbol("key");
let sym3 = Symbol("key");

sym2 === sym3; // false, symbols是唯一的
```

symbols一般用做定义对象属性的键 也就是属性名key

```ts
let sym = Symbol();

let obj = {
    [sym]: "value"
};

console.log(obj[sym]); // "value"

```





# 四、ts 中的函数

ts 中的函数需要指明参数类型和返回的数据类型

```ts
let fn = function (m:string,n:number):void{
	
}
let fn1 = (m:string,n:number):string=>{
    return ``
}
```

## 1、可选参数和默认参数和剩余参数

可选参数？ 可以不用传，注意：如果还有其他参数，可选参数不能作为第一个参数

```ts
可选参数在参数后+？
function add(x:number,y?:number):number{
     console.log(y)
     return y?x+y:x
 }

 默认参数就是给参数赋值默认值
function fn1(name:string,sex:string ='男',age?:number):string{
    return `${name}---${age}---${sex}`
 }

 剩余参数
 ...arr
function fn2(name:string="老杨",age?:number,...arr:any[]):string{
     console.log(name,age,arr)
      return ``
} 

fn2(undefined,32,'男','北京','已婚')
```

# 五、接口interface 和type

**接口的定义**： 指定结构的类型，并执行类型的检测，起到限制和规范的作用，说白了接口得作用主要是用来创建一种数据类型。而该数据类型必须是符合接口定义的规范。（通俗的讲，要想和接口对接，需要满足接口的合适插头）

**接口的语法**：使用 interface 关键字，后面添加接口的名称，并使用大括号描述接口中各结构属性的类型，完成定义后，可以像普通的类型一样使用它

```ts
interface Person{
     name:string, 
     age:number,
  }
     
let p:Person = {
      name:"沈腾",
      age:40
  }
```

## 1、接口属性

接口属性有 普通属性, 可选属性(?)，只读属性(readonly)  

普通属性：就是正常的属性

可选属性： 在使用该接口时，可选属性可传可不传。

只读属性：只读属性使用后，不能再次修改。

```ts
interface Person{
    name?:string, 
    readonly age:number,
 }
   
let p:Person = {
     name:"沈腾",  可选属性 可传可不传
     age:40   只读属性不能修改
}

```



## 2、接口定义函数

```ts
 fn 表示一种函数数据类型
interface fn{
   (m:string):void
}

let f1:fn = function(m:string){
     console.log(m)
 }
f1('郑爽')
```

3、接口中使用函数

如果在接口中有函数，可以在接口中直接定义

```ts
interface Men{
     name?:string, 
     readonly age:number,
     say:(m:string,n:number)=>string  第一种写法
     say(m:string,n:number)：string  第二种写法
 }
```

也可以使用接口定义，再引入到接口中，这两种方式都可以实现

```ts
 interface Say{
     (m:string,n:number):string
  }
  
 interface Men{
     name?:string, 
     readonly age:number,
     say:Say
  }

  let bichen:Men = {
      name:"张碧晨",
      age:30,
      say:function(m:string,n:number):string{
          return `${this.name}不结婚也要${m}`
      }
  }
```

## 3、可索引接口

定义：主要是来用定义数组或对象数据类型的。

```ts
1. 定义一个数组数据类型
interface arr{
    [name:number]:any
}
let arr1:arr=['刘','关','张',100,{}];

2. 定义一个对象数据类型
 interface obj{
     [name:string]:any
 }
 let obj1:obj={
     "sex":"男",
     age:20
 }
```

使用案例:

```ts
 interface child{
     name:string,
     age:number,
     say:()=>string,
     like:obj
 }

 let liuyan:child={
     name:"刘衍",
     age:20,
     say:():string=>{
       return ``
     },
     like:{
         "movie":"电影",
         "play":"lol"
     }
 }

 console.log(liuyan)
```

## 4、接口的继承

一个接口除被作为类型使用外，还可以被其他的接口继承，继承一个为单继承，二个以上为多接口继承，代码如下：

```ts
 定义 women接口
interface women{
    name:string,
    age:number,
    cook:(m:string)=>string
}
 定义 father接口
interface father{
    work:()=>string
}

 定义 son接口继承women,father接口
interface son extends women,father{
    play:string,
    study:()=>string
}

let zhanhzhenda:son={
    name:"张郑达",
    age:20,
    cook:(m:string):string=>{
        return `我喜欢做${m}`
    },
    play:"lol",
    study:():string=>{
        return`我要毕业上班`
    },
    work:():string=>{
      return `我要敲代码`
    }
}

```

## 5、使用接口定义额外的属性

```ts
// 正常定义一个接口
interface SquareConfig {
    color?: string;
    width?: number;
}

// 如果 上面使用接口定义的类型的`color`和`width`属性，并且*还会*带有任意数量的其它属 

interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}

let squareOptions:SquareConfig = { colour: "red", width: 100,opacity:0.5 };
```

## 6、类型断言

 有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。 

 通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

类型断言只在编译阶段的类型检查时起作用，且编译后会被移除，如果断言后的类型不正确，那么运行阶段会产生错误。 

 如下写法:  ts 报错提示 name 和age 不是student 上的属性

```
let student = {}
// let student: {}

student.name = 'jack'
// 类型“{}”上不存在属性“name”。
student.age = 18
// 类型“{}”上不存在属性“age”。
```

改成如下写法: 

```ts
interface Person{
  name:string;
  age: number
}
// 断言 student 对象类型为 person,这样如下就不会报错
// 指的是在使用的某个变量的时候, 指定断言他是某一种类型
let student = {} as Person
// let student: Person

student.name = 'jack'
student.age = 18
```



## 7、使用type 定义数据类型

```ts
type sta = 'success'|'error' // 定义一个sta 数据类型
type num = number  // 定义num 为number 数据类型
type str = string  // 定义 str 为string 数据类型

type obj = {
  name: string,
  age: number
}

let aaa: obj = {
  name: '张三',
  age: 20,
}
console.log(aaa);

// 还可以如下写法: 表示包含name 和age 属性的对象
type obj = {
  name: string,
  age: number,
  [propName:string]:any
}

```



**面试题:** 使用接口interface和type 定义数据类型的区别

01: 相当点: 都可以定义数据类型

02: 不同点: 

- type 能够**表示任意类型，** 而 `interface` 则**只能表示对象类型**。
- `interface`可以**继承其他的接口，** type 不支持继承。

- 重复interface 可以实现合并 或者由于继承实现interface 的合并 , 而type 不能重复声明



# 六、类的概念

在es5中使用函数创建类

```ts
 function Girl(name,age){
     this.name = name
     this.age = age
     this.say=function(){

     }
 }

 let fanbingbing = new Girl('范冰冰',40)
 let liuyifei = new Girl('刘亦菲',32)
 console.log(fanbingbing)
 console.log(liuyifei)
```

在es6中使用class 创建类

**注意**：类的构造函数：constructor(){}  ,一般在实例化类的时候，传参需要用constructor 构造函数接收参数的情形。所以这种情况下，才需要在类里写构造函数。否则不写。

指在构造（实例化）一个类时，执行的函数，故称为构造函数，它也是一个普通的函数，也能定义形参，在构造时传递实参, 该构造函数在实例化类的时候执行。如下：即在 new C（'张三'）的执行。

```ts
 class Girl{
     name:string;
     age:number;
     say():string{
       return `大家好，我是${this.name}`
     }
     constructor(m:string,n:number){ 构造函数
         this.name = m
         this.age = n
     }
 }

 let guanzhilin = new Girl('关之琳',40)
```



## 1、类的继承

- 在es6中，使用关键字  extends, super 实现类的继承。

- 允许使用继承来扩展现有的类，可通过 extends 关键字来完成，派生类通常被称作子类，基类通常被称作父类

- super() 表示调用父类的constructor(){} 方法

- 如果父类和子类的有相同的方法名，调用子类的方法时，则会执行子类的方法。（**也就是就近原则**）
  

```ts
// 定义 Girl类
class Girl{
     name:string;
     age:number;
     say():string{
       return `大家好，我是${this.name}`
     }
     constructor(m:string,n:number){ 构造函数
         this.name = m
         this.age = n
     }
 }
// 定义学生类 继承 Girl类
 class Student extends Girl{
     grade:string;
     study():string{
       return `我要学习web 前端`
     }
     say():string{
         return `子类中方法，我是${this.name}`
       }
     constructor(m:string,n:number,g:string){
         super(m,n)
         this.grade = g
     }
 }

let 大嫂= new Girl('大嫂',40)
```



## 2、静态属性

存在于类本身上面而不是类的实例上，即无须实例化就可以访问的属性，称之为静态属性

1、先看es5中类的静态属性表示如下：

![image-20211031192840452](https://i.loli.net/2021/10/31/QgkUCtduocH9Izw.png)

2、es6中类的静态属性

```ts
class Boy {
    static name1: string = '周杰伦';
    age: number;
    static say(): void {
        console.log(this.name1)
    }
    constructor(age: number) {
        this.age = age
    }

}
Boy.sex = '男'

let jielun = new Boy(45)
// jielun.say()
console.log(Boy.say())
```

## 3、类类型接口

定义一个接口，声明一个类去实现（implements）接口，注意不是继承 如下：

```ts
interface Person {
    name: string,
    age: number,
    say: (m: string) => void
}
// 使用类实现这个接口
class Man implements Person {
    name: string ;
    age: number;
    say(m: string): void {
        console.log(this.name)
    };
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}

let wangyucao = new Man('小明', 10);
```

## 4、抽象类

抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，  抽象类可以包含成员的实现细节。 abstract关键字是用于定义抽象类和在抽象类  内部定义抽象方法。

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 abstract关键字。

```ts
// 1. 特点 抽象类不能实例化
// 2. 抽象方法 不能写具体实现，必须在子类中写具体实现
abstract class Animal {
    name: string;
    age: number;
    constructor(m: string, n: number) {
        this.name = m
        this.age = n
    }
    abstract run(): string
}

class Cat extends Animal {
    constructor(m: string, n: number) {
        super(m, n)
    }
    run(): string {
        return `${this.name}在跑步`
    }
}
let huahua = new Cat('花花', 5)
console.log(huahua)
```

# 七、泛 型

**定义**：先定义一个变量代表传入的类型，再使用该变量，因此，它是一种特殊的变量，只代表传入的数据类型，不是一个固定的值，而是传什么是什么，而这种类型变量称之为泛型

说白了 就是ts 中的一个类型变量

```ts
// 普通函数1
const fn = (x:number)=>{
    console.log(x)
}
// 普通函数2
const fn2 = (x:string)=>{
    console.log(x)
}
// 普通函数调用
fn(1)
fn2('123')

//使用泛型定义简化上述方式
function fn<T>(x: T): T {
    return x
}

```

## 1、泛型在接口中的使用

```ts
interface fn1 <T>{
   (m: T): T
}

let f2: fn1<any> = function (m: any): any {
    return ``
}

let f1: fn1 = (m) => { return m }
```

## 2、泛型在类中的使用

```ts
class obj1<T>{
    arr: T[] = [];
    push(m: T) {
        this.arr.push(m)
    }
}

let obj = new obj1()
obj.push('及时雨宋江')
obj.push('黑旋风李逵')
obj.push('智多星吴用')
obj.push('豹子头林冲')
obj.push(100)
obj.push(true)
```

## 3、泛型约束

```ts
type a = string | number | boolean  // 泛型加约束条件
class obj1<T extends a>{
    arr: T[] = [];
    push(m: T) {
        this.arr.push(m)
    }
}

let obj = new obj1()
obj.push('及时雨宋江')
obj.push('黑旋风李逵')
obj.push('智多星吴用')
obj.push('豹子头林冲')
obj.push(100)
obj.push(true)
```

## 4、泛型数组方式

```ts
interface Array<T>{
    [name:number]:T
}
let arr3: Array<object> = [
    { name: "朱德", age: 60 },
    { name: "彭德怀", age: 60 },
    { name: "林彪", age: 66 }
]

```

