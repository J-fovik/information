进制

```
0 1  二进制   机器能够懂的语言
0 1 2 3 4 5 6 7 8 9   十进制  阿拉伯数字
0 1 2 3 4 5 6 7   八进制      二进制太长 十进制  计算容易出错  于是有了八进制
0 1 2 3 4 5 6 7 8 9  a b c d e f   十六进制
red    rgba   abcdef  
10满足二进制 八进制 十进制十六进制的要求
为了准确告诉计算机 这是八进制 需要加前缀
二进制   前缀  0b  或者 0B
八进制   前缀  0o   或者0O
十六进制  前缀  0x  或者0X
```

## JavaScript简介

1. **脚本**编程语言
2. 不能单独存在 必须依托与html
3. Javascipt 组成部分
   1. ecmascript 规定了 jsvascript 的语法
   2. DOM document object model **文档对象模型** 在html文件中 让助理js添加标签删除标签 修改标签内容
   3. BOM browser object  model 通过操作**浏览器**完成我们的需求
4. es5 es6 学习目标 (区别：语法升级)

## js三种应用方式

1. 行内

   1. a标签

   2. 非a标签

      ```html
      <!-- <a href="http://www.baidu.com">qfedu</a> -->
      
          <!-- alert('显示的内容'); 弹框  我们的第一行js代码-->
          <!-- 每行js代码一定要以分号为结尾 -->
      
          <a href="javascript:alert('hello world')">点我试试</a>
      
          <!-- 监控用户的一个动作 点击  只要点击 执行弹框  -->
          <!-- on+动作 -->
          <!-- click-->  
          <!-- onclick -->
          <div onclick="alert('hello world 9999')">试试就试试</div>
          <!-- 只要用户点击  就执行 alert('')代码 -->
      
      
          <a href="javascript:alert('范冰冰')" onclick="alert('李冰冰')">点我试试</a>
          <!-- 当href 和 onclick 同时存在  以on+动作 为准  -->
      ```

2. 内联

   ```html
       <script>
           alert('123');
       </script>
   </head>
   <body>
       <script>
           //代码是自上而下依次运行 
           // 一个html中 可以有多对script 标签  
           // 可以写在 head 中 也可以写在body 中 
           // 行内 必须用户操作才能执行js代码 
           // 打开页面自动执行 
   
           alert('456');
   
           // 建议写在body内部的最后面  (学习dom详细解释)
       </script>
   </body>
   ```
   
3. 外链

   ```html
       <script>
           alert('123');
       </script>
   </head>
   <body>
       <script>
           //代码是自上而下依次运行 
           // 一个html中 可以有多对script 标签  
           // 可以写在 head 中 也可以写在body 中 
           // 行内 必须用户操作才能执行js代码 
           // 打开页面自动执行 
   
           alert('456');
   
           // 建议写在body内部的最后面  (学习dom详细解释)
       </script>
   </body>
   ```
   
   ## 注意事项
   
   1. alert('123');   代码一定要以分号结尾   遇到分号表示一行代码结束
   2. 工作以后代码是要压缩的 ,压缩:省略空白换行 注释
   3. 如果不写分号 不运行报错
   
   ##  JS注释

   1. //      单行注释
2. /*    */      多行注释
   
   ## 输入输出
   
1. 输入
   
      1. prompt();
2. 输出
      1. alert();
      2. document.write();  显示在弹框
   3. console.log();   显示在控制台
   
> alert() 和doocument.write();  仅仅是输入内容
   >
   > console.log();  能够显示更多 更详细的信息

   ## 快捷键

   1. windows 系统查看 ip地址 window+r  -->cmd  -->  ipconfig  -->     
2. 查看能否上网  window+r   => cms  => ping www/baidu.com
   3. cal   计算器
   4. note 记事本
   5. tab  自动补全       光标最后 ctrl+c

   ## 变量（重点）

   ### 什么是变量
   
   * 语法  var 变量名 = 值;
   * 白话 ：变量就是一个装东西的盒子
   

​        变量是用于存放数据的容器，通过变量名获取数据 甚至可以修改数据

#### 注意：

1. 一个变量名只能存储一个值
   2. 当再次给一个变量赋值的时候，前面一次的值就没有了
   3. 变量名称区分大小写（`JS` 严格区分大小写）

   ###   变量的使用

   * 变量的声明
* 变量的赋值

## 变量的命名规则和规范

- 规则： 必须遵守的，不遵守就是错

  > 1. 一个变量名称可以由 **数字**、**字母**(a-zA-Z)、**英文下划线（_）**、**美元符号（$）** 组成,如：userrAge, num01, _name
  > 2. 严格区分大小写  var qf; 和 var Qf;
  > 3. 不能由数字开头  18age   是错误的
  > 4. 不能是 **保留字** 或者 **关键字**  编辑器中高亮的部分
  > 5. 不要出现空格 

- 规范： 建议遵守的（开发者默认），不遵守不会报错

  > 1. 变量名尽量有意义（语义化） nl   →     age  
  >
  > 2. 遵循驼峰命名规则，由多个单词组成的时候
  >
  >    大驼峰 UserName 小驼峰  userNameKangbazi
  >
  > 3. 不要使用中文

## 数据类型（重点）

>  数据类型就是数据的类别型号
>
> 变量没有数据类型，里面的数值才有数据类型

### 数据类型分类

#### 基本数据类型

1. 数值类型         (Number) 
   * 数字就是数值类型
     + 整数 
     + 浮点数 
     + 科学计数法  9e12 = 9乘以10的12次方
     + 二进制   八进制  十六进制     
     + NAN（非数字）    
     +  Infinity   （无穷大）
     + -Infinity  （负无穷大)

2. 字符串类型 (**String**)   

   + 用引号包裹起来   
     + 双引号“    ”       
     + 单引号‘  ’          
     + **反单引号``是es6 新增 （可以解析  变量）  ${变量}**

   * 字符串转义字符

   * 字符串拼接

     +  把两个字符串拼接一起

     + +字符串连接符   表示两个数字相加

       +左右只要有一个是字符串 就是字符串拼接

       +两边都是数字就是相加

   * 字符串长度

     + var strs = '       ';
     + var result = strs.length;
     + console.log(result);

| 转义符 | 解释说明                          |
| ------ | --------------------------------- |
| \n     | 换行符，n   是   newline   的意思 |
| \ \    | 斜杠   \                          |
| '      | '   单引号                        |
| "      | 双引号                            |
| \t     | tab  缩进                         |
| \b     | 空格 b 是blank 的意思             |

3. 布尔类型 (Boolean)    

+ true   false   全小写
  + true 在计算机按 1 存储      
  + false  按 0 存储

4. Null类型  (obgect   没有对象的意思)

+ 一个声明变量给 null 值，里面存的值为**空**

5. undefined类型  

- 一个声明里什么都没有，显示undefined     **没有值的意思**

#### 复杂数据类型

## 查看数据类型

* 使用tupeof关键字来判断

* 使用   string

  tupeof 数值     tupeof  变量名

  tupeof（ 数值）     tupeof  变量名
  
  null  'object'

### 数据类型转换

> 把一种数据类型的变量转换成我们需要的数据类型

* 数字转换成字符串
* 字符串转换成布尔
* 布尔转换成数字

### 其他数据类型转换成数值

**把一种数据类型的变量转换成我们需要的数据类型**

| 方式       | 说明             |
| ---------- | ---------------- |
| parseInt   | 转换成整数数值型 |
| parseFloat | 浮点数数值型     |
| Number     | 数值型           |
| + - / *    | 隐式转换         |

1. Number(变量或者数值)

   > **整体看待**
   >
   > 把一个变量强制转换成数值
   >
   > 规则转换    把变量中的内容当一个整体看，
   >
   > ​                    如果不是合法数字 就返回NaN
   >
   > **支持整数，支持浮点数  识别小数点**
   >
   > ‘true'   'false'   单词      =>  NaN
   >
   >  true --1     fales--0    

1. parseInt(变量)

   > **整数结果**
   >
   > 只能转整数
   >
   > 转换规则
   >
   > ​              不管转什么都是一位一位转换
   >
   > ​              如果第一位不是合法数字  直降输出NaN
   >
   > ​              如果第一位是合法数字 那么保留并输出 以此类推直道不能转换为止
   >
   > 注意事项
   >
   > ​                **不识别小数点**
   >
   > ​               不能转true false      ==> NaN
   >
   > ’100a'  =>100
   >
   > '200.56' => 200aa
   >
   > _true => NaN   false => NaN_

2. parseFloat(变量)

   > **小数**
   >
   > 转浮点数（小数）
   >
   > 只保留第一个小数点
   >
   > 转换规则
   >
   > ​              不管转什么都是一位一位转换
   >
   > ​              如果第一位不是合法数字  直降输出NaN
   >
   > ​              如果第一位是合法数字 那么保留并输出 以此类推直道不能转换为止
   >
   > 注意事项
   >
   > ​               **识别小数点**
   >
   > ​               不能转true false

3. 除了加减法以外的属性运算

   > +（也可以转换成数字型） 
   >
   > 除了+ 以外 - * / 等都会把数据转换成数字类型

### 其他数据类型转换成字符串

1. 变量.toString

   > **转成字符串**
   >
   > 转成字符串类型以后的数据
   >
   > 不能转换 undefined 和null
   >
   > 变量中必须是数值 字符串布尔 不能是undefined 和null

2. String(变量)

   > **转成字符串**
   >
   > 返回结果是 转成字符串类型以后的数据
   >
   > 任何类型都可以

3. 使用加减法

   > 和字符串拼接的结果都是字符串

### 其他数据类型转换成布尔

1. Boolean(变量)

   > **其他类型转成布尔值**
   >
   > 数值类型0、NaN 、0.0
   >
   > 空字符串 ''
   >
   > 布尔 false
   >
   > null
   >
   > undefined  
   >
   > 返回结果false
   >
   > 
   >
   > **有 Boolean  显示转换 也叫强制转换**
   >
   > **没有 Boolean   隐式转换**

   ##### 其余全是 true

## 运算符

### 数学运算符

1. +
2. -
3. *
4. /
5. %
   * 取余数
6. ** 幂运算 
   * parseInt( )     要商不要余数3

### 表达式和返回值

1. 字面量
   
   * 直接显示的数据数据字面量
   
   * 数字字面量   
     * 10  数字字面量
     * ’'abc' 字符串字面量   '  '空字符串
     * true 布尔字面量
     * 0.0
   
2. 表达式

   * 字面量+运算符   的一个组合

3. 返回值

   * 结果

### 浮点数的精度问题

0.1 + 0.2 = 0.3004 

0.6 * 100 = 60

0.07 * 100 = 7.01

> 浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。 

_变量  .toFixed   保留小数_

_(num1 * 10 + num3 * 10) / 10_

### 赋值运算符

1. =                 直接赋值
2. +=      -=       
3. *=   /=     %=   **=

### 比较运算符

###### 比较运算符一定是布尔类型 逻辑运算符不一定是布尔类型

任意数据一定有两部分 值 类型
NaN  跟任意数据都不相等       NaN  跟  NaN不相等   跟任意数据运算都是NAN

1. = 

   > 赋值

2. ==

   > 值相等
   >
   > 相等就是true

3. ===

   > 值和类型都相等
   >
   > 有一个不相等就是false

4. !=

   > 值不相等   类型不一样  为false
   >
   > **判断值是不是相等**
   >
   > 如果值不相等为true

5. !==

   > 值和类型有一个相等   为true
   >
   > 是不是不全等 如果不完全相等为true

6.  `>=`

7. `<=`

8. `>`

9. `<`

###  逻辑运算苏

1. &&

   * 逻辑与   并且

     > 一个为false  结果为false
   
2. ||

   * 逻辑或   或者

     > 一个满足true  结果为true
     >
   
3. !   

   *  逻辑非       **取反**

     > !true    结果为  false
     >
     > !false    结果为  true

4. 逻辑非>逻辑与>逻辑或

### 短路运算（逻辑中断）

* 逻辑与

  > 两边都是true  结果就是true
  >
  > 有一个是flase  结果为 false
  >
  > 遇到假 停止向右  

* 逻辑或

  > 有一个是true  结果就是true
  >
  > 两个都是flase  结果为false
  >
  > 遇到真 停止向右  

### 自增自减运算符

1. ++
   * 每次+ 1
   * 前置 先改变在运算
   * 后置 先运算在改变
2. --
   * 每次 - 1

## 分支结构

* 流程控制
  1. 顺序结构
  2. 分支结构
  3. 循环结构

### if 分支结构（重点）

#### if 单分支语句

```js
if(条件){
     alert('执行语句')  //要执行的代码
}
```

* 条件只有 true   和  false
* 除了0 剩下的语句都为真
* 除了' ' 剩下的字符串都为真
* 不能出现一个等号
* 代码只有一行可以省略大括号
* 只能执行一次

####  if    else  双分支语句

```js
if(条件){
     alert('执行语句')   //满足条件执行代码
} else {
     alert('执行语句')   //不满足条件执行代码
}
```

* 只能有一行代码被执行

#### if    else  if  多分支语句

```js
if(条件){
     alert('执行语句')   //满足条件执行代码
} else  if (条件二){
     alert('执行语句')   
} else  if (条件三){
     alert('执行语句')   
} else{
    alert('执行语句')
}
```

* 依次判断  满足条件则不望下执行
* 只能执行一行

### SWITCH 条件分支结构（重点）

* switch case 语句一般用于等值判断 不适合区间判断
* 需要配合break关键字使用，没有break会造成case穿透
* 如果条件是一个确定的值 那么使用swich
* 如果不是一个范围 那么不能使用swich 只能使用 if else if
*  类型要一样

```js
  switch (数据){
    case 值1:
        代码1
        braek   //推出
    case 值2:
        代码2
        braek
     default:   //最后
        代码n
        braek
}
```

### 三元运算符

* 符号 ？与 ：配合使用
* 一般用来取值

```js
条件 ？ 代码1 : 代码2   //真代码1   假代码2
3 > 5 ? alert('真') : alert ('假')
var num = 3 > 5  ? 3 : 5   //结果5
```

#  循环-重中之重

whlie ：在.....期间    在满足条件期间，重复执行代码

### whlie 语法

```js
whlie(循环条件){
     要重复执行的代码（循环体）
}
```

 循环条件为true 才会进入**循环体**执行代码

* 循环要素
  1. 变量起始值
  2. 终止条件 （没有终止 会死循环）
  3. 变量变化量 （用自增或自减）

### do ...whlie  循环

```js
do {
   执行的代码
} while(条件)
```



### for 循环

```js
for (变量；表达式；操作表达式){
    要循环的代码   循环体语句
}
for (var i = 1 ; i >= 10; i++) 
```

* 初始化语句只能执行一次
* 判断为true 循环继续
* 判断为false  循环停止
* 求和的变量不能写在循环里
* 把变量放在循环里 当前变量只能在本次循环有效 

#### while  for 

* 相同点：运行规则一样
* 不同
  * for  控制循环变量 ，归属for循环语法结构，在for循环结束以后 不在被访问
  * whilr 控制循环变量 ，不归属语法结构，循环结束，改变量还可以继续使用



### BREAK 终止循环

* 关键字 break   退出循环

> 一般用于结果已经得到 后续循环不需要的时候用

```js
whlie(循环条件){
if(i === 5){
dreak
}
     要重复执行的代码（循环体）
     i++
}
```



### CONTINUE 结束本次循环

* 关键字 continue    结束本次循环，进入下一个循环
* 结束本次循环 
* i++ 要写在continue上面   不然会卡死

> 一般用于排除或者跳过某一个循环

```js
whlie(循环条件){
if(i === 5){
i++
continue
}
     要重复执行的代码（循环体）
     i++
}
```

## 函数

> 函数又是一个数据类型
>
> alert （typeof  test ）   函数名+小括号表示执行 

> 函数叫工具 叫方法

* 函数就是把任意代码放**盒子**里
* 两个阶段就是 **放在盒子里面** 和 **让盒子里面的代码执行**
* 如果这个代码在多个地方修改，可以把重复代码放到函数中
* 多个地方只需要调用函数
* 提升代码复用性
* 节约代码量
* 如果修改这个函数，只能用函数的自动改

### 函数的阶段

1. 定义阶段
   1. 声明函数
   2. 赋值函数
2. 调用阶段

### 函数定义阶段

#### 声明式

* 标识符 变量名字 函数名字

* 使用 `function` 关键字

  ```js
  function 函数名(){
  
  }
  ```

* function 必须写 表是这是一个函数
* 函数名 必须遵守标识符的命名规范和规则
* （）必须写
*   { } 就是盒子 必须写

#### 赋值式

```js
var num = function(){ //匿名函数
}
num(); 
```

* 小括号代表执行

### 函数调用阶段

#### 调用一个函数

```js
   for (var i = 1; i <= 10; i++) {
           函数名()
        }

函数名（）
```

### 声明和调用区别

* 声明函数函数声明前后调用都可以
* 必须在函数声明以后调用

### 函数的参数

参数分两种

1. 行参
   * 函数定义阶段
   * 行参是写在函数定义阶段的小括号里 多个之间逗号隔开
   * 行参临时变量 遵循标识符命名规范规则
   * 行参里的值由实参决定
2. 实参
   * 调用时候
   * 从左到右依次给到行参
   * 函数调用阶段 现在调用阶段小括号内 
   * 是一个字面量 一个真实的值 按照从左到右的顺序依次给到行参

#### 函数调用阶段 

1. 先找到toys1这个工具
2. 实参传递给行参
3. 函数内的代码执行

```js
function toys1(num){//函数的参数  行参 

}
num(10)//实际参数
```

## 函数的return

* alert() document.write() console.log() 他们仅仅是将将结果输出到页面上，弹框控制台，我们看不到结果
* alert（） 仅弹框显示
* console.log()   仅控制台显示
* document.write()  undefined 不给结果

```js
//例子 
function add_nums(num1,num2){
return num1 +num2 
}
var res = add_nums (10,20);
console.log(res)；
```

```js
function test1(){
ocnsole.log('tese1')
}
function test2(){
ocnsole.log('tese2')
tese1()
}
tese2()
```

>  函数中如果有while 或者for

>  break 和 return 效果一样 都是中断代码

> 一般用return 代替break

总结return

1. 返回值 把结果给你
2. 中断函数 一旦遇到return 函数结束
3. return  后面的代码不执行

优点 

1. 提升代码的复用性
2. 如果修改 只需要修改函数就可以 其他就可以自动跟着修改

### 参数的默认值



#### 形参和实参个数不一样

1. 形参和实参相等
   * 从左到右 实参挨个传给形参

2. 形参个数大于实参个
   * 没有就是undefined

3. 实参个数大于形参个数
   * 超过形参个数部分被忽略

系统提供了 一个函数，保存所有形参arguments

#### arguments

> 用来接收所有的实参
>
> 就想一个大箱子
>
> 实参会被编号 然后放到箱子里
>
> 编号从0开始
>
> **arguments.length** 查看箱子中有多少个数值  

####  函数预解析

> 执行代码以前对代码进行通读并解释
>
> 老板的活 秘书先处理 然后老板在去做

> 预解析只解析两部分内容
>
> 1. var 声明变量 赋值式函数按照 这个规则进行解析 （赋值式函数也有var 关键字）
> 2. 声明式函数

##### 对var解析

var 声明变量 提前进行声明但是不解析

##### 注意

用浏览器打开一个html代码 预解析全局代码

不会对函数内的代码进行预解析

只有函数调用了里面的函数才会预解析

##### 对声明式函数

对函数名进行提前声明 然后往箱子里放一个函数

#### 函数在定义阶段和调用阶段分别都干了什么

> 面试题

### 内存调用

1. 栈内存
   * 基本数据类型
   * 复杂数据类型的名字
2. 堆内存
   * 复杂数据类型的数据体

### 函数在定义阶段做的事情

1. 在栈内存开辟一个空间放函数名

2. 先在堆内存中开辟一个空间

3. 把函数体内的代码 一模一样的放在堆内存的空间（不预解析）

4. 把空间的内存地址给到存放函数名字的栈内存空间 

### 函数在调用阶段做的事情

1. 根据函数的名字找到栈内存，
   * 栈内存是否有这个名字
   * 有这个名字 那么查看里面是否有这个内存地址，如果有函数正常执行
2. 会在调用栈内开辟一个新的函数执行空间
3. 在执行空间内给形参赋值
4. 、在执行空间内对函数进行预解析
5. 在实现空间内对函数体内的代码执行一遍
6. 执行完毕这个执行空间被销毁

#### 作用域

>  就是形式权力的范围
>
> 中国的某个省  印度的某个邦 只能在各自的省和邦行驶权力 超过以后不能行驶权力
>
> 标识符： 变量名 函数名 参数名
>
> 总结： 作用域就是函数名 变量名 生效使用的范围

大括号

1. if(){}
2. whlie(){}
3. for(){}
4. function(){}

上面很多大括号 只有函数的大括号才叫作用域

#### 作用域的分类

1. 全局作用域  公家的
   * 一个html 页面打开，就是一个全局作用域
   * 这个全局作用域叫 window 
2. 私有作用域  私人的
   * 只有函数次有私有作用域，别的没有

#### 作用域的上下级关系

>  定义在那个作用域内的函数，就是那个作用域的子集作用域
>
>  定义在那个作用域下的变量 结束那个作用域的私有变量
>
>  私有变量只能自己用 或者给自己的后代用 不能给父级用
>
>  不同作用域   变量名字相同 各自独立
>
>  相同作用域 函数名字相同 后面会把前面替换掉
>
>  函数参数只能在函数内生效出了函数不生效

```js
//定义在全局作用域下的ti
//ti的父级是全局作用域 就是window
function t1(){
//ti通称为ti的私有作用域

//t2 是定义在他内的私有作用域
//t2 的父级是 t1
function t2(){
//这里是t2的私有作用域

}
}
```

#### 标识符的行为

1. 标识符的定义
   * var  变量名
   * function 函数名(){}
   * 函数的形参
2. 访问标识符
   * 函数内访问形参
   * 函数内调用函数
3. 变量的赋值
   * 形参和实参的

#### 标识符访问

1. 当我们访问作用内的变量的时候 现在自己的作用域内查找

   自己作用域有 直接用 停止查找

2. 没有 就去父级作用域查找 父级有直接用 停止查找

3. 父级作用域没有就继续往上 有直接用 没有继续查找

4. 以此类推 知道window  如果window没有直接报错 ** is not defined

#### 提示

创建变量的时候一定要加上var  因为不加 var 会当做全局变量

局部变量 在函数运行完了以后会在栈内存释放空间

全局变量必须等到整个html页面关闭 才会释放

## 递归

在调用函数的时候把问题逐层解决

一个函数调用了自身，并设置了结束条件，这个函数才是一个正确的递归

> 自己调用自己

书写递归函数技巧

1. 先写一个函数
2. 一定要写好结束条件 （写不好就是死循环）
3. 如果没有达到结束条件 一定要 用return返回

1. 先写函数
2. 写结束条件
3. 没有达到结束条件 return

> 递归函数其实就是一个循环 一定要写好结束条件
>
>  递归函数只不过是给我们多了一个解决方案
>
> 递归函数调用自己已有次数限制
>
> 如果次数大于100 可能会报错

```js
//斐波那契数列
function fn(n){
     if(n === 1 || n === 2) return 1;
     return fn(n - 1) + fn(n - 2);
}
document.write(fn(6));
```



## 对象

> 对象（object）javascript里的一种复杂数据类型
>
> 名字在栈内存    数据体在 堆内存
>
> 对象也是一个放数据的盒子
>
> 成对出现  键值对
>
> 为一种无序的数据集合

内置改造函数

#### 特点

1. 无序的数据集合
2. 可以详细的描述某个事物

#### 对象的使用

1. 对象声明语法（字面量方式） 
   * var  对象名 = {  }
   * var 对象名 = new  Object（）
2. 对象由属性和方法组成
   * 属性   消息和特征
   * 方法   功能和行为

### 键的要求

1. 方法是由方法名和函数两部分构成，用  ：  分隔
2. 多个属性之间用 ， 号分隔
3. 方法是依附在对象中的函数
4. 键如果是字符串 可加单引号可不加
5. 可以添加形参和实参
6. 如果对象的键是数字 会在最前面
7. 支持特殊符号   比如  # ! % &等必须加单引号包裹
8. 虽然支持数字特殊符号建议写的时候最好是字符串

```js
var 对象名 = {
属性名 ： 属性值 ,
方法名 ： 函数 
}
console.log(对象名)
console.log(typpeof 对象名) //检测 object
// 对象里面叫属性，外面叫变量。
```

### 对象的使用

#### 增   删    改    查

1. 查  （read） 
   * 对象名 . 键名       
   * 访问某个成员 键对应的值
2. 改(update)  
   *  对象名 . 键名 = 新值    
   * 修改对象的某一个成员（键值对）
3. 增（created）   
   * 对象名 . 键名 = 新值  
   *  往对象中添加一个键值对
4. 删 (deleted)    
   * delete 对象名 . 键名  
   * 删除对象的一个成员（键值对）

#### 数组关联语法

1. 查  （read） 
   * 对象名 . [ ' 键名 ' ] 
2. 改(update)  
   *  对象名 . [ ' 键名 ' ]  = 新值 
3. 增（created）   
   * 对象名 . [ ' 键名 ' ]   = 新值  
4. 删 (deleted)    
   * delete 对象名 . [ ' 键名 ' ]  

如果你对象的键是数字或者特殊符号 只能用数组关联语法

如果键放到了一个变量中只能用数组关联语法 不支持给变量加引号

```js
var box = {								
	run : function () {						//对象中的方法
		return '运行';
	}
}
box.run()						//调用对象中的方法
```

### 遍历对象

> for   in   
>
> 根据对象中有多少个键值对 然后循环多少次
>
> 每次循环 把变量的键放到变量中
>
> 拿到键 想要拿到键对应的值

```js
var box = {							//创建一个对象
	'name' : 'qfedu',					//键值对，左边是属性名，右边是值
	'age' : 28,
	'height' : 178
};
for (var 变量 in 对象名) {						//列举出对象的所有属性
	aler(p);
}
```

查看对象中是否有某个键

* in      键 in 对象名
* 只能判断键支付在对象中 不能判断值

## 数组

#### 术语

1. 元素：数组中保存的每个数组的元素
2. 下标：数组中数据的编号
3. 长度：数组中数据的个数，通过数组的length属性获得

#### 认识数组

* 复杂数据类型

* 是个盒子

* 有序的数据集合
* 可以存放任意数据类型
* 一般存放相同数据类型
* 数组中的数值称为元素

> 基本数据类型
>
> ​    number  String  Boolean   Null   undefined   
>
> ​    null 是一个特殊的对象 
>
> 引用数据类型（复杂数据类型）
>
> 1. function
> 2. object
> 3. Array

#### 数组分类

* 字面量
  * 空数组    var 数组名 = [  ]
  * 非空数组   var 数组名 = [  数据1 ， 数据2 ....... 数据n]
* 内置改造函数
  * 空数组    var  数组名 = new  Array  (  )    
  * var 数组名 = new  Array (  数字)    只放一个数字
  * new  Array (8 )  创建长度为8 的数组
  * var  数组名 =  new Array ( 数据1 ，数据2 ，......)   创建多个数组
  * 数据每个位置用  empty 填充

#### 数组中元素的排列

* 数组是有序的集合
* 每个元素有自己的序号   从0开始  序号叫索引（下标）

#### 数组的基本操作

1. length

   * 读（查）
     * 数组 . length
   * 写（增删改）
     * 数组 . length = 数字
     * 等于  什么也没操作
     * 大于   大于的部分用  empty  补齐
     * 小于   从数组末尾删除小于的部分

2. 索引

   * 读

     * 语法   数组[索引]
     * 如果索引存在 输出索引存在的值，不存在返回 undefined

   * 写

     * 数组[索引] = 新值 
       * 给一个存在的索引赋值，索引存在的值会呗替换成新值
       * 如果所有超过数组长度，这个索引会是新值，其他用empty补齐

     * 数组[数组名.length] = 新值
       * 如果给数组的长度索引赋值，相当于在数组末尾添加一个元素
       * lenght - 1 最后一个元素

3. 数组的遍历**超级重点**

   >  把数组的元素挨个取出来
   >
   >  用循环把数组里的每个元素访问一遍

   1. 构建元素的索引 循环 初始值 范围 数组 . length

   ```js
   2.每次循环把下标放到变量中
   for(var 变量  in  数组名){
     console.log(变量)  // 输出下标
     console.log(数组名[变量]) // 输出值
   
   }
   ```

   ```js
   3.每次循环把元素放到变量中
   for(var 变量  of  数组名){
      console.log(变量)//输出值
   } 新增+
   ```
   



### 数组常用方法之 push

作用：用来在数组的末尾追加元素

语法： 数组.push(数据)

返回值：添加数据以后, 数组最新的 长度

```text
var arr = [1, 2, 3]

// 使用 push 方法追加一个元素在末尾
var res = arr.push(4)

//返回数组的长度
console.log(res); // 4

console.log(arr) // [1, 2, 3, 4]
```

### 数组常用方法之 pop

作用：pop 是用来删除数组末尾的一个元素

语法：数组.pop()

返回值：被删除的数据

```text
var arr = [1, 2, 3]

// 使用 pop 方法删除末尾的一个元素
var res = arr.pop()

// 返回被删除的元素
console.log(res); 3
console.log(arr) // [1, 2]
```

### 数组常用方法之 unshift

作用：在数组的最前面添加元素

语法：数组.unshift(数据)

返回值：返回数组最新的长度

```text
var arr = [1, 2, 3]

// 使用 unshift 方法想数组的最前面添加一个元素
var res = arr.unshift(9)

//返回数组最新的长度
console.log(res); // 4
console.log(arr) // [9, 1, 2, 3]
```

### 数组常用方法之 shift

作用：删除数组最前面的一个元素

语法：数组.shift()

返回值：被删除的数据

```text
var arr = [4, 5, 6]

// 使用 shift 方法删除数组最前面的一个元素
var res = arr.shift()

//返回被删除的数据
console.log(res); // 4
console.log(arr) // [5, 6]
```

### 数组常用方法之 splice

作用: splice 是截取数组中的某些内容，按照数组的索引来截取

语法: splice(从哪一个索引位置开始，截取多少个，替换的新元素)

第三个参数可以不写

返回值：返回一个新数组，数组内的值就是刚刚删除掉的值

注意：该方法会改变原始数组

```text
var arr = [1, 2, 3, 4, 5]

// 使用 splice 方法截取数组
var res = arr.splice(1, 2)

//返回一个新数组 元素就是截取出来的数据
console.log(res); //[2, 3]
console.log(arr) // [1, 4, 5]
```

arr.splice(1, 2) 表示从索引 1 开始截取 2 个内容。第三个参数没有写，就是没有新内容替换掉截取位置。

```text
var arr = [1, 2, 3, 4, 5]

// 使用 splice 方法截取数组
var res = arr.splice(1, 2, '我是新内容')

//返回的是一个新数组，里面的元素就是截取出来的元素
console.log(res); //[2, 3]
console.log(arr) // [1, '我是新内容', 4, 5]
```

arr.splice(1, 2, '我是新内容') 表示从索引 1 开始截取 2 个内容，然后用第三个参数把截取完空出来的位置填充。

### 数组常用方法之 reverse

作用：reverse 是用来反转数组使用的

语法：数组.reverse()

返回值：反转后的数组

```text
var arr = [1, 2, 3]

// 使用 reverse 方法来反转数组
var res = arr.reverse()
console.log(res); //[3, 2, 1]

console.log(arr) // [3, 2, 1]
```

### 数组常用方法之 sort

作用：sort 是用来给数组排序的

简单用法语法：数组.sort()

基础用法

```text
var arr = [2, 3, 1, 4, 5, 18, 7, 32]

// 使用 sort 方法给数组排序
var res = arr.sort()

console.log(res);

console.log(arr) // [1, 18, 2, 3, 32, 4, 5, 7]
```

升序和降序用法

○语法->升序：数组.sort(function (a, b) { return a - b })

○语法->降序：数组.sort(function (a, b) { return b - a })

返回值：排序后的数组

```text
var arr = [ 1, 23, 32, 20, 19, 10, 3, 5, 33 ]
console.log('原始数组 : ', arr)

// 升序排列
var res = arr.sort(function (a, b) { return a - b })

console.log('排序之后 : ', arr)
console.log('返回值 : ', res)


var arr = [ 1, 23, 32, 20, 19, 10, 3, 5, 33 ]
console.log('原始数组 : ', arr)

// 降序排列
var res = arr.sort(function (a, b) { return b - a })

console.log('排序之后 : ', arr)
console.log('返回值 : ', res)
```

### 数组常用方法之 concat

作用：concat 是把多个数组进行拼接

语法：数组.concat(数组)

返回值：返回一个新数组

和之前的方法有一些不一样的地方，就是 concat 不会改变原始数组，而是返回一个新的数组

```text
var arr = [1, 2, 3]

// 使用 concat 方法拼接数组
var newArr = arr.concat([4, 5, 6])

console.log(arr) // [1, 2, 3]
console.log(newArr) // [1, 2, 3, 4, 5, 6]
```

注意： concat 方法不会改变原始数组

### 数组常用方法之 join

作用: 数组里面的每一项内容链接起来，变成一个字符串

可以自己定义每一项之间链接的内容 join(要以什么内容链接)

不会改变原始数组，而是把链接好的字符串返回

```text
var arr = [1, 2, 3]

// 使用 join 链接数组
var str = arr.join('-')

console.log(arr) // [1, 2, 3]
console.log(str) // 1-2-3
```

注意： join 方法不会改变原始数组，而是返回链接好的字符串

### 数组常用方法之 slice

作用：slice 能够截取数组，并返回一个新的 数组不改变数组

语法：数组.slice(下标开始值,下标结束值)

注意：从下标开始值开始，保留到 下标结束值的前一个结束，如果没有下标结束值（也就是没有第二个参数），就保留到最后一个字符结束。

返回值：返回一个新的 数组

```text
var arr = [1, 2, 3, 8, 4, 9]

// 使用slice截取
var str = arr.slice(2, 4)

console.log(arr) // [1, 2, 3, 8, 4, 9]
console.log(str) //[3, 8]
```

### 数组常用方法之 indexOf

语法: 数组名.indexOf(要查找的数据)

语法二: 数组名.indexOf(要查的数据,索引)

- 这个语法的意思是.要从指定的索引开始查找该数据,
- 如果有就放回该数据在原数组中第一个出现的位置,如果没有就返回 -1

作用: 从前往后在数组中查找该数据第一次出现的位置

返回值: 如果该数组中有这个数据就返回这个数据第一个次出现的位置也就是索引,如果没有返回 -1

```text
// indexOf() 方法
// 语法一:
var arr = [200, 300, 100, 200, 300, 200, 300]
var res = arr.indexOf(100)
console.log('原始数组:', arr);
console.log('返回值:', res);


// 语法二:
var arr = [200, 300, 100, 200, 300, 200, 300]
var res = arr.indexOf(300, 1)
console.log('原始数组:', arr);
console.log('返回值:', res);
```

### 数组常用方法之 lastIndexOf

语法: 数组名.lastIndexOf(要查的数据)

语法二: 数组名.lastIndexOf(要查找的数据,索引)

- 这个语法的意思是.要从指定的索引开始查找该数据,
- 如果有就放回该数据在原数组中第一个出现的位置,如果没有就返回 -1

作用: 从后往前在数组中查找这个数据第一次出现的位置

返回值: 如果该数组中有这个数据就返回这个数据第一个次出现的位置也就是索引,如果没有返回 -1

```text
// lastIndexOf() 方法
// 语法一:
var arr = [200, 300, 100, 200, 300, 200, 300]
var res = arr.lastIndexOf(400)
console.log('原始数组:', arr);
console.log('返回值: ', res);


// 语法二:
var arr = [200, 300, 100, 200, 300, 200, 300]
var res = arr.lastIndexOf(200, 2)
console.log('原始数组:', arr);
console.log('返回值: ', res);
```

### 数组常用方法之 forEach

作用：和 for 循环一个作用，就是用来遍历数组的

语法：arr.forEach(function (item, index, arr) {})

- item 表示数组内的每一项
- index 表示数组内每一项的索引
- arr 表示原始数组

返回值：没有返回值，是undefined

```text
var arr = [100, 200, 300, 400, 500]

arr.forEach(function(item, index, arr) {
    // 这个函数会根据数组内有多少成员执行多少回
    console.log('我执行了')
    console.log(item);
    console.log(index);
    console.log(arr);
    console.log(item, ' ---- ', index, ' ---- ', arr)
})
```

- forEach() 的时候传递的那个函数，会根据数组的长度执行
- 数组的长度是多少，这个函数就会执行多少回

### 数组常用方法之 map

和 forEach 类似，只不过可以对数组中的每一项进行操作，返回一个新的数组

语法：arr.map(function (item, index, arr) {})

返回值：是一个新数组, 并且和原始数组长度一致

- 新数组内每一个数据都是根据原始数组中每一个数据映射出来的
- 映射条件以 return 的形式书写

```text
var arr = [1, 2, 3]

// 使用 map 遍历数组
var newArr = arr.map(function(item, index, arr) {
    // item 就是数组中的每一项
    // index 就是数组的索引
    // arr 就是原始数组
    return item + 10
})

console.log(newArr) // [11, 12, 13]
```

### 数组常用方法之 filter

作用：和 map 的使用方式类似，按照我们的条件来筛选数组

语法：arr.filter(function (item, index, arr) {})

返回值：把原始数组中满足条件的筛选出来，组成一个新的数组返回

```text
var arr = [1, 2, 3]

// 使用 filter 过滤数组
var newArr = arr.filter(function(item, index, arr) {
    // item 就是数组中的每一项
    // index 就是数组的索引
    // arr 就是原始数组
    return item > 1
})

console.log(newArr) // [2, 3]
```

我们设置的条件就是 > 1，返回的新数组就会是原始数组中所有 > 1 的项

### 数组常用方法之 every

作用：判断数组中是不是每一个数据都满足条件

语法：arr.every(function (item, index, arr) {})

返回值：一个布尔值

- 如果数组中每一个都满足条件, 那么返回值 true
- 只要数组中任何一个不满足条件, 那么返回 false

判断条件以 return 的形式书写

```text
var arr = [100, 200, 300, 400, 500]
console.log('原始数组 : ', arr)

var res = arr.every(function(item, index, srr) {
    console.log(item)
    console.log(index);
    // 以 return 的形式书写 判断 条件
    return item < 500

    // return index > 6
})

console.log('返回值 : ', res) //false
```

### 数组常用方法之 some

作用：判断数组中是不是有某一个满足条件

语法：arr.some(function (item, index, arr) {})

返回值：一个布尔值

- 如果数组中有任何一个满足条件, 那么返回 true
- 只有数组中所有的都不满足条件, 才会返回 false

```text
var arr = [100, 200, 300, 400, 500]
console.log(arr)

var res = arr.some(function(item, index, arr) {
    console.log(item);
    console.log(index);

    // 以 return 的形式书写 判断 条件
    return item < 50

    // return index > 6
})

console.log(res) //false
```

### 数组常用方法之 reduce

作用：进行叠加累计，函数根据数组中的成员进行重复调用

语法：arr.reduce(function (prev, item, index, arr) {}, 初始值)

- prev: 初始值 或 每一次叠加后的结果
- item: 每一项
- index: 索引
- arr: 原始数组

初始值: 默认是数组索引 0 位置数据, 表示从什么位置开始叠加

返回值：返回最终的结果

```text
var arr = [100, 200, 300, 400, 500]
console.log(arr)

var res = arr.reduce(function a(prev, item, index, arr) {
    console.log(prev);
    console.log(item);
    console.log(index);
    // 以 return 的形式书写每次的叠加条件
    return prev + item
}, 0)

console.log(res)
```

### 数组常用方法之 find

作用：查找数组中某一个数据

语法：arr.find(function (item, index, arr) {})

返回值：数组中你查找到的该数据。

查找条件以 return 的形式书写

```text
var arr = [100, 200, 301, 400, 500]
console.log(arr)

// 我想找到原始数组中的哪一个 奇数
var res = arr.find(function(item, index, arr) {
    console.log(item);
    console.log(index);

    // 以 return 的形式书写查找条件
    return item % 2 === 1
})

console.log(res)
```

### 数组常用方法之 findIndex

作用：查找数组中某一个数据

语法：arr.findIndex(function (item, index, arr) {})

返回值：数组中你查找到的该数据所在的索引位置。

查找条件以 return 的形式书写

```text
var arr = [100, 200, 301, 400, 500]
console.log(arr)

// 我想找到原始数组中的哪一个 奇数
var res = arr.find(function(item, index, arr) {
    console.log(item);
    console.log(index);

    // 以 return 的形式书写查找条件
    return item % 2 === 1
})

console.log(res)
```

### 数组的排序

1. 内置方法

   * 数组.sort()

   ```
   默认对字母排序 一位一位的
   如果排序是数字  需要传递一个函数
   ```

2. 冒泡

   1. 一轮不够
   2. 每一轮比较好多次
   3. 每一轮（两两比较）比较好多次
   4. 下一轮比上一轮少一次

   口诀

   * 双重for循环，一层少一次，里层减外层，变量向交换

   ```js
   var num = [11, 2, 58, 97, 63, 14, 26, 54, 89, 32, 15, 46, 87]
       for (var i = 0; i < num.length - 1; i++) {              //循环次数
           for (var j = 0; j < num.length - i - 1; j++) {      //排序次数
               if (num[j] < num[j + 1]) {                      //排序比较
                   var ble = num[j]
                   num[j] = num[j + 1]
                   num[j + 1] = ble
               }
           }
       }
       console.log(`最后一次循环${num}`);
   ```

   

3. 选择

   > 第一轮 找到所有数字的最大值 然后把最大值 跟下标0位置的数据进行交换
   >
   > 第二轮 下标0位置的不参与了 剩下的里边找到最大值 然后把最大值 跟下标1的位置数据进行交换
   >
   > 第三轮 下标1的位置不参与了 剩下的里边找到最大值 然后把最大值 跟下标的位置数据进行交换

   ```js
       var num = [11, 2, 58, 97, 63, 14, 26, 54, 89, 32, 15, 46, 87]
           for (var j = 1; j < num.length; j++) {
               var maxin = j - 1
               for (var i = j; i < num.length; i++) {
                   if (num[i] > num[maxin]) {
                       maxin = i
                   }
               }
               var temp = num[j - 1];
               num[j - 1] = num[maxin];
               num[maxin] = temp;
               console.log(num);
           }
           console.groupEnd('---------');
   ```

4. 快速选择  

   ```js
   var num2 = [6,8,2,5,1,3,7,4];
   
   //选定一个基准值    准备两个空数组  left  right  比基准值小的放left中 大的放right中  
   
   // 不断的重复这个操作   直到所有的数组长度为1  结束  
   
   // 1    基准值 4     [2,1,3]  4   [6,8,5,7]
   // 2    基准值 3     [2,1] 3 [] 4 [6,8,5,7]
   // 3    基准值 1     []  1 [2]  3 [] 4 [6,8,5,7]
   // 4    基准值 7     []  1 [2]  3 [] 4  [6,5]   7 [8]
   // 5    基准值 5      []  1 [2]  3 [] 4  [] 5 [6] 7 [8]
   //      所有的数组拼接成一个数组         1 2 3 4 5 6 7 8 
   数组.concat(数组或者元素,数组或者元素,...数组或者元素)
   // 如果是数组 那么就拆出里边的元素 拼接到后面 
   
   写递归的三个条件  
   1. 函数 
   2. 结束的条件 
   3. 如果没有达到结束的条件 那么return 返回  
   
   
   // 每一步都在重复 找到基准值 准备两个数组    比基准值小的放左边 比基准值大的放右边 
   // 直到数组的长度为1 才结束  
   function  quicksort(arr){
   	if(arr.length<=1) return arr; // 如果长度是0或者1 那么不用循环了 结束
   	//准备基准值 始终拿最后一个元素作为基准值
   	var point = arr[arr.length-1];
   	var left = right = []; //准备两个空数组  
   	// 挨个跟基准值比较 比基准值小的放左边  大的放右边 
   	for(var i=0;i<arr.length-1;i++){
   		if(arr[i]>point){
   			right.push(arr[i]); // 得到一个数组 如果长度大于1 也要进行同样的操作 
   		}
   		else{
   			left.push(arr[i]);  // 得到一个数组  如果长度大于1 也要进行同样的操作 
   		}
   	}
   	return quicksort(left).concat(point,quicksort(right))
   }
   ```

5.  解决数组塌陷 两种方式  

   1. 从右往左删除
   2. 删除了一个元素之后  下标减一 表示重新再来一次 

   * 只能删除一个叫数组塌陷

   * 下标一直向后进行但是元素往前移动补位一定有元素被忽略这就是数组塌陷

### 数组去重

1. 第一种

   * 准备一个空数组，遍历原数组

   * 挨个判断原数组中的每个元素是否在新数组中，不在就把元素加进去

   * 新数组中最后一定是唯一的元素

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

   * 前后比较   如果相同 删除后一个  

   * 前提 得先排序   只有排序才能把相同的挨在一起

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

   * 数组.indexOf(元素,从哪个位置开始往后查找)

   * 返回元素第一次出现的位置  找不到返回-1

   * 始终从下一个元素到最后 查找这个元素是否在这个范围内

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

   *  使用集合数据结构  

   * 集合里边的元素 一定唯一的  

   * 数组转成集合  集合再转回数组   

     ```JS
      var nums = [1, 15, 20, 1, 32, 7, 3, 1, 2, 7, 4, 7, 9];
     var s = new Set(nums);
     console.log(s);
     ```

### 集合转数组

1. 集合转回数组

   ```js
   var res1 = Array.from(s); 
   console.log(res1);
   ```

2. ```
    集合转回数组
   把集合中的数据 取出来  然后放到空数组中
   ... 展开运算符  
   可以展开 数组可以展开对象 可以展开集合 可以展开 map    set
   ```

3. 集合转数组的方法

   ```js
   var data = [...s]
   console.log(data);
   ```

### 数组的高级方法 (es5新增)

```js
push()
pop()
unshift()
shift()
reverse()
sort()
splice()
---影响原数组
concat()
slice()
join()
indexOf()
lastIndexOf()
---不影响原数组
```

1. forEach()

   ```js
   数组.forEach(function(item,index,origin){
        console.log(item)    //数组中的每个元素  六个对象
        console.log(index )  //每个元素的索引 0 - 5
        console.log(origin)  //原数组 6个元素打印六次 原数组
   })
   //上面三个参数 需要啥写啥
   
   例子1：修改每个元素的值
   数组.forEach(function(item){
       item.元素 = 新值
   })
   ```

   * 从左往右 数组的元素 索引 原数组
   * 名字可以是  a b c 
   * 一般第一个参数必须写

   * 作用：调用数组的每个元素，将元素传递给回调函数

   * 返回值：没有

   * 注意：对空数组不会执行回调函

2. map()

   * 作用：返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
   * 返回值： 和原始数组长度一样的数组, 只不过内部数据经过映射加工 加工以后的新数组 
   * 注意：条件以 return 的形式书写

   ```js
   数组.map(function(item,index,origin){
        console.log(item)    //数组中的每个元素  六个对象
        console.log(index )  //每个元素的索引 0 - 5
        console.log(origin)  //原数组 6个元素打印六次 原数组
   })
   上面三个参数 需要啥写啥
   ```

   ```js
   例子1：每个都加heep
   var 变量 = 数组.map(function(item){
        return 'http'+item
   })
   例子2：输出[{1:10},{2:20},{3:30}]
   var kv = [
       {key : 1 , value : 10},
       {key : 2 , value : 20},
       {key : 3 , value : 30}
   ]
   var 变量 = 数组名.mep(finction(item){
           var 新变量 = {}
           新变量[item['key']] = item['value']
           return 新变量
   })
   cunsole.log(新变量)
   ```

3. filter()

   ```js
   var 变量 = 数组.filter(function(item){
   return item.键>60
   })
   console.log(变量)
   ```

   * 作用：过滤数组
   * 返回值：一个新数组
   * 注意：条件以 return 的形式书写

4. find()

   * 返回值：输出一个条件满足条件

5. findIndex()

   * 返回值：输出满足条件

6. every()

   * 数组中有一个是假就是假
   * 返回值：一个布尔类型

7. some()

   * 数组中都满足就是true
   * 返回值：一个布尔类型

8. reduce()

   *  数组.reduce(function(prev,item,index,oring)  {} ,init)
     * prev : 表示初始值或者上一次的运算结果
     * item ： 表示数组的每一项
     * index ： 表示数组每一项的索引、
   * 作用：实现叠加效果
   * 返回值： 最终叠加效果
   * 注意：reduct() 对空数组不会执行函数，有初始值输出，没有报错
     * 没有传 prev init 0就是数组下标对应的元素	

### 基本包装类型  了解

1. 基本数据类型
   1. Number
   2. String
   3. Boolean
   4. Null
   5. Undefined
2. 引用数据类型
   1. Function
   2. Object
   3. Array

* 字符串.length
* 字符串.indexOf(字符)
* 字符串.toFixed（2）

## 认识字符串

> 字符串是字符的集合
>
> 引号包裹  单 反单引号 

### 创建字符串

1. 字面量

   ```
   var num1 = '...';
   var num2 = "...";
   var num3 = `...`;
   ```

2. 内置构造函数方式

   ```
   var num = new String('...')
   ```

#### 注意事项：

* 基本数据类型原值永不改变，会给你返回一个新的值

## 字符集 了解

> 计算最终用二进制去处理
>
> 字符喝二进制的一个转换关系对照表

ASCII代码

```js
0   1  => abc ... z 标点符号等 给它们编号 10进制
0  ~ 127  编号 一共128
```

unicode 万国码 统一码

```js
前 128 是ASCII
从129开始 依次是每个国家的文字
存在两种 unicode
八位的进制十六进制   unicode
16位的十六进制      unicode
```

gbk2312->gbk

```js
前 128 是ASCII
从129开始 依次是每个汉字
```

网络传输 使用八位的十六进制

传输中起名字 utf-8 八位的十六进制 unicode编码

中文 4e00-9ea5

## 字符串的基本操作

```
length
    只读
索引
    只读
变量
    支持遍历
```

## 字符串的19个方法

```js
      1. charAt()
            + 语法: 字符串.charAt(索引)
            + 返回值: 该索引位置的字符
              => 如果没有该索引位置, 返回的是 空字符串

      2. charCodeAt()
        + 语法: 字符串.charCodeAt(索引)
        + 返回值: 该索引位置字符的 unicode 编码
          => 如果没有该索引位置, 返回的是 NaN

      3. toUpperCase()
        + 语法: 字符串.toUpperCase()
        + 返回值: 将原始字符串内的所有字母转换成大写

      4. toLowerCase()
        + 语法: 字符串.toLowerCase()
        + 返回值: 将原始字符串内的所有字母转换成小写

      5. substr()
        + 语法: 字符串.substr(开始索引, 多少个)
        + 返回值: 截取出来的部分字符串

      6. substring()
        + 语法: 字符串.substring(开始索引, 结束索引)
        + 特点: 包前不包后
        + 返回值: 截取出来的部分字符串

      7. slice()
        + 语法: 字符串.slice(开始索引, 结束索引)
        + 特点: 包前不包后, 填写负整数
        + 返回值: 截取出来的部分字符串

      8. split()
        + 语法: 字符串.split('分隔符')
        + 返回值: 是一个数组
          => 按照分隔符把字符串分开成为几段内容

      9. concat()
        + 语法: 字符串.concat(字符串1, 字符串2, ...)
        + 返回值: 拼接好的字符串

      10. indexOf()
        + 语法: 字符串.indexOf(查找的字符, 开始索引)
        + 返回值:
          => 如果原始字符串内有该字符串片段, 那么是该字符串片段第一次出现的首字母索引位置
          => 如果原始字符串内没有该字符串片段, 那么是 -1

      11. lastIndexOf()
        + 语法: 字符串.lastIndexOf(字符串片段, 开始索引)
        + 返回值:
          => 如果原始字符串内有该字符串片段, 那么是该字符串片段第一次出现的首字母索引位置
          => 如果原始字符串内没有该字符串片段, 那么是 -1

      12. includes()
        + 语法: 字符串.includes(字符串片段)
        + 作用: 该字符串中是否包含该字符串片段
        + 返回值: 一个布尔值
          => true 说明有该字符串片段
          => false 说明没有该字符串片段

      13. startsWith()
        + 语法: 字符串.startsWith(字符串片段)
        + 作用: 判断该字符串是否以该字符串片段开头
        + 返回值: 一个布尔值
          => true 说明以该字符串片段开头
          => false 说明不以该字符串片段开头

      14. endsWith()
        + 语法: 字符串.endsWith(字符串片段)
        + 作用: 判断该字符串是否以该字符串片段结尾
        + 返回值: 一个布尔值
          => true 说明以该字符串片段结尾
          => false 说明不以该字符串片段结尾

      15. trim()
        + 语法: 字符串.trim()
        + 作用: 去除字符串首尾空白
        + 返回值: 去除首尾空白后的字符串

      16. trimStart() / trimLeft()
        + 语法:
          => 字符串.trimStart()
          => 字符串.trimLeft()
        + 返回值: 去除开始位置空白以后的字符串

      17. trimEnd() / trimRight()
        + 语法:
          => 字符串.trimEnd()
          => 字符串.trimRight()
        + 返回值: 去除结束位置空白以后的字符串

      18. repalce()
        + 语法: 字符串.replace(换下字符, 换上字符)
        + 作用: 替换原始字符串内的片段
        + 注意: 只能替换一个
        + 返回值: 替换好的字符串
      19. String.fromCharCode()
```

## 查询字符串

> 分解：https://    http://   协议       s  safe 证书
>
> ​            www.baidu.com :80      域名+端口号   http :默认 80    443（可写可不写）
>
> ​          s    //路径   通过路径访问指定的网页
>
> ​          ?        后面跟着参数  多个之间 & 隔开
>
> ​         `# `   锚点
>
> js 数据类型  对象也是键值对  js支持的格式
>
> 

## json字符串

字符串类型

1. 普通字符串

   * " "   ''    ``

2. url字符串

   * 查询字符串

3. html字符串

   * `<div> ... </div>`

4. 数字字符串

   * “1”

5. json字符串

   * 本质是字符串   所有编程语言都接受
   * 格式：对象的键一定是双引号包裹 键对应的值如果是字符串 也要用双引号包裹
   * 数组：数组的元素如果是字符串也要用双引号包裹

   JSON.stringify() 普通转数组

   JSON.parse()  转普通的对象数组

   xml  

   * 跟html一样  可以是单标签也可以是双标签  ，名字可以自己定义
   * 为了替代xml 找到了一个更清凉的字符串  json

## 数学对象和日期对象

> Math  和   Date

* Math 是js的内置对象，帮助我们操作**数字**
* Date   是js的内置对象，帮助我们操作**时间**

## math

> Math.属性    

1. random()
   * Math.random()
   * 作用：获取**随机数**
2. round()
   * Math.round()
   * 作用：将小数**四舍五入**
3. cail()
   * Math.cail()
   * 作用：是将一个小数 **向上取整** 得到的整数
4. floor()
   * Math.floor()
   * 作用： 是将一个小数 **向下取整** 的到的整数
5. abs()
   * Math.abs()
   * 作用： 是返回一个数字的 **绝对值**
6. pow()
   * Math.pow(底数，指数)
   * 作用：幂运算
7. sprt()
   * Math.sprt()
   * 作用：去数的平方根
8. max()
   * Math.max(数字，数字，数字.....)
   * 作用：得到的是你传入的几个数字之中 **最大** 的那个数字
9. min()
   * Math.min(数字，数字，数字.....)
   * 作用： 得到的是你传入的几个数字之中 **最小** 的那个数字
10. PI
    * Math.PI
    * 作用： 得到的是 `π` 的值
    * 使用 Math.PI 的时候，是不需要加 () 的

```js
1-6
var num = Math.floor(Math.random() * 6 + 1);
0-2
var num = Math.floor(Math.random() * 3 + 0);
```

#### 数字转换进制

1. `toString()` 方法可以在数字转成字符串的时候给出一个进制数

- 语法： `toString(你要转换的进制)`

- ```js
  console.log(num.toString(2)//10 转 2  8  16
  ```

2. parseInt()` 方法可以在字符串转成数字的时候把字符串当成多少进制转成十进制

- 语法： `parseInt(要转换的字符串，当作几进制来转换)`

- ```js
  console.log(parseInt(str, 2))//其他进制转10进制
  ```

## 日期对象

#### 日期的获取 get

##  get

```
 1. getFullYear()
        + 语法: 时间对象.getFullYear()
        + 返回值: 该时间对象内的 年份信息

      2. getMonth()
        + 语法: 时间对象.getMonth()
        + 返回值: 该时间对象内的 月份信息
        + 注意: 0 表示 1 月, 11 表示 12 月

      3. getDate()
        + 语法: 时间对象.getDate()
        + 返回值: 该时间对象内的 日期信息

      4. getHours()
        + 语法: 时间对象.getHours()
        + 返回值: 该时间对象内的 小时信息(24小时制)

      5. getMinutes()
        + 语法: 时间对象.getMinutes()
        + 返回值: 该时间对象内的 分钟信息

      6. getSeconds()
        + 语法: 时间对象.getSeconds()
        + 返回值: 该时间对象内的 秒钟信息

      7. getMilliseconds()
        + 语法: 时间对象.getMilliseconds()
        + 返回值: 该时间对象内的 毫秒信息

      8. getDay()
        + 语法: 时间对象.getDay()
        + 返回值: 该时间对象内的 星期信息
          => 一周中的第几天
        + 注意: 0 表示 周日, 1 ~ 6 表示 周一到周六

      9. getTime()
        + 语法: 时间对象.getTime()
        + 返回值: 该时间对象内的 时间戳 1970的1月1日0时0分0秒到现在的秒数  
        + 时间戳: 从 格林威治时间 到 该时间对象 的毫秒数
```

#### 日期的设置 set

 	  1. setFullYear()
        + 语法: 时间对象.setFullYear(年份信息)
        
+ 作用: 修改该时间对象内的 年份信息
      
              2. setMonth()
          
        + 语法: 时间对象.setMonth(月份信息)

      + 作用: 修改该时间对象内的 月份信息
        
        + 注意: 0 表示 1 月, 11 表示 12 月

            3. setDate()
          
        + 语法: 时间对象.setDate(日期信息)

      + 作用: 修改该时间对象内的 日期信息
        
              4. setHours()
        
      + 语法: 时间对象.setHours(小时信息)
        
        + 作用: 修改该时间对象内的 小时信息

            5. setMinutes()
          
        + 语法: 时间对象.setMinutes(分钟信息)

      + 作用: 修改该时间对象内的 分钟信息
        
              6. setSeconds()
          
        + 语法: 时间对象.setSeconds(秒钟信息)
        
        + 作用: 修改该时间对象内的 秒钟信息
        
              7. setMilliseconds()
          
        + 语法: 时间对象.setMilliseconds(毫秒信息)
        
        + 作用: 修改该时间对象内的 毫秒信息
        
              8. setTime()
          
        + 语法: 时间对象.setTime(时间戳信息)
        
        + 作用: 使用时间戳信息直接定位时间对象

### 定时器

#### 倒计时定时器

1. 语法： `setTimeout(要执行的函数，多长时间以后执行)`
2. 倒计时多少时间以后执行函数
3. 返回值是，当前这个定时器是页面中的第几个定时器

注意:

1. 时间是按照毫秒进行计算的，1000 毫秒就是 1秒钟
2. 只执行一次，就不在执行了

```js
var timerId = setTimeout(function () {
  console.log('我执行了')
}, 1000)
console.log(timerId) // 1
```

####  间隔定时器

1. 每间隔多少时间就执行一次函数
2. 语法： `setInterval(要执行的函数，间隔多少时间)`
3. 返回值是，当前这个定时器是页面中的第几个定时器

注意：

1. 每间隔 1 秒钟执行一次函数
2. 只要不关闭，会一直执行

```js
var timerId = setInterval(function () {
  console.log('我执行了')
}, 1000)
```

####  定时器的返回值

1. 设置定时器的时候，他的返回值是部分 `setTimeout` 和 `setInterval` 的
2. 只要有一个定时器，那么就是一个数字

#### 关闭定时器

1.  `timerId` 就是用来关闭定时器的数字
2. 两个方法来关闭定时器 `clearTimeout` 和 `clearInterval`

```js
var timerId = setTimeout(function () {
  console.log('倒计时定时器')
}, 1000)
clearTimeout(timerId)

var timerId2 = setInterval(function () {
  console.log('间隔定时器')
}, 1000)
coearInterval(timerId2)
```

## Bom

* `BOM（Browser Object Model）`： 浏览器对象模型
* `BOM` 的核心就是 `window` 对象

###  获取浏览器窗口的尺寸

1. `innerHeight`  设置浏览器高度
2.  `innerWidth  `  设置浏览器宽度

### 浏览器的弹出层

1. `alert` 是在浏览器弹出一个提示框
   * 这个弹出层知识一个提示内容，只有一个确定按钮，
2. `confirm` 是在浏览器弹出一个询问框
   * 这个弹出层有一个询问信息和两个按钮
     * 确定 true  取消false
3. `prompt` 是在浏览器弹出一个输入框
   * 这个弹出层有一个输入框和两个按钮
     * 当你点击取消的时候，得到的是 `null`
     * 当你点击确定的时候得到的就是你输入的内容

###  浏览器的地址信息

* 在 `window` 中有一个对象叫做 `location`
* 就是专门用来存储浏览器的地址栏内的信息的

1.  location.href

   * `location.href` 这个属性存储的是浏览器地址栏内 `url` 地址的信息

   ```js
   console.log(window.location.href)
   ```

   * `location.href` 这个属性也可以给他赋值

   ```js
   window.location.href = './index.html'
   // 这个就会跳转页面到后面你给的那个地址
   ```

2.  location.reload

   * `location.reload()` 这个方法会重新加载一遍页面，就相当于刷新是一个道理

   ```js
   window.location.reload()
   ```

###  浏览器的历史记录

* `window` 中有一个对象叫做 `history`
* 是专门用来存储历史记录信息的

1. history.back()
   * 是用来会退历史记录
2. history.forword()
   * 是去到下一个历史记录里面
3. history.go()
   * 正整数 前进  0 刷新  负数 回退

####  浏览器的 onload 事件

* 这个不在是对象了，而是一个事件
* 是在页面所有资源加载完毕后执行的

```js
window.onload = function () {
  console.log('页面已经加载完毕')
}
```

1.  在 html 页面中把 js 写在 head 里面
2. 在 html 页面中把 js 写在 body 最后面

####  浏览器的 onscroll 事件

* `onscroll` 事件是当浏览器的滚动条滚动的时候触发
* 或者鼠标滚轮滚动的时候出发

* 注意：**前提是页面的高度要超过浏览器的可是窗口才可以**

```js
window.onscroll = function () {
  console.log('浏览器滚动了')
}
```

#### 浏览器onresize事件

* 改变浏览器尺寸

#### 浏览器标签页

* window.open()
* window.close()

####  浏览器滚动的距离

#####  scrollTop

* 获取的是页面向上滚动的距离

1. document.body.scrollTop

2. document.documentElement.scrollTop

   ```js
   window.onscroll = function () {
     console.log(document.body.scrollTop)
     console.log(document.documentElement.scrollTop)
   }
   ```

##### scrollLeft

* 获取页面向左滚动的距离

1. `document.body.scrollLeft`
2. `document.documentElementLeft`

```js
window.onscroll = function () {
  console.log(document.body.scrollLeft)
  console.log(document.documentElement.scrollLeft)
}
```

## Dom

浏览器本地存储

1. localStoraage
2. sessionStorage
3. cookie

DOM操作

1. 获取元素
2. 元素样式
3. 元素标签
4. 操作标签中间内容
5. 案例

### 浏览器本地存储

> JSON.stringify()  js数据类型 => json
>
> JSON.parse() json格式转 普通js类型

读

1. 存储中取出来

写

1. 往里新增数据      
2. 修改里面数据
3. 删里面数据

##### localStorage

* 一旦存储永远存在
* 只能存储基本数据类型 不能存储复杂数据类型，如果想存储复杂数据类型 通过JSON.stringify 进行转化在存储
* 同一个域名下的垮页面通信

读

* window.localStorage.getItem('键名',对应的值)
* 不存在返回null
* 存在 返回键对应的值

写

* 增   window.localStorage.setItem('键名',新值)

* c

* 删  window.localStorage.removeItem('键')

  ​      window.localStorage.clear();   清空

##### sessionStorage

* 临时存储 关闭浏览器 自动消失
* 只能存储字符串
* 可以跨页面访问，但是有要求，如a页面存储了，从a页面跳转过去的页面才可以访问，否则即使同一个域名也不可以

读

* window.localStorage.getItem('键名',对应的值)
* 不存在返回null
* 存在 返回键对应的值

写

* 增   window.localStorage.setItem('键名',新值)

* 改  window.localStorage.setItem('键名',对应的值)

* 删  window.localStorage.removeItem('键')

  ​      window.localStorage.clear();

##### cookie

注意：

1. 存储的也是字符串 key = value

2. 大小有限制 4kb

3. 存储的时效性

   1. 手动设置过期时间
   2. 默认时 关闭浏览器没了

4. 必须依赖服务器

   1. 不能在本地直接双击来打开页面
   2. 只能用 127.0.0.1:5500

5. 支持程序员往里放

   * 浏览器自动携带
   * 服务器自动给浏览器

6. 前端

   * js操作cookie

   后端

   * 任意语言

7. 同一个域名才能通讯 那个存储那个使用

设置一条cookie

document.cookie = '键 = 值’

获取cookie

document.cookie

### dom

js操作html页面的标签（元素）让他发生变化

```js
修改文本
标签的增删改查
标签的样式等
```

1. 获取元素

   1. 非常规元素
      1. html
         * document.documentElement
      2. body
         * document.body
      3. hear
         * document.hear

   ```js
       console.log(document.documentElement)
       console.log(document.body)
       console.log(document.head)
   ```

   2. 常规元素

   1. id      不加s

      * document.getElementByld('id名字‘)
      * 返回的是一个元素
      * 找不到返回null

   2. class    加s

      * document.getElementsByClassName（’class名字‘）
      * 可能是多个元素，返回的是一个伪数组，不能当伪数组用
      * 找不到返回空的伪数组

   3. 标签名   加s

      * document.getElementsByTagName('标签名字 ’）
      * 可能是多个元素，返回的是一个伪数组，不能当伪数组用
      * 找不到返回空的伪数组

   4. name    加s

      * document.getElementsByName('name名字')
      * 可能是多个元素，返回的是一个伪数组，不能当伪数组用
      * 找不到返回空的伪数组

   5. 选择器  

      querySelector()

      1. 符合条件的第一个元素
      2. 找不到返回nul

      querySelectorAll()

      1. 可能是多个元素，返回的是一个伪数组，不能当伪数组用
      2. 找不到返回空的伪数组

### dom操作样式

1. 行内
2. 内联
3. 外链

##### 操作样式

1. 获取
   1. 行内
      * 元素.style.样式名字
      * 只能获取行内
      * 如果样式名字之间有横杠，要转成小驼峰命名法
   2. 内联/外链
      * window.getComputedStyle(元素).样式名字
2. 设置
   * 只能设置行内，没法设置内联和外链
   * 元素.style.样式名字 = ’样式值'

#### 操作属性

```js
<div id="" class="" style="">
<input type="txex">
id class style type  属于w3c内置的属性

checked  disabled  selseted 这些事布尔类型属性
<input type="radio" checked=false>
<input type="text" disabled>
select
      option selseted
      
 //daring="1314" 对标签没有影响 仅仅记录一些东西
<div id="" class="" style="" daring="1314">
      
date-属性  h5新增 仅仅用来区分 或者记录一些东西
<div id="" class="" darling="520" date-haha='test'>

```

#### 操作

1. 原生属性

   * 读

     * 语法： 元素 . 属性名

   * 写

     * 语法 ： 元素 . 属性名 = 属性的值

       布尔类型属性   checked  selected  disabled

     * 读 ： 元素 . disabled
     * 写：元素 . disabled = false

2. 自定义

   * 设置
     * 元素 . setAttribute（属性名，属性值）
   * 删
     * 元素 . removeAttribute（属性名）
   * 查  获取
     * 元素 . getAttribute（属性名）
     * 如果属性名不存在返回null
   * 修改
     * 元素 . setAttribute（存在的属性名，新的属性值）

   上面四个方法适用于h5新增自定义属性 date-开头

3. h5新增自定义属性

   * 每个标签天生自带一个属性 叫dataset,datase中就是所有data-开头的属性
   * 如果拿到dataset属性，就等于拿到所有data-开头属性
   *  dataset 是一个对象

   ```js
   <div id="abc123" data-id="abc456" data-user-name="张三">还得是老罗</div>
   	
   	
   	var divEle = document.getElementById('abc123');
           // console.log(divEle.dataset,typeof divEle.dataset);  // object
           // for(var v in divEle.dataset){
           //     // console.log(v)  v是键  id  userName
           //     console.log(divEle.dataset[v])
           // }
           console.log(divEle.dataset.id)
           console.log(divEle.dataset.userName) //小驼峰
           console.log(divEle.dataset['userName'])//数组关联语法 
   ```

####  dom操作类

```js
1.className 

	读: 
		元素.className
	写:
		元素.className = '值'; //完全覆盖
		元素.className += ' 值'; // 追加类名值得前面要有空格
		
		
	<div class="one two three">

    </div>
    
2. 每个标签天生自带一个属性  classList 包含着你所有的类名  是个对象 

	追加类名 
		元素.classList.add('新类名')
	删除类名
		元素.classList.remove('新类名')
	切换类名
		元素.classList.toggle('新类名')
		如果有就把原来的删除掉 
		如果没有就添加  
```

### dom操作元素标签的内容

> 操作标签中间的内容

1. innerText    纯文本

   1. 把标签中的内容全部当做字符串来看待 不解析标签

      支持读写

      1. 读

      *  元素.innerText 得到是文本 不包含标签

      2. 写

      * 元素.innerText = '内容'; 完全覆盖 标签原样显示

2. innerHTML    文本和子标签

   1. 可以解析html标签

* 读
  * 元素.innerHTML 得到是文本 包含标签

             ```js
             2. 写
                *  元素.innerHTML = '内容'; 完全覆盖 标签被解析
             ```

3. value 关于表单

* 读
  * 表单元素.value 

* 写
  * 表单元素.value = '值';

### 元素尺寸

> 元素在页面的占地面积	
>
> display : none  拿不到数据
>
> visbility : hidden   可以拿到数据

1. offset

   元素的内容  + 内边距  padding   +  边框  

   1. 先拿到元素
      * 元素.offsetWidth
      * 元素.offsetHeight

2. client

   内容 + 内边距 padding

   1. 元素.clientWidth
   2. 元素.clientHeight

### 偏移量

1. left
2. top

1. offset

   1. 元素.offsetLeft

   2. 元素.offsetTop

   3. 相对于定位父级，左边和上边的距离

2. client

   1. 元素.clientLeft
   2. 元素.clientTop
   3. 简单来说就是左边框和上边框的宽度

父级

1. 结构父级

   按照w3c标准 ，下载标签外层标签，称为父级

2. 定位父级  offsetParent

   如果标签没有给绝对定位，以谁位参考标准，谁就是这个标签的地位父级

   ![](F:\二阶段笔记\18dbb8dd4c00e68be77f7f9d48667fc.png)

### 可视化窗口的尺寸

1. bom 级别

   window.innerWidth

   window.innerHeight

   包含滚动条

2. dom级别

   document.documentElement.clientWidth

   document.documentElement.clientHeight

   不包含滚动条

### 瀑布流

![](F:\二阶段笔记\892350c85ac36af73514a269382860b.png)



滚动条 滚动 用scroll  事件

滚动条滚动判断是否加载新内容

```js
window.onscroll = dunction (){ 
    var 卷去高 = document.documentElement.offsetHeight || document.body.scrollTop;
    var 可视化窗口高度 = window.innerHeight;
    var 元素尺寸 = 元素.offsetHeight;
    var 定位高度 = 元素.offsetTop;
    //加载下一页条件
    if(卷去高 + 可视化窗口高 > 元素尺寸 + 定位高){
        console.log('开始加载')
    }
     console.log('不加载')
}
```

### dom升级

##### 节点

1.    元素节点        1
   * `<div> </div>     <p>   </p>     `
2. 属性节点     2
   * class = 'text'  id = 'a1'2
   * 不作为子元素存在
3. 文本节点    3
   * wall  eye     knee   text 
4. 注释节点     8
   * `<!-- -->`      comment

#### **获取节点**

元素对象.childNodes   节点

##### **children**

​                ● 作用：children ：获取某一节点下所有的子一级 **元素节点**

​                ● 语法：元素对象.children

##### **firstChild**

​                ● 作用：firstChild：获取某一节点下子一级的 **第一个节点**

​                ● 语法：元素对象.firstChild

##### **lastChild**

​                ● 作用：lastChild：获取某一节点下子一级的 **最后一个节点**

​                ● 语法：元素对象.lastChild

##### **firstElementChild**

​                ● 作用：firstElementChild：获取某一节点下子一级 **第一个元素节点**

​                ● 语法：元素对象.firstElementChild

##### **lastElementChild**

​                ● 作用：lastElementChild：获取某一节点下子一级 **最后一个元素节点**

​                ● 语法：元素对象.lastElementChild

##### **nextSibling**

​                ● 作用：nextSibling：获取某一个节点的 **下一个兄弟节点**

​                ● 语法：元素对象.nextSibling

**previousSibling**

​                ● 作用：previousSibling：获取某一个节点的 **上一个兄弟节点**

​                ● 语法：元素对象.previousSibling

##### **nextElementSibling**

​                ● 作用：nextElementSibling：获取某一个节点的 **下一个元素节点**

​                ● 语法：元素对象.nextElementSibling



##### **previousElementSibling**

​                ● 作用：previousElementSibling：获取某一个节点的 **上一个元素节点**

​                ● 语法：元素对象.previousElementSibling

##### **parentNode**

​                ● 作用：parentNode：获取某一个节点的 **父节点**

​                ● 语法：元素对象.parentNode

##### **parentElement**

​                ● 作用：parentElement获取某一节点的**父元素**节点

​                ● 语法：元素对象.parentElement

##### **attributes**

​                ● 作用：attributes：获取某一个 **元素节点** 的所有 **属性节点

​              ● 获取的是一组数据，是该元素的所有属性，也是一个伪数组

### 节点属性

​                ○ nodeType

​                ○ nodeName

​                ○ nodeValue

## **nodeType**

​                ● nodeType：获取节点的节点类型，用数字表示

```js
console.log('元素节点 : ', ele.nodeType) // 1
console.log('属性节点 : ', attr.nodeType) // 2 
console.log('文本节点 : ', text.nodeType) // 3
console.log('注释节点 : ', comment.nodeType) // 8
```

​                ● nodeType === 1 就表示该节点是一个 **元素节点**

​                ● nodeType === 2 就表示该节点是一个 **属性节点**

​                ● nodeType === 3 就表示该节点是一个 **文本节点**

​                ● nodeType === 8 就表示该节点是一个 **注释节点**

## **nodeName**

​                ● nodeName：获取节点的节点名称

```js
console.log('元素节点 : ', ele.nodeName) // DIV
console.log('属性节点 : ', attr.nodeName) // class
console.log('文本节点 : ', text.nodeName) // #text
console.log('注释节点 : ', comment.nodeName) // #comment
```

​                ● 元素节点的 nodeName 就是 **大写标签名**

​                ● 属性节点的 nodeName 就是 **属性名**

​                ● 文本节点的 nodeName 都是 **#text**

​                ● 文本节点的 nodeName 都是 **#comment**

## **nodeValue**

​                ● nodeValue： 获取节点的值

```js
console.log('元素节点 : ', ele.nodeValue) // null 
console.log('属性节点 : ', attr.nodeValue) // box
console.log('文本节点 : ', text.nodeValue) // hello world
console.log('注释节点 : ', comment.nodeValue) // 你好 世界
```

​                ● 元素节点没有 nodeValue返回的是**null**

​                ● 属性节点的 nodeValue 就是 **属性值**

​                ● 文本节点的 nodeValue 就是 **文本内容**

​                ● 文本节点的 nodeValue 就是 **注释内容**

# **操作 DOM 节点**

​                ● 我们所说的操作无非就是 **增删改查（CRUD）**

## **创建一个节点**

​                ● createElement：用于创建一个元素节点

​                ● 语法：document.createElement('要创建的标签')

​               ● 创建出来的就是一个可以使用的 div 元素

​                ● createTextNode：用于创建一个文本节点

​                ● 语法：document.createTextNode('要写的文本内容')

​                ● 返回值：就是一个文本节点, 不是字符串

## **插入一个节点**

​                ● appendChild：是向一个元素节点的末尾追加一个节点

​                ● 语法： 父节点.appendChild(要插入的子节点)

​               ● insertBefore：向某一个节点前插入一个节点

​                ● 语法： 父节点.insertBefore(要插入的节点，插入在哪一个节点的前面)

## **删除一个节点**

### **removeChild**

​                ● removeChild：移除某一节点下的某一个节点

​                ● 语法：父节点.removeChild(要移除的子节点)

### **remove**

​                ● remove：移除某个节点，那个节点要移除直接调用这个方法

​                ● 语法：元素节点.remove('要移除的节点')

## **替换一个节点**

​                ● replaceChild：将页面中的某一个节点替换掉

​                ● 语法： 父节点.replaceChild(新节点，旧节点)

## **克隆一个节点**

​                ● 语法：节点对象.cloneNode(参数)

​                ○ 参数是一个布尔值true或者false,不写就是默认的参数是false

​                ○ false代表只克隆本身的标签，标签里面的所有节点不可隆，也就的不可隆后代节点

​                ○ true代表全部可隆，本身节点以及所有的后代节点

​                ● 返回值：就的克隆好的一个新节点

# **026_DOM 事件**

​                ● 是指用户在某事务上由于某种行为所执行的操作; （对页面元素的某种操作）

​                ● 一个事件由哪几部分组成

​                ○ 触发谁的事件：事件源

​                ○ 触发什么事件：事件类型

​                ○ 触发以后做什么：事件处理函数（事件处理程序）

```js
        //dom0级绑定
        var btn = document.querySelector('button');
        btn.onclick = function () {
            alert('jingjing')
        }
        //一个事件只能绑定一个函数 后面会把前面替换掉
```

# **事件绑定和事件解绑**

## **事件绑定**

​                ● 事件绑定有DOM 0级和DOM 2级  

​                ● 没有DOM 1级

### **DOM 0级事件绑定**

​                ● 我们现在给一个注册事件都是使用 onxxx 的方式

​                ● 一旦写了第二个事件，那么第一个就被覆盖了

​                ● 语法：事件源.onclick = 事件处理程序（事件处理函数）

```js
oDiv.onclick = function () {
  console.log('我是第一个事件')
}

oDiv.onclick = function () {
  console.log('我是第二个事件')
}
```

​                ● 当你点击的时候，只会执行第二个，第一个就没有了

### **DOM 2级事件绑定  事件监听**

#### **标准浏览器**

​                ● 使用 addEventListener 的方式添加。addEventListener :  非 IE 7 8 下使用

​                ● 语法： 元素.addEventListener('事件类型'， 事件处理函数， 冒泡还是捕获)

​                ● 可以给同一个事件源的同一个事件类型绑定多个事件处理函数

```js
oDiv.addEventListener('click', function () {
  console.log('我是第一个事件')
}, false)

oDiv.addEventListener('click', function () {
  console.log('我是第二个事件')
}, false)
```

​                ● 当你点击 div 的时候，两个函数都会执行，并且会按照你注册的顺序执行

​                ● 先打印 我是第一个事件 再打印 我是第二个事件

​                ● 注意： **事件类型的时候不要写 on，点击事件就是 click，不是 onclick**

#### **IE低版本浏览器**

​                ● attachEvent ：IE 7  8 下使用

​                ● 语法： 元素.attachEvent('事件类型'， 事件处理函数)

​                ● 可以给同一个事件源的同一个事件类型绑定多个事件处理函数

```js
oDiv.attachEvent('onclick', function () {
  console.log('我是第一个事件')
})

oDiv.attachEvent('onclick', function () {
  console.log('我是第二个事件')
})
```

​                ● 注意： **事件类型的时候要写 on，点击事件就行 onclick**

#### **两个方式的区别**

​                ● 注册事件的时候事件类型参数的书写

​                ○ addEventListener ： 不用写 on

​                ○ attachEvent ： 要写 on

​                ● 参数个数

​                ○ addEventListener ： 一般是三个常用参数

​                ○ attachEvent ： 两个参数

​                ● 执行顺序

​                ○ addEventListener ： 顺序注册，顺序执行

​                ○ attachEvent ： 顺序注册，倒叙执行

​                ● 适用浏览器

​                ○ addEventListener ： 非 IE 7 8 的浏览器

​                ○ attachEvent ： IE 7 8 浏览器

## **事件解绑**

### **DOM 0级事件解绑**

​                ● 语法：事件源.on事件类型 = null

​                ● 因为赋值符号, 当你给这个事件类型赋值为 null 的时候，会把本身的事件处理函数覆盖

```js
btn.onclick = function(){
            alert('赵媳妇儿')
        }
        btn.onclick = null;   //dom0 解绑
```

### **DOM 2级事件解绑**

#### **标准浏览器**

*  语法：事件源.removeEventListener('事件类型', 事件处理函数)

*  注意：
  * 当你使用 DOM 2级 事件解绑的时候, 因为函数是一个复杂数据类型, 所以你在绑定的时候
  * 需要把函数单独书写出来, 以函数名的形式进行绑定和解绑

```js
  //事件源.removeEventListener(事件类型，事件处理函数)
        function zhao() {
            alert('胡苇臻要退学');
        }
        function zhang() {
            alert('你在狗叫什么！！！！')
        }
        btn.addEventListener('click', zhao)
        //tese加上（）函数执行
        //这里想要函数 不是函数结果
        btn.removeEventListener('click',zhao);
```

# **常见的事件分类（了解）**

​                ● 我们在写页面的时候经常用到的一些事件

​                ● 大致分为几类，**浏览器事件** / **鼠标事件** / **键盘事件** / **表单事件** / **触摸事件**

​                ● 不需要都记住，但是大概要知道

## **鼠标事件**

​                ● click ：点击事件

​                ● dblclick ：双击事件

​                ● contextmenu ： 右键单击事件

​                ● mousedown ：鼠标左键按下事件

​                ● mouseup ：鼠标左键抬起事件

​                ● mousemove ：鼠标移动

​                ● mouseover ：鼠标移入事件

​                ● mouseout ：鼠标移出事件

​                ● mouseenter ：鼠标移入事件

​                ● mouseleave ：鼠标移出事件

*  鼠标事件
              + 就是依赖行为触发的事件
                          + over 和 out 是一套
                => 在移入和移出后代元素的时候也会触发
                          + enter 和 leave 是一套
                => 在移入和移出后代元素的时候不会触发

## **键盘事件**

​                ● keyup ： 键盘抬起事件

​                ● keydown ： 键盘按下事件

​                ● keypress ： 键盘按下再抬起事件

* 键盘事件 依赖与键盘行为
* 所有的元素都可以绑定键盘事件，不代表所有元素都能触发
* 触发键盘事件的只能是   window  document  表单元素

## **表单事件**

1. focus：聚焦（获取焦点）以后触发

2. blur：失去焦点以后触发

3. change : 表单内容改变事件，需要内容改变才触发
   1. 保证你聚焦和失焦的时候, 内容不一致才会触发
   2.  时机: 失焦以后

4. input : 表单内容输入/删除时触发该事件
   *  随着输入和删除内容实时触发

5. reset：表单重置事件。事件需要绑定给 form 标签。由 form 标签内的 reset 按钮触发
   *  事件需要绑定给 form 标签
   *  通过点击 重置 按钮触发
   * form.addEveatListener(表单事件，事件处理函数)

6.  submit : 表单提交事件。事件需要绑定给 form 标签。由 form 标签内的 submit 按钮触发
   * 事件需要绑定给 form 标签
   * 通过点击 提交 按钮触发
   * form.addEveatListener(表单事件，事件处理函数)

> 表单.addEveatListener(表单事件，事件处理函数)

## **触摸事件**

​                ● touchstart ： 触摸开始事件

​                ● touchend ： 触摸结束事件

​                ● touchmove ： 触摸移动事件

```js
      var dtn = document.querySelector('div')
        dtn.ontouchstart = function () { console.log('触摸开始') }
        dtn.ontouchmove = function () { console.log('触摸移动') }
        dtn.ontouchend = function () { console.log('触摸结束') }
```

## **拖拽事件**

​                ● 在浏览器内元素发生了拖拽行为以后触发的事件

### **拖拽元素的事件**

1. dragstart   开始拖拽 即将进入拖拽移动状态的瞬间触发

            2.     drag   拖拽移动 拖拽元素在移动的时候实时触发
               3.     dragend   拖拽结束 拖拽元素放手的时候触发



> 元素必须要有  draggable="true" 

### **拖放元素事件**

​            1.     dragenter   拖拽元素进入拖放元素范围的时候触发(注意: 光标进入)

​            2.     dragleave   拖拽元素离开拖放元素范围的时候触发(注意: 光标离开)

​            3.     dragover    拖拽元素完全进入拖放元素范围内触发

```js
pEle.ondragover = function () {

  // 一段取消影响的代码

  return false

}
```

​            4.     drop   拖拽元素在拖放元素内放手的时候触发

```
    // 获取元素
    var divEle = document.querySelector('div')
    var pEle = document.querySelector('p')

    // 拖拽元素的事件
    // 1. dragstart   开始拖拽
    //    即将进入拖拽移动状态的瞬间触发
    divEle.ondragstart = function () { console.log('你要把我拖走了') }

    // 2. drag   拖拽移动
    //    拖拽元素在移动的时候实时触发
    divEle.ondrag = function () { console.log('你拖着我正在走') }

    // 3. dragend   拖拽结束
    //    拖拽元素放手的时候触发
    divEle.ondragend = function () { console.log('你放手了') }


    // 拖放元素事件
    // 4. dragenter   拖拽元素进入拖放元素范围的时候触发(注意: 光标进入)
    pEle.ondragenter = function () { console.log('啊, 你把它拖进来了 !! ^_^') }

    // 5. dragleave   拖拽元素离开拖放元素范围的时候触发(注意: 光标离开)
    pEle.ondragleave = function () { console.log('你又把他带走了') }


    // 6. dragover    拖拽元素完全进入拖放元素范围内触发
    pEle.ondragover = function () {
      // 一段取消影响的代码
      return false
    }

    // 7. drop   拖拽元素在拖放元素内放手的时候触发
    //    注意: 需要在 dragover 事件内进行阻止默认行为
    //    drop 事件会被 dragover 事件影响, 你想触发 drop 事件, 就需要消除 dragover 事件的影响
    //    需要给元素绑定一个 dragover 事件, 并且在该事件内取消影响
    pEle.ondrop = function () { console.log('你在我身上放手了') }
```

# **事件对象**

​                ● 什么是事件对象？

​                ● 就是一个对象数据类型, 记录了本次事件的所有信息

​                ● 也就是当你触发了一个事件以后，对该事件的一些详细的描述信息。就存储在事件对象中

​                ● 每一个事件都会有一个对应的对象来描述这些信息，我们就把这个对象叫做 **事件对象**

​                ● 浏览器给了我们一个 **黑盒子**，叫做 window.event，就是对事件信息的所有描述

```
oDiv.onclick = function () {
  console.log(window.event.X轴坐标点信息)
  console.log(window.event.Y轴坐标点信息)
}
```

​                ●就会有 **兼容性问题**

​                ● 在 IE低版本 里面这个东西好用，但是在 高版本IE 和 Chrome 里面不好使了

​                ● 我们就得用另一种方式来获取 **事件对象**

​                ● 在每一个事件处理函数的行参位置，默认第一个就是 **事件对象**

```
oDiv.onclick = function (e) {
  // e 就是和 IE 的 window.event 一样的东西
  console.log(e.X轴坐标点信息)
  console.log(e.Y轴坐标点信息)
}
```

想获取事件对象的时候，都用兼容写法

```
oDiv.onclick = function (e) {
  e = e || window.event
  console.log(e.X轴坐标点信息)
  console.log(e.Y轴坐标点信息)
}
```

# **事件对象内的信息--鼠标事件**

​                ● 因为都不一样，所以我们获取的 **事件对象** 里面的属性也不一样

```
 offset  
            offsetX  光标相对于触发事件的元素  左上角(00) 距离左边和距离上面的大小
            offsetY
            
            

        client 
             clientX  相对于浏览器可视化窗口 左上角的距离
             clientY   相对于浏览器可视化窗口 左上角的距离
        page 
            pageX       光标相对于 文档流左上角的距离  = clientX + 滚动条横向移动的距离
            pageY       光标相对于     = clientY + 滚动条纵向移动的距离
```

![](C:\Users\zhang\Desktop\照片\7506b939b34f351bbc26bee7cda2603.png)

## **事件对象内的信息--键盘事件**

​                ● 我们通过 事件对象.keyCode 或者 事件对象.which 来获取

​                ● 为什么要有两个呢，是因为 FireFox2.0 不支持 keycode 所以要用 which

```j&amp;#39;s
// 键盘事件的目的

        // 了解用户到底按下了按个键 

        // 是否按下组合键 

        // 只有window  document 表单事件才会触发 键盘事件
        window.onkeydown = function(e){
            e = e || window.event;
            console.log(e)
            // console.log(e.keyCode||e.which)


            // if((e.keyCode||e.which) == 65 ){
            //     alert('您按下了a键')
            // }
            // else{
            //     alert('您按下了其它键')
            // }
            // 如果点了ctrl 返回true  
            // 点了shift 返回true
            if(e.ctrlKey && e.shiftKey && e.keyCode === 65){
                alert('您按下了ctrl+shift+a');
            }
        }
```

## **常见的键盘码（了解）**

​                ● 8： 删除键（delete）

​                ● 9： 制表符（tab）

​                ● 13： 回车键（ebter）

​                ● 16： 上档键（shift）

​                ● 17： ctrl 键

​                ● 18： alt 键

​                ● 27： 取消键（esc）

​                ● 32： 空格键（space）

​                ● ...

## **组合按键**

​                ● 组合案件最主要的就是 alt / shift / ctrl 三个按键

​                ● 在我点击某一个按键的时候判断一下这三个键有没有按下，有就是组合了，没有就是没有组合

​                ● 事件对象里面也为我们提供了三个属性

​                ○ altKey ：alt 键按下得到 true，否则得到 false

​                ○ shiftKey ：shift 键按下得到 true，否则得到 false

​                ○ ctrlKey ：ctrl 键按下得到 true，否则得到 false

​                ○ metakey：win键按下是true，否则是false

​                ● 我们就可以通过这三个属性来判断是否按下了

```
document.onkeyup = function (e) {
  e = e || window.event
  keyCode = e.keyCode || e.which
  
  if (e.altKey && keyCode === 65) {
    console.log('你同时按下了 alt 和 a')
  }
}
```

## 事件传播

1. 从外往内  捕获
2. 事件目标 触发事件的元素
3. 从内往外  冒泡

* 捕获  冒泡
  * 元素.addEventListener(事件类型，函数，第三参数)
  * 第三函数默认值是false  表示冒泡
  * 写成true表示捕获

### 阻止事件传播

1. w3c浏览器
   * 事件对象.stopPropgation();
2. ie
   * 事件对象.cancelBubble();

### 阻止默认行为

1. 事件对象.preventDefaule( )
2. return false
3. 兼容性
   * 事件对象.returnValue = false

### 事件委托

减少dom元素的获取

可以动态的获取新添加的元素的内容

## this指向

* 是一个关键字 

1. 全局作用域是window  
   * this在全局作用域下就是window

2. 局部作用域
   * 只有函数才有局部作用域
   * 普通函数 => this代表window
   * 对象内函数   =>this  代表对象本身
   * 定时器  =>函数内 this 代表  window
   * 事件处理函数  =>  函数呢 this 绑在谁身上 谁就是this

自执行函数 => this 是window

```js
（function(){console.log()}()

（function(num1,num2){console.log(num1+num2)}(10.20)
```

### 强行改变this指向

1. call()
   * 函数名.call()
   * 对象.函数名.call()
   * call参数
     * 第一个 把this指向谁  从第二个到最后依次给函数形参赋值

2. apply()
   * 函数名.apply()
   * 对象.函数名.apply()
   * apply参数
     * 第一个 把this指向谁  第二个一定是数组，数组中是给形参赋值的实参
3. bind()
   * 函数名.bind()
   * 对象.函数名.bind()
   * bind()参数
     * 第一个 把this指向谁 依次给函数形参赋值

相比较 call apply  函数不会立即执行，而是返回一个新的函数，这个函数已经改变了this指向

### 正则表达式    RegExp

* 简单来说就是对字符串进行查找替换判断是否满足要求
* 前端提交过去的顺序都是满足要求的

1. js内置数据类型   （复杂数据类型）
2. RegExp（Regular   expresstion）

### 字面量创建

var  规则名字  = /规则/

### 内置构造函数

var  规则名字   =   new RegExp（'规则'，'其他参数'）

#### 常用方法

正则.tese(字符串)    =>   true(满足正则要求)    false (不满足正则要求)

#### 基本元素符

* `.`     :   匹配非换行的任意数字，表示字符串中至少有一个非换行的内容
* `\`   :  转义字符
* `\s`  ：匹配空白字符（空格 换行 制表）
* `\S`  ：匹配非空白字符
* `\d`  :  匹配一位   表示字符串中至少包含一位数字（0 - 9）
* `\D`  ：只要这一位不是（0 - 9）任意一个就满足要求
* `\w`  ： 匹配数字母下划线 （a - z 0 - 9  —）中任意一个
* `\W`  ：匹配非数字字母下划线

\d \D \w   匹配的是一位  字符串如果有一位就是true

```js
字面量创建
        var reg = /w/;
        var num ='wjindquyfgayufvbuawuni';
        alert(reg.test(num));
```

```js
内置 
        var reg = new RegExp('\\W');
        var num = 'agabs54agha45 ';
        alert(reg.test(num));
```

### 限定符

* ： 前一个内容重复至少 0 次，也就是可以出现 **0 ～ 正无穷** 次

* + ： 前一个内容重复至少 1 次，也就是可以出现 **1 ～ 正无穷** 次

*  ? ： 前一个内容重复 0 或者 1 次，也就是可以出现 **0 ～ 1** 次

*  {n} ： 前一个内容重复 n 次，也就是必须出现 **n** 次

* {n,} ： 前一个内容至少出现 n 次，也就是出现 **n ～ 正无穷** 次

* {n,m} ： 前一个内容至少出现 n 次至多出现 m 次，也就是出现 **n ～ m** 次

*  限定符是配合元字符使用的

### 边界符

*  ^ ： 表示开头

*  $ ： 表示结尾

* 边界符是限定字符串的开始和结束的

### 特殊符号

*  () ： 限定一组元素

* [] ： 字符集合，表示写在 [] 里面的任意一个都行

*  [^] ： 反字符集合，表示写在 [^] 里面之外的任意一个都行

* ： 范围，比如 a-z 表示从字母 a 到字母 z 都可以

* | ： 或，正则里面的或 a|b 表示字母 a 或者 b 都可以

* 现在我们就可以把若干符号组合在一起使用了

### 标识符(修饰符)

*  i ：ignore的简写  表示忽略大小写
  * 这个 i 是写在正则的最后面的
  * /\w/i
  * 就是在正则匹配的时候不去区分大小写

* g ： global的简写  表示全局匹配
  * 这个 g 是写在正则的最后面的
  *  /\w/g
  * 就是全局匹配字母数字下划线

### exec()方法——捕获

*  作用：exec方法是把字符串中满足条件的内容捕获出来

* 语法： 正则.exec(字符串)

* 返回值： 把字符串中符合正则要求的第一项以及一些其他信息，以数组的形式返回
  * 原始字符串中没有符合正则要求的字符串片段   null

* 原始字符串中有符合正则要求的片段
  * 正则没有 () 也没有 全局标识符g
  *  返回值是一个数组

* 正则有全局标识符 g
  * 返回值是一个数组

*  有 ()     返回的是一个数组

> js弱类型的脚本解释型语言

> 贪婪  能拿多少 拿多少  尽可能的多
>
> 惰性  能拿多少 拿多少  尽可能的少 

1. 惰性
   * 每次都从下标[0] 检索 
   * 如何让它继续往后走   用修饰符  g 可以解决这个问题
   * *?   +?  {n}?    {n,}?   {n,m}?
2. 贪婪
   * 尽可能多的去捕获 
   * 如何解决贪婪    后边+?
   * `*`    +    {n}   {n,}   {n,m}

### test()方法——匹配

* 作用：test方法是用来检测字符串是否符合我们正则的标准

* 语法： 正则.test(字符串)
  *  如果该字符串符合正则的规则, 那么就是 true
  * 如果该字符串不符合正则的规则, 那么就是 false

### 字符串和正则合用的方法

#### search

* 作用：search 是查找字符串中是否有满足正则条件的内容

* 语法： 
  *  字符串.search(正则)
  * 字符串.search(字符串片段)

* 返回值 ： 有的话返回开始索引，没有返回 -1

#### match

* 作用：match 找到字符串中符合正则条件的内容返回

* 语法：
  * 字符串.match(正则)
  * 字符串.match(字符串片段)

* 返回值 ： 
  * 没有标示符 g 的时候，是和 exec 方法一样
  *  有标示符 g 的时候，是返回一个数组，里面是匹配到的每一项

#### replace

*  作用：replace 是将字符串中满足正则条件的字符串替换掉

*  语法： 
  * 字符串.replace('换下字符', '换上字符')
  * 字符串.replace(正则表达式, '换上字符')

* 返回值 ： 替换后的字符串
  *  当你的第一个参数传递字符串的时候, 只能替换一个
  *  当你的第一个参数传递正则表达式的时候, 只能替换一个
  * 但是如果你的正则表达式有全局标识符 g, 那么有多少替换多少

## es6

* es6在之前的基础上 有新增的内容
  1. 定义变量
  2. 箭头函数
  3. 解构赋值
  4. 模板字符串
  5. 对象简写方式
  6. 展开符号  ...
  7. 数参数默认值
  8. 模块化

>  es2015
>
> ie及低版本不兼容

#### let声明变量

* 用let声明的变量也叫变量

#### const声明变量

* 用const声明的变零叫产量

#### let和const的区别

* 声明时赋值
  * 用let声明变量的时候可以不赋值
  *  用const声明变量的时候 **必须** 赋值

* 值的修改
  * 用let声明的变量的是可以任意修改的
  * 用const声明的变量的是不能修改的，也就是说一经声明就不允许修改
  * 但是用const声明的对象，对象里面的属性的值是可以修改的

#### let/const和var的区别

* 预解析
  *  var 会进行预解析, 可以先使用后定义
  *  let/const 不会进行预解析, 必须先定义后使用

* 变量重名
  * var 定义的变量可以重名, 只是第二个没有意义
  * let/const 不允许在同一个作用域下, 定义重名变量

* 块级作用域
  * var 没有块级作用域
  *  let/const 有块级作用域
  *  块级作用域: 任何一个可以书写代码段的 {} 都会限制变量的使用范围
    *  if() {}
    *  for () {}

### ES6箭头函数

* 语法：() => {}
  * ()中书写形参
  * =>
  * {}中书写代码片段
* 箭头函数只能用来定义函数表达式，所以箭头函数也是匿名函数
* 0
* 声明式的函数不能写成箭头函数

##### 特点

1. 箭头函数没有this  ，this就是外部作用域的this 。 绑定在谁身上谁就是this .
2. 箭头函数没有 arguments  没有arguments.callee
3. 形参只有一个可以省略小括号 ， 如果形参是一个或者两个及以上小括号必须写
4. 当执行语句只有一行可以省略大括号 ， 自动把折行代码的值返回 。

### 解构赋值

1. 解构数组

   1. 多维数组
   2. 一维数组

   * 语法 ： var [变量1,变量2, 变量3，....] = 数组
   * 按照索引的顺序依次给变量赋值

2. 解构对象

   * 语法  ：var {键名1, 键名2, 键名3, ... } = 对象

   1. 别名
      *  解构的时候可以给变量起一个别名
      * 语法：var { 键名: 别名, 键名2: 别名 } = 对象

#### 函数参数默认值

* 默认值一定要写在形参列表的末尾
* 防止形参和实参不对应

#### 模板字符串

* 使用 反引号(``) 来进行字符串的定义
* 当只有一行 return 不用写 自动返回

...两种功能

1. 展开
   *  数组 对象  Set   Map 意义去掉外边包裹的部分
2. 合并
   * 解构的时候  形参

### map 和 set

* ES6 新增的两种数据结构

* 共同特点：不接受重复值

### set

* 语法: var 变量 = new Set([ 数据1, 数据2, 数据3, ... ])

1. size（）
   * 去重以后的个数
   * 语法: 数据结构.size
2. add() 方法
   * 语法: 数据结构.add(数据)
   * 作用: 向该数据结构内添加数据
3. has() 方法
   * 语法: 数据结构.has(数据)
   * 返回值: 一个布尔值
     *  true, 表示该数据结构内有该数据
     * false, 表示该数据结构内没有该数据
4.  delete() 方法
   * 语法: 数据结构.delete(数据)
   * 作用: 删除该数据结构内的某一个数据
5. clear() 方法
   * 语法: 数据结构.clear()
   * 作用: 清除该数据结构内所有数据
6.  forEach() 方法
   * 语法: 数据结构.forEach(function (item,index,origin) {})
   * value   = >   元素    key  =>  索引    origin   =>  原数据

### map

* 是一个类似于对象的数据结构, 但是他的 key 可以是任何数据类型
* 语法: var 变量 = new Map([ [ key, value ], [ key, value ] ])

1. size 属性
   * 语法: 数据结构.size
   *  得到: 该数据结构内有多少个数据

2. set() 方法
   * 语法: 数据结构.set(key, value)
   * 作用: 向该数据结构内添加数据

3. get() 方法
   *  语法: 数据结构.get(key)
   * 如果键是对象或者数组，那么不能根据他进行取值，
   * 如果键是字符串基本数据类型 
     * 返回值: 数据结构内该 key 对应的 value

4. has() 方法
   *  语法: 数据结构.has(key)
   *  返回值: 一个布尔值
     *  true, 该数据结构内有该数据
     *  false, 该数据结构内没有该数据

5. delete() 方法
   * 语法: 数据结构.delete(key)
   * 作用: 删除该数据结构内的某一个数据

6. clear() 方法
   * 语法: 数据结构.clear()
   *  作用: 清除该数据结构内所有数据

7. .forEach() 方法
   * 语法: 数据结构.forEach(function (value, key, origin) {})
   * value   = >   值    key  =>  键    origin   =>  源map

#### 对象简写语法

* 当对象和键的值一模一样 ， 并且这个值还是一个变量，  这个时候省略一个不写
* 当对象内的key  对应的是一个函数 并且不是箭头函数，可以省略function  和  冒号不写

## es6模块化开发

* 把一个完整功能，拆开成一个一个独立功能（模块）=> js文件就是一个独立模块
* 根据业务需求，来进行模块整合
* 当模块开发的时候
  * 把多个逻辑书写在多个js文件中
  * 某一个文件都是独立文件
* 导入 / 导出
  * script 标签必须要有一个 type = module
  * 页面必须在服务器上打开（借助 live   server)
* 必须采用服务器打开方式的
  1. cookie
  2. 模块化开发
* 导出
  * 语法1 export  default   
    * 一个文件只能导出一个default 数据
  * 语法2  export  定义变量  =  值
    * 一个文件只能导出多个新定义的变量
* 导入
  * 语法1   import   变量  from   ' 文件 '
  * 语法2  import   { 导出的内容 }  from  ' 文件 '

### 运动函数

* 对运动过程的封装