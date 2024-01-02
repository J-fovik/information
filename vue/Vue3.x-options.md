

typora-copy-images-to: C:\Users\Administrator\Pictures\Camera Roll



# 一、Vue3简介

## 1、概述

作者：尤雨溪

官网：https://cn.vuejs.org

![image-20211013151546366](https://i.loli.net/2021/10/13/I8gfJyaxQmGjztR.png)

Vue.js是一套构建用户界面的**渐进式**框架。**声明式渲染和组件系统是Vue的核心库所包含内容。**

- **构建用户界面:**  通过数据渲染成页面模板,展示给前端用户

- **渐进式**：从`vue` 模板-指令-组件-路由- `vuex`等由简单=>复杂开发学习应用过程
- **框架**：半成品的应用，不断在维护更新的开源框架（之前学的bootstrap,jQuery也是一个框架）

![Vue特点](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/4f9e45db623e2c0f0dd1a45e985c36b83825ce6c.png?sign=cead88c04c73c6d314f093b1ef504d42&t=5f50707e)

![Vue与react在github上](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/229fe902962263af21443024e14a78b54ea5c3d1.png?sign=976600758fc879cea5db0a54774e1bda&t=5f5070ea)



- **声明**式渲染：（如同js基础一样，要使用变量则必须先声明变量，这种称之为声明式, 数据驱动视图渲染）

Vue.js的核心是一个允许采用简洁的模板语法来声明式的将数据渲染进DOM的系统。也就是咱们后边数据驱动页面渲染。

- **响应性**：Vue 会自动跟踪 JavaScript 状态变化并在改变发生时响应式地更新 DOM。

- **组件化**应用构建

组件系统是Vue的另一个重要概念，因为它是一种抽象的允许我们使用**小型、独立**和通常**可复用**的“小积木”构建大型应用。几乎任意类型的应用界面都可以抽象为一个组件树。

![组件](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/279e26e948d53d33a2a05e10e7c29aa736fe80a1.png?sign=9f63925b42a763bb945ad25cda41928f&t=5f5073bf)



## 2、回顾js-dom渲染数据

特点: 需要开发者频繁操作dom data数据;

```js
<body>
    <div id="test"></div>
    <p>
        <button onclick="editmsgFn()">修改msg</button>
    </p>
</body>
<script>
    // 数据 DATA
    let msg = 'hello world'
    // 视图 DOM
    let oDom = document.querySelector('#test')
    // 渲染
    oDom.innerHTML = msg


    // 修改msg 数据
    const editmsgFn = () => {
        msg = '嘻嘻嘻';
        // 渲染
        oDom.innerHTML = msg
    }
</script>
```



# 二、Vue入门

## 1、vue2-API 

```js
// vue2提供的是一个vue的类, 需要实例化出一个vue的实例作为应用
// 依然需要传入 options来进行配置
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<div id="app">
  {{ message }}
</div>

 const app = new Vue({
   // Vue2中提倡我们整个网站中只有一个实例, 所以这个时候data是不是函数已经无所谓
   data: {
       msg: 'hello Vue 2',
       count: 10,
     }
   })
 app.$mount('#app')
```



## 2、vue3-API 风格 2种

> -  Vue 的组件可以按两种不同的风格书写：**选项式 API** 和**组合式 API**。 

- ####  选项式 API (Options API)

使用选项式 API，我们可以用包含多个选项的对象来描述组件的逻辑，例如 `data`、`methods` 和 `mounted`。选项所定义的属性都会暴露在函数内部的 `this` 上，它会指向当前的组件实例。 

> 对应的data数据只能显示在对应的vue实例绑定的DOM 容器中 

- 代码片段如下：


~~~html
<body>
  <div id="app">
    <div id="test">{{ msg }}</div>
    <div id="count" @click='addCount'>{{ count }}</div>
  </div>
</body>
<!-- <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script> -->
<script type="module">
   import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'
  // vue3提供了一个Vue大对象, 在其中有很多api方法
  // createApp 创建"应用"的方法, 每一个应用都可以实现/体现完整的vue能力
  const { createApp } = Vue
  // createApp内部可以传入options, options来配置应用的能力(这就是选项式, 在options中配置各种各样的内容, 来使用各种各样的能力)
  // Vue采用MVVM的形式, 这个app体现了VM的能力
  const app = createApp({
    // 利用data函数来声明数据, 函数的返回值就是声明的数据
    data () {
      // 写成函数返回的目的: 为了防止有不同的应用采用相同的数据, 导致数据混乱, 写成函数返回, 就可以保证每个应用都会拥有自己独立的数据
      return {
        msg: 'Hello Vue!',
        count: 10,
      }
    },
    methods:{
      addCount(){
          this.count++
      }  	
    }
  })
  // 创建好 app后, 需要让app起作用, 需要将其"装载"在页面的某一个范围内(划地盘)
  app.mount('#app')

</script>
</html>
~~~



- #### 组合式 API (Composition API)

通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [``](https://cn.vuejs.org/api/sfc-script-setup.html) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`` 中的导入和顶层变量/函数都能够在模板中直接使用。 

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script> -->
    <script src="./node_modules/vue/dist/vue.global.js"></script>
</head>

<body>
    <div id="app">
        <p>{{ message }}</p>
        <p>{{count}}</p>
        <button @click="count--">-</button>
        <!-- 为button绑定click事件,处理程序为methods中的handleIncrement方法 -->
        <button @click="handleIncrement">+</button>
    </div>
</body>

</html>
<script >
    const { createApp, ref } = Vue;
    createApp({
        // 在选项式api中可以利用setup函数来使用组合式api
        setup() {
            // 利用ref来声明(响应式的: 有数据劫持)状态
            const message = ref('hello world')
            const count = ref(1)
            const handleIncrement = () => count.value++
            // setup中声明的方法和状态, 都需要返回出去, 才能在模板中使用
            return { count, message, handleIncrement }
        }
    }).mount('#app')

</script>
```



## 3、数据渲染

新旧虚拟DOM做比较,  判断那个地方更新,然后实现局部的数据更新

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <div>
            <p>count: {{ count }}</p>
        </div>
        <p>
            <button @click="count++">+</button>
        </p>
        <br />
        <p>msg: {{ msg }}</p>
    </div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
    const { createApp } = Vue
    // 1. app 会查找对应的模板: mount
    // 2. 编译模板为 虚拟dom(js结构)  virtual dom
    const vdom = {
        tag: 'div',
        attrs: {
            id: 'app'
        },
        children: [
            { tag: 'p', children: `count: {{ count }}` },
            { tag: 'br' },
            { tag: 'p', children: `msg: {{ msg }}` },
        ]
    }
    // 3. 根据数据修改后,生成新的虚拟dom
    const vdom2 = {
        tag: 'div',
        attrs: {
            id: 'app'
        },
        children: [
            { tag: 'p', children: `count: 1` },
            { tag: 'br' },
            { tag: 'p', children: `msg: Hello World` },
        ]
    }
    // 4. 对比 vdom1 和 vdom2, 查找到需要更新的地方, 判断出最优的更新解然后执行.
    const app = createApp({
        data() {
            return {
                msg: 'Hello World',
                count: 1
            }
        }
    }).mount('#app')

    console.log(app);
</script>

</html>
```



## 4、Vue的开发模式

- M-V-VM
  - M：（model）普通的javascript数据对象（其实就是一个对象，对象里放了数据）
  - V：（view）前端展示页面（可以理解成html内容）
  - VM：（ViewModel）用于**双向绑定数据**与页面，对于我们的课程来说，就是vue的实例vm

MVVM 模式中的 ViewModel，它采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然。**这种模式下，页面输入改变数据，数据改变影响页面数据展示与渲染**

> vue使用MVVM响应式编程模型，避免直接操作（真实）DOM , 降低DOM操作的复杂性。（虚拟DOM）

![MVVM](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/daf286bb718f7afb09ddea7205c58a18d56797f5.png?sign=9597d9d44c7ac8ace46d8ab7c0500407&t=5f560d40)





## 4、vue devtools工具安装

1. 通过chrome中的谷歌插件商店安装Vue Devtools工具，此工具帮助我们进行vue数据调试所用，一定要安装。Vue工具在谷歌商店的地址是：https://chrome.google.com/webstore?utm_source=chrome-ntp-icon

> 请注意：打开chrome应用商店，**需要科学上网**才能访问到，至于怎么科学上网请各位自行解决。

![Vue工具谷歌商店](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/dc840deb63c247fb5b7fac6f162f3fece10832ae.png?sign=180564fbbac92d11b6e89e4e9d8df208&t=5f561a39)

安装好后打开Chrome的`开发者工具（F12或Ctrl+Shift+I）`即可使用：

![谷歌浏览器使用Vue工具](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/7ea8955be2b91f4c8530fdf7696fa2f5508b3a35.png?sign=2773c0e615128ca86b96c79bca31eaf4&t=5f561a6a)

2. **补充：如果自己解决不了科学上网问题，但是又需要用Vue开发工具那该怎么办？**

> 如果实在解决不了科学上网难题，Vue官方也提供了插件源码允许我们自己编译/构建Google Chrom插件，步骤如下（构建插件流程稍微麻烦一些<**不要求掌握如何构建**>，此处已为同学们构建好，可以直接使用）。
>
> ![Vue调试工具](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/6f094719844e04c1aa853b36c18b1e18c1ee6c2d.png?sign=55a5622a22b937b7b1b419ca67dafd6b&t=5f38fa24)

3. 安装chrome  Vue Devtools调试工具详细教程：

https://blog.csdn.net/li22356/article/details/113092495?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control



## 5、Vue模板语法

### 5.1、插值表达式 {{数据}} => Mustache 语法

**插值表达式：**是vue框架提供的一种在HTML模板中绑定数据的方式，也叫 “Mustache”语法, 使用`{{变量名}}`方式绑定Vue实例中data中的数据变量，会将绑定的数据实时的在视图中显示出来。

插值表达式的写法支持使用：

- 变量名
- **部分**JavaScript表达式
  - 注：`{{  }}`括起来的区域，就是一个就是js语法区域，在里面可以写部份的js语法。不能写 var a = 10;分支语句;循环语句
- 三元运算符
- 方法调用（方法必须需要先声明）
- ...

~~~html
<body>
    <div id="app">
        <p>msg: {{ msg }}</p>
        <p>count: {{ count }}</p>
        <p>fn: {{ fn }}</p>
        <p>bool: {{ bool }}</p>
        <p>udf: {{ udf }}</p>
        <p>null: {{ null }}</p>
        <p>arr: {{ arr }}</p>
        <p>obj: {{ person }}</p>
        <hr />
        <!-- 三目运算 -->
        <p>sum: {{ count * 2 + 3 }}</p>
        <p>random: {{ randomNum() }}</p>
        <!-- if/for等逻辑语句不能在表达式中运行 -->
        <p>是否成年: {{ age >= 18 ? '成年' : '未成年' }}</p>
        <p>昵称: {{ nickname || '新用户asydiasbd' }}</p>
        <p>昵称: {{ nickname ?? '新用户asydiasbd' }}</p>
        <p>昵称: {{ nickname && '新用户asydiasbd' }}</p>
        <!-- 使用函数 -->
        <h5>{{title.substr(0,6)}}</h5>
    </div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
    const { createApp } = Vue;
    // 数据类型: 基本(string, number, boolean, undefined, null, Symbol) 引用(function, array, object)
    createApp({
        data() {
            return {
                msg: 'Hello World',
                count: 1,
                bool: true,
                udf: undefined,
                null: null,
                fn: () => { },
                arr: [1, 2, 3],
                person: { name: 'Allen', age: 18 },
                age: 18,
                nickname: undefined,
                title: "我是一个标题，你们看到没有",
                name: "张三",
            }
        },
        methods: {
            randomNum() {
                return Math.random()
            }
        }
    }).mount('#app')
</script>

</html>
~~~



### 5.2、指令

**问1：什么是指令？**

- [x] 指令的本质就是标签中的vue**自定义属性**
- [x] 指令格式以“v-”开始，例如：v-cloak，v-text、v-html等

**指令的含义：在vue的html中，以`v-`开头的自定义属性就是指令。**

详见官网对指令的说明：https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4

**问2：指令有什么作用？**

正如插值表达式的效果，插值表达式只能用于标签之间的数据输出；在标签属性上，插值表达式无用武之地，但是有需要在属性中使用可变数据的情况，此时指令就能帮助我们解决这个问题。

语法糖：复杂操作的简化形式

当表达式的值改变时，将其产生的连带影响，响应式地作用于页面（DOM）。（简化操作）

**小试牛刀**：v-text指令与v-html指令【相当于innertHTML和innerText】

> **官网**
>
> v-text：https://cn.vuejs.org/v2/api/#v-text
>
> v-html：https://cn.vuejs.org/v2/api/#v-html





# 三、常用指令

## 1、v-cloak

**作用：**解决浏览器在加载页面时因存在时间差而产生的`闪动`问题

**原理：**先隐藏元素挂载位置，处理好渲染后再显示最终的结果

> 注意：**通过属性选择器设置该元素的  `display: none;`

**文档地址：**https://cn.vuejs.org/v2/api/#v-cloak

**示例：**

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script> -->
    <script src="./node_modules/vue/dist/vue.global.js"></script>
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>
<body>
    <!--
	 01:将该指令加在某一个标签上,只有该标签起作用
     02:如果后期有多个元素需要解决闪动问题，可以将`v-cloak`写在根元素上（id="app"顶级的div上）。	
	-->
    <div id="app" v-cloak>
        <p>{{ message }}</p>
        <p>{{count}}</p>
        <button @click="count--">-</button>
        <!-- 为button绑定click事件,处理程序为methods中的handleIncrement方法 -->
        <button @click="handleIncrement">+</button>
    </div>
</body>

</html>
<script type="module">
    const { createApp } = Vue
    setTimeout(() => {
        const app = createApp({
            data() {
                return {
                    message: 'hello world',
                    count: 1
                }
            },
            methods: {
                handleIncrement() {
                    this.count++
                }
            }
        })

        app.mount('#app')
    }, 1000)
</script>
~~~



## 2、v-text 数据绑定

- v-text	填充纯文本, 可以将文本数据渲染在绑定指令的dom内部(innerText) 
  - 相当于之前原生的innerText
  - 相比插值表达式更加简洁
  - 不存在插值表达式的闪动问题

~~~html
<div id='app'>
    <!-- 插值表达式此时与v-text是等效的 -->
	<span>{{msg1}}</span>
    <span v-text="msg1"></span>
    
     <!-- 插值表达式此时与v-text是不等效的 -->
    <span>{{msg2}}</span>
	<span v-text="msg2"></span>
</div>
 <!-- <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script> -->
<script>
  new Vue({
    el: '#app',
    data: {
      msg1: '今晚da老虎',
      msg2:'<a href="http://www.baidu.com/">百度一下</a>'
    }
  })
</script>
~~~

## 3、v-html 数据绑定

- v-html 	填充HTML片段, 可以将html格式的字符串转译为真正的html标签来渲染(利用文章数据/商品详情数据很可能会是html格式的字符串数据) ,往往这种标签结构的数据都是前端通过富文本编辑器提交给后端的.
  - 相当于原生innerHTML
  - 存在安全问题
  - 本网站内部数据可以使用，来自第三方的数据不可使用
    - 只有一个场景会使用：后台会用，比如有一个企业站，会发不企业的动态的新闻，这个时候会使用富文本编辑器，由于内容是自己人加的，所以可以放心使用。  自己攻击自己（自攻）

~~~html
 <!-- <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script> -->
<div id='app'>
     <!-- 插值表达式此时与v-html是等效的 -->
    <div>{{html1}}</div>
    <div v-html="html1"></div>
    <!--有xss攻击风险，获取当前本网站的cookie-->
    <div v-html="html2"></div>
</div>
<script>
    const { createApp } = Vue;
    const app = createApp({
            data() {
                return {
                    html1:'<a href="http://www.baidu.com/">百度一下</a>',//跳转到其他网站
      				html2:'<span onclick="console.log(document.cookie)">测试</span>' 
                }
            },
            methods: {
                handleIncrement() {
                    this.count++
                }
            }
        })

    app.mount('#app')
</script>
~~~

> **有些时候我们不想指令中的表达式进行运行，只需要给表达式加个引号**。例如：

~~~html
<div v-html='"msg"'></div>
<div v-html="'msg'"></div>
~~~

针对后续想让指令属性值不解析的操作都可以这么去做。



## 4、v-once（了解）

**作用：**只渲染**元素或组件**一次**，之后元素或组件将失去**响应式（数据层面）功能（对于数据的一锤子买卖）

> - 使用vm.message = 'hello world' ,页面不会重新渲染

**示例：**

~~~html
<div id="app">
	<h3>{{message}}</h3>
	<!-- 动态修改message值，此绑定将不会发生改变 -->
	<div v-once>{{message}}</div>
</div>
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script type='javascript'>
     const { createApp } = Vue;
    const app = createApp({
            data() {
                return {
                  message:"只有一次哦"
                }
            }
        })
    app.mount('#app')
</script>
~~~

## 5、v-pre（了解）

元素内具有 `v-pre`，所有 Vue 模板语法都会被保留并按原样渲染。最常见的用例就是显示原始双大括号标签及内

```html
<body>
  <div id="app">
    <!-- 解析输出 -->
    <div v-html="msg"></div>
    <!-- 转义输出 -->
    <div v-text="msg"></div>
    <!-- 文本插值 -->
    <div>{{ msg }}</div>
    <!-- v-pre 含有该指令的标签内部的语法不会被vue解析 -->
    <div v-pre>{{ msg }}</div>
  </div>
</body>
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
  const { createApp } = Vue;
  const app = createApp({
    data () {
      return {
        msg: '<h1>hello vue</h1>'
      }
    }
  })
  app.mount('#app')
</script>
</html>

```



## 6、v-bind（重点）

**作用：**动态地绑定一个或多个`attribute`【实现了可以允许我们在html内置的属性值中使用变量】

场景：复用某个数据的时候会使用。例如：飞猪官网

~~~html
<!-- v-bind：给非指令的属性使用变量 -->
<a v-bind:href="url" v-bind:target="target">{{alt}}</a>

<!-- v-bind的简写形式，实际使用这样的写法 -->
<a :href="url" :target="target">{{alt}}</a>
~~~

**示例代码**:

~~~html
<body>
    <div id="app">
        <!--v-bind  动态设置标签属性  -->
        <p v-bind:title="tit">北科千锋前端培训扛把子</p>
        <!-- 动态设置样式 -->
        <p v-bind:class="active">北科千锋前端培训扛把子</p>
        <!-- v-bind:属性名   缩写:属性名-->
        <p :class="active">北科千锋前端培训扛把子</p>
        <img :src ='imgUrl'/>
        <!-- 将对象身上所有的属性都为dom绑定上, 就直接利用v-bind进行全量绑定即可, 不需要传递参数 -->
        <img v-bind="imgInfo"/>
    </div>
</body>
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
    Vue.createApp({
        data() {
            return {
                imgurl: 'http://www.mobiletrain.org/page/images/%E9%83%AD%E7%BF%94.png',
                width: 200,
                disabled: false,
                imgInfo: {
                    width: 200,
                    src: 'http://www.mobiletrain.org/page/images/%E9%83%AD%E7%BF%94.png'
                }
            }
        },
    }).mount('#app')
</script>
~~~





## 7、v-on（重点）

### 7.1、基本使用

**作用：**绑定事件监听器（事件绑定）

**语法:** v-on:事件名= "事件的逻辑处理",    注意: 事件的处理逻辑可以是简单的逻辑运算比如赋值, 也可以是一个处理函数 

**示例代码：**

~~~html
<!-- 直接执行操作 -->

<!-- 常规写法: 事件的逻辑处理为运算时的情况 -->
<button v-on:click="num++"></button>

<!-- 简写 -->
<button @click="num++"></button>

<!--v-on 不允许使用内置函数的，必须要调用自己定义的函数，否则报错，你可以在自定义的函数内使用内置函数-->
<button @click="alert(123)">alert</button>

<!-- 事件处理函数调用：直接写函数名,方法默认会接收到事件对象, vue已经对事件对象做出了封装和兼容处理等优化操作  -->
<button @click="say1">点击事件不传参</button>

<!-- 事件处理函数调用：常规调用 -->
<button @click="say2(100,$event)">点击事件传参数</button>

<!--事件的逻辑处理也可以直接写一个函数,但是不建议这么写,因为当重新渲染dom,事件函数还会被重新创建,浪费性能 -->
<p>
  <button v-for="(item,index) in numArr" :key="index" @click="()=>{count+=item}">+   {{item}}</button>
</p>
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
`<script>
   Vue.createApp({
        data() {
            return {
             	num：10
            }
        },
       methods: {// 方法集合
            say1: function (event) {
                console.log(event)
            },
            say2: function (num, event) {
                console.log(event)
                console.log('你点了' + num);
            }
        }
    }).mount('#app')
</script>
~~~

在不传递自定义参数的时候，上述两种用法均可以使用；但是如果需要传递自定义参数的话，则需要使用第2种方式。

> 事件对象的传递与接收注意点
>
> - 如果事件直接使用函数名并且不写小括号，那么**默认**会将事件对象作为唯一参数进行传递，可以在定义函数的位置直接定义一个形参，并且在函数内可以使用该形参
> - 如果使用常规的自定义函数调用（只要写了小括号），那么如果需要使用**事件对象则必须作为参数进行传递**，且事件对象的名称必须是“$event”



### 7.2、事件修饰符

含义：用来处理事件的特定行为（也是vue提供一些语法糖）

使用示例：

~~~html
<!-- 事件执行一次 -->
<button @click.once="doThis">抽奖</button> 
<!-- self 只有触发事件源本身时才触发，也可用于阻止事件冒泡行为场景-->
<!-- 点击子元素不会触发父元素的点击事件，因为父元素的点击事件只有点击本身才触发 -->  
<div @click.self ='fun1'>
    <p @click='fun2'>
       子元素 
    </p>
</div>
<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>
<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>
<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 即 事件捕获 -->
<div @click.capture="doThis">...</div>
<!-- 点击事件最多被触发一次 -->
<a @click.once="doThis"></a>
<!--  串联修饰符（连贯操作） -->
<button @click.stop.prevent="doThis"></button>
~~~

更多事件修饰符请参考官方文档：https://cn.vuejs.org/v2/api/#v-on



### 7.3、按键修饰符

按键修饰符：**注意必须是键盘事件**

- `.enter`
- `.tab`
- `.delete` (捕获“Delete”和“Backspace”两个按键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

> 在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符。

~~~html
<!-- 只有在 `key` 是 `Enter` 回车键的时候调用 -->
<input @keyup.enter="submit">

<!-- 只有在 `key` 是 `Delete` 回车键的时候调用 -->
<input v-on:keyup.delete="handle">
~~~

更多按键修饰符请参考官方文档：[https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6](https://cn.vuejs.org/v2/guide/events.html#按键修饰符)

## 8、循环渲染指令

**作用：**根据一组**数组**或对象的选项列表进行渲染。

**指令：**v-for

- 数组遍历使用示例：

~~~html
<body>
    <div id="app">
        <!-- 遍历数组 -->
        <!-- 不使用key -->
        <ul>
            <li v-for="(item,index) in list">
                {{index}}--{{item.name}}
            </li>
        </ul>
        <!-- 使用key -->
        <ul>
            <li v-for="(item,index) in list" :key="item.id">
                {{index}}--{{item.name}}
            </li>
        </ul>
        <!-- 遍历对象 -->
        <ul>
            <li v-for="(value, key, index) in myObject">
                {{ index }}. {{ key }}: {{ value }}
            </li>
        </ul>

        <!-- 循环字符串的时候, 取出的是字符串中的每一个字符 -->
        <span v-for="(str, index) in message" :style="{ color: index % 2 === 0 ? 'red' : 'green' }">{{str}}</span>

        <!-- v-for 嵌套使用 -->
        <ul>
            <li v-for="(item,index) in list1" :key="item.id">
                <div>
                    <p>{{item.name}}</p>
                    <span v-for="child in item.girlfriend" :key="index">{{child}}</span>
                </div>
            </li>
        </ul>
    </div>
</body>
<script>
    Vue.createApp({
        data() {
            return {
                message: 'Hello',
                list: [
                    { id: 1, name: '张三', age: 16 },
                    { id: 2, name: '李四', age: 8 },
                    { id: 3, name: '王五', age: 1.5 },
                ],
                userinfo: {
                    name: "李云龙",
                    age: 20,
                    sex: '男'
                },
                list1: [
                    { id: 1, name: '张三', girlfriend: ['小红', '小丽'] },
                    { id: 2, name: '李四', girlfriend: ['小白', '小黑'] },
                ]
            }
        },
    }).mount('#app')
</script>
~~~

> 细节：key的作用，提高性能，不影响显示效果（`如果没有id，可以考虑使用索引替代或单元项本身`），切记`key`的值不能重复，只要遵循不重复的原则即可，值是什么无所谓。
>
>  key的作用, 就是帮助vue在数据变化而生成新的虚拟dom之后, 做虚拟dom对比的时候判断的依据. 判断节点没变化就复用, 如果变化了,就根据虚拟dom,删除旧的真实的dom节点,然后创建新的真实的dom 节点.  

**演示案例 - 设置key的必要性** 

```html
<!--模板部分-->
<body>
    <div id="app">
        <!--不设置key的话，新增后选中的复选框发生变化所以必须指key-->
        <ul>
            <li v-for="(item,index) in listName" :key='item.id'>
                <input type="checkbox" />
                {{index}}--{{item.name}}
            </li><input type="text" v-model='a' />
        </ul>
        <button @click='add'>add</button>
    </div>
</body>
<script>
    Vue.createApp({
        data() {
            return {
                a: '',
                "listName": [
                    { "id": 1, name: '德玛西亚' },
                    { "id": 2, name: '德邦' },
                    { "id": 3, name: '剑圣' }
                ]
            }
        },
        methods: {
            add() {
                this.listName.unshift({ id: this.listName.length + 1, name: this.a })
            }
        }
    }).mount('#app')
</script>
```



## 9、分支渲染指令

**作用：**根据表达式的布尔值(true/false)进行判断是否**渲染**/**显示**该元素

- v-if
- v-else
- v-else-if

> 上述三个指令是分支中最常见的。根据需求，v-if可以单独使用，也可以配合v-else一起使用，也可以配合v-else-if和v-else一起使用。

使用示例：

~~~html
<!-- 模板部分 -->
<body>
    <div id="app">
        <!-- 模板部分 -->
        <div v-if="score >= 90">
            优秀
        </div>
        <div v-else-if="score >= 80 && score < 90">
            良好
        </div>
        <div v-else-if="score >= 70 && score < 80">
            一般
        </div>
        <div v-else>
            不及格
        </div>
        <hr />
        <div v-if="flag">控制元素是否渲染</div>
        <!-- 当处理多个元素的渲染时,可以使用一个template 模板标签包裹,但是template 不会最终渲染,
        只起到包裹元素的作用
		-->
        <button @click="flag=!flag">是否渲染</button>
    </div>
</body>
<script>
    Vue.createApp({
        data() {
            return {
                score: 88,
                flag: false
            }
        },
    }).mount('#app')
</script>
~~~

- v-show

   v-show是根据表达式之真假值，切换元素的 `display` CSS属性（是根据表达式的布尔值来判断是否**显示**该元  素）。

```html
<!-- v-show -->
<p v-show ='isflag'>是否显示内容</p>
<p v-if ='isflag'>是否显示内容</p>
<button @click='isflag=!isflag'>切换</button>

<!-- v-show 不支持在template标签上使用 -->
 <template v-show="isflag">
            <h1>Title</h1>
            <p>Paragraph 1</p>
            <p>Paragraph 2</p>
        </template>
<!-- JavaScript部分 -->
<script>
    Vue.createApp({
        data() {
            return {
                isflag: false
            }
        },
    }).mount('#app')
</script>
```



> 思考：v-if系列与v-show的区别是什么？
>
> v-if：控制元素是否渲染
>
> v-show：控制元素是否显示（**已经渲染**，display:none;）

> v-if系列指令、v-show指令可以与v-for指令结合起来使用（循环+分支）。例如

~~~html
<ul>
    <li v-for='(v,k,i) in obj' v-show='v==25'>{{v}}</li>
</ul>
~~~

**面试题：v-for与v-if谁的优先级高，能否一起使用？**

- 面试题 v-for 与v-if 比较 优先级谁高?  

  解释:   

  在vue3中,  当它们同时存在于一个节点上时，`v-if` 比 `v-for` 的优先级更高 

  在vue2中,  当它们同时存在于一个节点上时，`v-for 比 `v-if` 的优先级更高 

  ```vue
   <ul>
        <!--第一种情况报错,由于v- for 优先级高于v-if 在v-if 的作用域中获取不到item-->	
        <!-- <li v-for="(item,index) in list" :key="item.id" v-if="item.age>8">
       {{index}}--{{item.name}}
       </li> -->
        <!--解决方法1: 使用v-show 代替v-if-->	
       <!-- <li v-for="(item,index) in list" :key="item.id" v-show="item.age>8">
       {{index}}--{{item.name}}
       </li> -->
         <!--解决方法2: 使用v-show 代替v-if-->	
       <template v-for="(item,index) in list">
           <li v-if="item.age>8">
           {{ item.name }}
         </li>
       </template>
   </ul>
  ```

  



 **面试题：v-if 和 v-show的 使用区别？**

答： 如果元素频繁要切换显示隐藏 则使用v-show 更加合适，  

 	    如果元素被创建，可能不会进行状态得切换，使用v-if 

## 9、综合案例：简易购物车

**案例效果**

![简易购物车案例效果](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/306aab7bc3f508529eacd7e352283a5670a36873.gif?sign=c1f7cd1b184d05b5a750c70759eb9a0a&t=5f5748db)

> 细节：
>
> - 展示基本的商品信息
> - 计算每个商品的小计
> - 商品数量的加、减操作
>   - +：增加商品数量，同时更新小计
>   - -：减少商品数量，同时更新小计，如果本身为“1”，再点-号则需要移除商品

> 如果需要在Vue实例中访问自身data属性中的数据，可以使用以下方式：
>
> - **this.xxxxx**
> - this.$data.xxxxx
> - this._data.xxxxx

**实例代码**

~~~html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <title>综合案例：简易购物车</title>
    <meta name="viewport"
        content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
    <meta name="description" content="" />
    <meta name="keywords" content="" />
    <link href="" rel="stylesheet" />
    <script src="./js/vue.js"></script>
</head>

<body>
    <div id="app">
        <ul>
            <li v-for="(item,index) in cartData" :key="item.id">
                商品id：{{item.id}}&emsp;&emsp;
                商品名称：{{item.name}}&emsp;&emsp;
                商品单价：{{item.price}}&emsp;&emsp;
                购买数量：<button @click="jian(item,index)">-</button>{{item.num}}<button
                    @click="item.num++">+</button>&emsp;&emsp;
                商品小计：{{item.price * item.num}}
            </li>
        </ul>
    </div>
    <script>
        Vue.createApp({
            data: {
                cartData: [
                    {
                        id: 1,
                        name: '小米',
                        price: 100,
                        num: 1
                    },
                    {
                        id: 2,
                        name: '华为',
                        price: 200,
                        num: 1
                    },
                    {
                        id: 3,
                        name: '联想',
                        price: 300,
                        num: 1
                    }
                ]
            },
            // 处理程序
            methods: {
                // 减法操作处理
                jian(item, index) {
                    if (item.num === 1) {
                        if (confirm("确认不买一件吗？")) {
                            this.cartData.splice(index, 1)
                        }
                    } else {
                        // 够减
                        item.num--
                    }
                },
            }
        }).mount('#app')
    </script>
</body>
</html>
~~~

> `&emsp;`表示`tab`，一个顶四个`&nbsp;`



## 10、样式绑定

### 10.1、class样式绑定

- 对象语法（`用于控制开关切换`）【高频】

~~~html
<style>
/* CSS片段 */
.on {
	color: red;
}
.big{
	font-size: 40px;
}
</style>

<!-- HTML片段 -->
<div v-bind:class="{on: isflag}">class样式</div>

<!-- 三目表达式 isflag为true,绑定类名on2，否则绑定类名 on3-->
<div v-bind:class="isflag?'on2':'on3'">三目运算符</div>
<!-- 数组写法-->
<div v-bind:class="[activeClass]">数组写法</div>

<!--在数组中使用三元表达式-->
<p :class="['on',isflag?'big':'']">{{msg}}</p>

<!--在数组中使用对象简化三元表达式-->
<p :class="['on',{big:isflag}]">{{msg}}</p>

<!--使用对象作为类样式-->
<p :class="{on:false,big:isflag}">{{msg}}</p>

<script type='text/javascript'>
// JavaScript片段
data: {
	isflag: true,
    activeClass:'on'
}f
</script>
~~~



### 10.2、style样式处理

- 对象语法

~~~html
<!--vue中使用行内样式-->
<p  style="color:red">{{msg}}</p>
<!--行内样式对象写法-->
<p :style="{color:'red','font-size':'20px'}">{{msg}}</p>

~~~

### 10.3、 综合案例: 选项卡案例

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        ul {
            display: flex;
        }

        ul li {
            list-style: none;
            margin: 10px 20px;
        }

        .on {
            background-color: red;
        }
    </style>
</head>

<body>
    <div id="app">
        <div class="tabs is-centered">
            <ul>
                <li v-for="(item,index) in tabs" :class="activeTabId==item.id?'on':''" :key="index"
                    @click="activeTabId=item.id">
                    {{item.title}}
                </li>
            </ul>
        </div>
        <div class="box">
            <div v-for="(item,index) in contentArr" v-show="item.id==activeTabId">
                {{item.content}}
            </div>
        </div>
    </div>
    <script>
        const { createApp } = Vue
        createApp({
            data() {
                return {
                    tabs: [
                        { id: 1, title: '图片' },
                        { id: 2, title: '音乐' },
                        { id: 3, title: '视频' }
                    ],
                    activeTabId: 1,
                    contentArr: [
                        {
                            id: 1,
                            content: '我是图片div'
                        },
                        {
                            id: 2,
                            content: '我是音乐div'
                        },
                        {
                            id: 3,
                            content: '我是视频div'
                        }
                    ]
                }
            },
            methods: {
                handleChangeTab(id) {
                    this.activeTabId = id
                }
            }
        }).mount('#app')
    </script>
</body>

</html>
```



## 9、v-model

**作用:**表单元素的绑定，实现了**双向数据绑定**，通过表单项可以更改数据。

![单向与双向数据绑定](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/9c5b0121053708abca7fb4fb7aad1ebbbccda672.png?sign=c753d4526e96e7bbb0e226e6b3036883&t=5f2d216b)

v-model会忽略所有表单元素的value、checked、selected特性的初始值,而总是将Vue实例的数据作为数据来源，应该在data选项中声明初始值。

- 普通文本框上的使用

~~~html
<div id='app'>
    <p>{{message}}</p>
    <input type='text' :value='msg' @input='msg=$event.target.value'/>
    <!--如上的绑定, 可以利用v-model的指令来快速实现-->
     <!--
    v-model其实是`语法糖`, 语法糖：这种语法对语言的功能并没有影响，但是更方便程序员使用
    -->
    <input type='text' v-model='msg'>
</div>

<script type='text/javascript'>
new Vue({
    el: '#app',
    data: {
		msg: 'message默认值'
    }
})
</script>
~~~



- 多行文本框上的使用

~~~html
<div id='app'>
    <textarea v-model="message"></textarea>
</div>

<script type='text/javascript'>
new Vue({
	el: '#app',
	data: {
		message: '我是多行文本内容'
	}
})
</script>
~~~

**注意：在多行文本框中使用插值表达式无效（此时，其只能接受数据，不能改变数据）**



- 单个复选框上的使用

~~~html
<div id='app'>
    <input type="checkbox" v-model="checked">
</div>

<script type='text/javascript'>
new Vue({
	el: '#app',
	data:{
		checked:true
	}
})
</script>
~~~



- 多个复选框上的使用

~~~html
<div id='app'>
    <input type="checkbox" value="html" v-model="checkedNames">
    <input type="checkbox" value="css" v-model="checkedNames">
    <input type="checkbox" value="js" v-model="checkedNames">
</div>

<script type='text/javascript'>
new Vue({
	el: '#app',
	data:{
    	// 如果数组中有对应的value值，则此checkbox会被选中
		checkedNames:['html']
	}
})
</script>
~~~

**注意：此种用法需要`input`标签提供`value`属性，并且需要注意属性的大小写要与数组元素的大小写一致**



- 单选按钮上的使用

~~~html
<div id='app'>
    男<input type="radio" name="sex" value="男" v-model="sex">
	女<input type="radio" name="sex" value="女" v-model="sex">
</div>

<script type='text/javascript'>
new Vue({
	el: '#app',
	data: {
		sex: '女'
	}
})
</script>
~~~



- 下拉框上的使用

~~~html
<div id='app'>
    <select v-model="selected">
        <option >请选择</option>
        <option value ='HTML'>HTML</option>
        <option value ='CSS'>CSS</option>
        <option value ='JS'>JS</option>
    </select>
</div>

<script type='text/javascript'>
new Vue({
	el: '#app',
	data: {
		selected: 'JS'
	}
})
</script>
~~~

### 1. 表单修饰符

表单修饰符：用来处理表单的一些特定行为

> .lazy：默认情况下Vue的数据同步采用`input`事件，使用`.lazy`将其修改为失去焦点时触发
>
> .number：自动将用户的输入值转为数值类型（如果能转的话）
>
> .trim：自动过滤用户输入的首尾空白字符

```html
<!--html 片段-->
<body>
    <div id='app'>
    	<!--不使用.lazy在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 ，
		使用 .lazy 将其修改为失去焦点时触发--> 
    	<input type="text" v-model.lazy="msg" @input="fun"/>
        <!--.number：自动将用户的输入值转为数值类型（如果能转的话）--> 
        <input type="text" v-model.number="num" />
        <!--.trim：自动过滤用户输入的首尾空白字符--> 
        <input type="text" v-model.trim="str" />
    </div>
</body>
<!--js 片段-->
const app = Vue.createApp({
			el: '#app',
			data() {
               return{
				 msg:''，	
                 num:'',
                 str:''
			  }
			},
			methods:{
			fun(){
				console.log(this.msg)
			}
		}
	})
```



### 2.Vue的双向数据绑定原理

**1.原理说明：**

当把一个普通的JavaScript对象传给Vue实例的data选项，Vue将遍历此对象所有的属性，使用Object.defineProperty 把这些属性全部转为getter/setter(数据劫持/数据映射)。在属性被访问和修改时通知变化。每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。

**2. Object.defineProperty(obj, prop, descriptor)** 

1. obj： 要定义属性的对象。 
2. prop：要定义或修改的属性的名称 。
3. descriptor: 描述选项。

​           3.1  value：默认值（给对象属性赋值）

​           3.2  writable： 是否能够写/修改 true | false（默认）

​           3.3  configurable： 是否能够删除 true | false（默认）

​           3.4  enumerable：是否可枚举（遍历） true | false（默认）

​           3.5  get：获取属性值 

​           3.6  set：设置属性值 

>**注意：  不能同时设置(writable，value) 和 get，set方法，否则浏览器会报错。 **

**4.代码演示vue 双向数据绑定原理**let result = {};

```js
<body>
    <div>
         <!-- 输入框 -->
         <input type="text" id="inpt" oninput="changeVal(this.value)" />
     </div>
     <div id="content"></div>
</body>
<script>
 // 将上述代码可以改为如下;
    let obj = {
        name: '郭麒麟',
        age: 20
    };
    // 定义一个观察者函数,用来实现监听原对象数据的变化,即obj的变化
    function observe(target, fn) {
        let result = {}; // 定义一个劫持数据,操作劫持数据,进而达到修改obj的效果
        Object.defineProperty(result, 'name', {
            get() {
                console.log('访问了name');
                return target.name
            },
            set(val) {
                console.log('修改了name');
                target.name = val;
                // 实时监听obj  obj 的name属性被修改 , 模拟vue修改数据,一般修改操作为复杂的函数
                fn(result)
            }
        })
        // 初始化执行
        fn(result)
        return result
    }

    const res = observe(obj, (result) => {
        document.querySelector('p').innerHTML = `${result.name}`
    })

    // 通过输入框来达到修改obj数据的目的 
    function changeVal(val) {
        // console.log(this.value);
        res.name = val
    }
</script>
```

### vue3双向数据绑定原理

```js
<body>
    <p id="p1"></p>
    <input type="text" oninput="changeFn(this.value)">
</body>

</html>
<script>
    // vue3 使用 proxy 实现的数据的代理,操作代理对象的属性,实现监听原对象数据的变化,从而渲染最新的数据到页面
    // vue2 中, 使用Object.defineProperty(代理对象/劫持数据,属性,配置项)
    // 通过修改操作代理对象中的属性,实现对原对象的数据的监听
    // 01: 什么是proxy
    // Proxy为js 内置的构造函数,主要用来实例化创建一个代理对象的

    // console.log(Proxy);
    // 案例:
    let obj = {
        name: '小翠',
        age: 15
    };

    // vue3 通过proxy数据代理实现了对原数据对象的监听
    // 案例:

    let p = new Proxy(obj, {
        get(target, prop) {
            // target 表示obj  
            // prop 表示 target 对象中的属性
            // 内置循环将obj上的所有属性复刻一份到p代理对象上
            // 当你访问p 中的属性时,触发该get 方法
            console.log(`访问了p的${prop}属性`);
            return Reflect.get(target, prop)
        },
        set(target, prop, val) {
            //console.log('添加或修改'+prop+'属性');
            // 根据监听到数据的变化,数据改变,页面重新渲染新数据
            Reflect.set(target, prop, val)
        },
        deleteProperty(target, prop) {
            console.log(`删除课p 上的${prop}属性`);
            return Reflect.deleteProperty(target, prop)
        }
    })
    console.log(p); // p 为代理对象
    // // 访问属性
    // // p.age
    // // 添加或修改属性
    // p.sex = '女'
    // // console.log(p);
    // // console.log(obj);

    // // 删除p上的属性
    // delete p.age
    // console.log(p);
    // console.log(obj);
    document.querySelector('#p1').innerHTML = p.name;
    document.querySelector('input').value = p.name

    function changeFn(value) {
        console.log(value);
        document.querySelector('#p1').innerHTML = value;
        document.querySelector('input').value = value
    }

```



## 10、综合案例：全选/全不选

实现步骤：

- 添加input框，使得页面具备相同的效果
- 给`全选/全不选`的框添加==合适==的事件
- 指定事件处理程序

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	    <script src="./node_modules/vue/dist/vue.global.js"></script>
		<style>
			li{
				list-style: none;
				display: flex;
				justify-content: space-around;
				align-items: center;
			}
			img{
				width: 100px;
				height: 50px;
			}
			#box{
				padding: 30px 0;
			}
		</style>
	</head>
	<body>
		<div id="app">
			<p><input type="checkbox" name="" v-model="checkFlag" @change="allcheck"  />全选/全不选</p>
			<ul>
				<li v-for="item in shopcar" :key= 'item.id'>
					<input type="checkbox" name="" v-model="checkGroup" :value="item"  @change="danxuan"  />
					<img :src="item.pic" >
					<div>
						<p>商品名称：{{item.name}}</p>
						<p>商品价格：{{item.price}}</p>
					</div>
					<p><button @click='item.num--' :disabled="item.num==1">-</button>{{item.num}}<button @click="item.num++" :disabled="item.num==item.limit">+</button></p>
					<button @click="del(item.id)">删除</button>
				</li>
			</ul>
			<div id="box">
				总价:{{sum()}}
			</div>
		</div>		
	</body>
</html>

<script type="text/javascript">
	const app = Vue.createApp({
		data(){
			checkFlag:false,
			checkGroup:[], // 
			shopcar:[
				{
					"id":0,
					"name":"商品1",
					"price":10,
					"num":1,
					"limit":5,            "pic":"https://static.maizuo.com/pc/v5/usr/movie/44dc08914d508fc47c8267c6ca73f2d8.jpg"
				},
				{
					"id":1,
					"name":"商品2",
					"price":20,
					"num":2,
					"limit":5,
					"pic":"https://static.maizuo.com/pc/v5/usr/movie/44dc08914d508fc47c8267c6ca73f2d8.jpg"
				},
				{
					"id":3,
					"name":"商品3",
					"price":30,
					"num":3,
					"limit":5,
					"pic":"https://static.maizuo.com/pc/v5/usr/movie/44dc08914d508fc47c8267c6ca73f2d8.jpg"
				}
			]
		},
		methods: {
			sum(){ // 总价
				//  只要checkGroup 这个数组发生变化，那么该函数就会被重新调用执行
				let total = 0;
				this.checkGroup.forEach((item)=>{
					total+=item.price*item.num
				})
				
				return total
			},
			del(id){ // 删除
				this.shopcar.forEach((item,i)=>{
					if(item.id == id){
						this.shopcar.splice(i,1)
					}
				})
					
				this.checkGroup= this.checkGroup.filter((item)=>{
					 return item.id !=id
				})	
				
				//调用
				this.danxuan()
			},
			allcheck(){ // 全选和全不选
				if(this.checkFlag){
					this.checkGroup = this.shopcar
				}else{
					this.checkGroup = []
				}
				
			},
			danxuan(){ // 单选
				if(this.checkGroup.length == this.shopcar.length){
					this.checkFlag =true
				}else{
					this.checkFlag =false
				}				
			}
		}
	})
    
    app.mount('#app');
</script>

```



## 11、v-memo (不常用)

```vue
 <div id="app" v-cloak> 
     // 案例1: 
     <p>{{ memoData }}</p>
     // 当v-memo 为空数组时, 没有依赖的值, 当前节点数据更新后,不重新渲染
     <div v-memo="[]" @click="changeData">{{ memoData }}</div>
     // 当依赖的项为memoData时, memoData发生变化, 重新渲染当前节点
     <div v-memo="[memoData]" @click="changeData">{{ memoData }}</div>
     
     // 案例2: 
	// item.id === selected 当该值发生变化,当前所在节点才会重新渲染
   <div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
    <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
    <p>...more child nodes</p>
  </div>
		
</div>
<script>
<script>
    const { createApp } = Vue;
    const vm = createApp({
        data() {
            return {
                memoData: 99,
                selected: 1,
                list: [
                    { id: 1, name: 'item 1' },
                    { id: 2, name: 'item 2' },
                    { id: 3, name: 'item 3' }
                ]
            }
        },
        methods: {
            changeData() {
                this.memoData = 1000
            },
        }
    }).mount('#app')
</script>
```

























# 四、Vue常用属性

## 1、自定义指令 - directive

[directive]:https://cn.vuejs.org/guide/reusability/custom-directives.html#directive-hooks

除了核心功能默认内置的指令，Vue也允许开发者注册自定义指令。有的情况下，对普通DOM元素进行底层操作，这时候就会用到自定义指令绑定到元素上执行相关操作。

**自定义指令分为：全局指令和局部指令**，当全局指令和局部指令同名时**以局部指令为准**（局部指令的优先级高于全局的）。

**问题：全局与局部有什么区别？**

- **在当前（非工程化，每一个文件都是一个html文件）的时候是没区别的**
- vue工程化的时候是有区别的
  - 全局的适用于整个项目的（常用）
  - 局部的适用于当前组件的

自定义指令**常用**钩子函数（或生命周期函数）有：

- created (el, binding, vnode, prevVnode) {    // 在绑定元素的 attribute 前 调用  }
- beforeMount(el, binding, vnode, prevVnode) {//   在元素被插入到 DOM 前调用 }, 
-  mounted(el, binding, vnode, prevVnode) {//  在绑定元素的父组件 ,  及他自己的所有子节点都挂载完成后调用 }, 
-  beforeUpdate(el, binding, vnode, prevVnode) { //   绑定元素的父组件更新前调用 }, 
-   updated(el, binding, vnode, prevVnode) {// 在绑定元素的父组件 , 及他自己的所有子节点都更新后调用 },
-   beforeUnmount(el, binding, vnode, prevVnode) {//   绑定元素的父组件卸载前调用 },  
-   unmounted(el, binding, vnode, prevVnode) {//   绑定元素的父组件卸载后调用 } 

> 请注意：不管在定义全局还是局部自定义指令时，**所提及的指令名均是不带`v-`前缀的名称**。

**全局指令语法**

~~~js
// 无参
app.directive('指令名',{
	钩子函数名: function(el){
        // 业务逻辑
    	// el参数是挂载到的元素的DOM对象
    	// <div v-abc>123</div>
    }
}

// 传参
app.directive('指令名',{
	钩子函数名: function(el,binding){
    	let param = binding.value
        // 业务逻辑
    },
    ....
}
~~~

> **请务必注意，作为全局配置，不能将其写在指定的Vue实例里，后续其它全局配置亦是如此**

**局部自定义指令定义**

可以在`new Vue`的时候添加`directives`以注册局部自定义指令，局部自定义指令只能在当前组件实例中使用：

~~~javascript
directives: {
  指令名: {
    // 指令的定义
    钩子函数名: function (el,binding) {
      // 业务逻辑
    }
  }
}
~~~

**函数简写（了解，使用机会很少）**

> 部分时候，我们可能想在 `bind` **和 **`update` 时触发**相同**行为（如果只是其一，则还是单独分开声明），而不关心其它的钩子。那么这样写：

~~~javascript
// 全局
app.directive('指令名', function (el,binding) {
  // 业务逻辑
})

// 局部
directives: {
  指令名(el,binding) {
      // 业务逻辑
  }
}
~~~

> 在自定义指令的方法中，不能像以前的`methods`中的方法一样使用关键词`this`，此时`this`关键词指向的是`Window`对象。

案例：使用自定义指令实现以下效果

- 使用全局指令定义自定义的`v-red（不传参）`和`v-color（传参）`，在元素被插入时设置内容颜色
- 使用局部自定义指令实现`v-mobile（不传参）`验证用户输入的是否是合法的手机号，不合法手机号为红色，合法为黑色

~~~html
 <div id="app">
        <!-- 自动聚焦指令 v-focus -->
        <input type="text" v-focus>
        <!--使用局部自定义指令  -->
        <p v-color v-if="flag">{{msg}}</p>
        <button @click="flag=!flag">修改flag</button>
        <!-- 全局自定义指令传参 -->
        <p v-blue="'blue'">自定义指令传递参数</p>
        <!-- 指令v-mobile：需要验证用户输入的手机号是否合法 -->
        手机号: <input type="text" v-model="mobile" v-mobile />
    </div>
 <script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
    const { createApp } = Vue;
    const app = createApp({
        data() {
            return {
                msg: "自定义指令",
                flag: true,
                mobile: ''
            }
        },
        methods: {

        },
        // 自定义的局部指令
        directives: {
            focus: { // 自动聚焦
                mounted: (el) => el.focus()
            },
            color: {
                created(el, binding, vnode) {
                    //   console.log(el, binding, vnode);
                },
                beforeMount(el, binding, vnode, prevVnode) {
                    //console.log(el, binding, vnode, prevVnode);
                },
                mounted(el, binding, vnode, prevVnode) {
                    //console.log(el, binding, vnode, prevVnode);
                },
                beforeUpdate(el, binding, vnode, prevVnode) {
                    // console.log(el.innerHTML, binding, vnode, prevVnode);
                },
                updated(el, binding, vnode, prevVnode) {
                    // console.log(el.innerHTML, binding, vnode, prevVnode);
                },
                beforeUnmount(el, binding, vnode, prevVnode) {
                    // console.log(el, binding, vnode, prevVnode);
                },
                // 绑定元素的父组件卸载后调用
                unmounted(el, binding, vnode, prevVnode) {
                    // console.log(el, binding, vnode, prevVnode);
                }
            },
            mobile: {
                // 定义需要使用的函数
                updated(el) {
                    console.log(el);
                    // 获取手机号
                    let mobile = el.value
                    // 正则表达式验证手机号是否合法
                    if (/^1[3-9]\d{9}$/.test(mobile)) {
                        el.style.color = "black"
                    } else {
                        el.style.color = "red"
                    }
                }
            }

        }
    })
    
	// 全局自定义指令
    // 注意全局自定义指令必须写在  app.mount('#app') 之前,否则报错
    app.directive('blue', {
        mounted(el, binding, vnode, prevVnode) {
            // console.log(el, binding, vnode, prevVnode);
            el.style.background = binding.value
        }
    })

    app.mount('#app')
</script>
~~~



## 2、计算属性 - computed

01: 为什么使用计算属性

模板中放入太多的逻辑（方法）会让模板过重且难以维护，使用计算属性可以让模板变得简洁易于维护。计算属性是基于它们的响应式依赖进行**缓存**的

02: 计算属性语法特点

计算属性定义在Vue对象中，通过关键词`computed`属性对象中定义一个个函数，并返回一个值，该返回的值就是计算属性的值, 使用计算属性时和`data`中的数据使用方式一致。

计算可以根据一个或者多个现有的数据 来生成一个新的数据, 并且新的数据依赖于现有的数据,当依赖的数据发生变化, 计算属性会重新计算

核心点：

- 计算属性其在代码的表现也是方法，但是与methods不同
  - 计算属性必须有return
- 在某些场景下，计算属性的效率要比methods效率高
  - 计算属性支持数据的缓存操作（在依赖数据不变的情况下），而methods不行
  - 只要依赖的数据源不发生改变，计算属性里的对应方法就只被调用1次，其它时候被调用时则使用缓存。提高效率。

**示例**

~~~html
<div id="app">
    <!-- 当多次调用 cfn计算属性时只要里面的 num值不改变,它会把第一次计算的结果直接返回直到data中的num值改变 计算属性才会重新发生计算 -->
    <div>{{ cfn }}</div>
    <div>{{ cfn }}</div>
    <!-- 调用methods中的方法的时候  他每次会重新调用 -->
    <div>{{ fn() }}</div>
    <div>{{ fn() }}</div>
</div>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script type="text/javascript">
    Vue.createApp({
        data() {
            return {
                num: 'abc',
            }
        },
        methods: {
            fn() {
                console.log("methods");// methods 中的方法每次调用都会执行
                return this.num.toUpperCase();
            },
        },
        // 计算属性
        computed: {
            cfn() {
                console.log("computed");// 如果依赖的数据没有变化，则计算属性会返回上一次结果，使用					缓存
                return this.num.toUpperCase();
            }
        },

    }).mount('#app')
</script>
~~~



03: 计算属性默认只有 getter，不过在需要时你也可以提供一个 setter：现在再运行 `vm.fullName = 'John       `
     `Doe'` 时，setter 会被调用，`vm.firstName` 和 `vm.lastName` 也会相应地被更新。

**课堂案例:** 

```js
// html
 <div id="app">
        <p>
            <input type="text" name="" id="" v-model="firstname">
        </p>
        <p>
            <input type="text" name="" v-model="lastname">
        </p>
        <p>
            <input type="text" v-model="fullname">
        </p>
 </div>

<script>
Vue.createApp({
    data() {
        return {
            firstname: '',
            lastname: ''
        }
    },
    // 计算属性
    computed: {
        fullname: {
            get() {
                return this.firstname + this.lastname
            },
            set(val) {
                console.log(val);
                this.firstname = val.substr(0, 1);
                this.lastname = val.substr(1)
            }
        }
    }
}).mount('#app')
</script>
```

课堂案例： 全选/反选/单选

​      ![image-20211014163502383](https://i.loli.net/2021/10/14/KXiYbI4NyRadPsZ.png)

**代码实现：**

```html
<script src="./node_modules/vue/dist/vue.global.js"></script>
<body>
    <div id="app">
        <table>
            <tr v-for='item in goodlist'>
                <td>
                    <input type="checkbox" v-model='item.checked'>
                </td>
                <td>
                    商品名称：{{item.name}}
                </td>
                <td>
                    商品价格：{{item.price}}
                </td>
                <td>
                    数量：{{item.num}}
                </td>
            </tr>
        </table>
        <!-- 全选 -->
        <div>
            <input type="checkbox" name="" v-model='checkall'> 全选
        </div>
    </div>
</body>
<script>
    Vue.createApp({
        data() {
            return {
                goodlist: [
                    {
                        id: 0,
                        name: "苹果",
                        checked: true,
                        price: 100,
                        num: 1
                    },
                    {
                        id: 1,
                        name: "梨",
                        checked: false,
                        price: 200,
                        num: 2
                    }, {
                        id: 2,
                        name: "香蕉",
                        checked: true,
                        price: 300,
                        num: 3
                    }
                ]
            }
        },
        // 计算属性
        computed: {
            checkall: {
                get() {
                    // 只要 checkall 依赖的数据发生变化，checkall就会重新调用get,也就是重新计算
                    // console.log(11, this.goodlist.every((item) => { return item.checked == true }))
                    // checkall的结果为true或false 
                    return this.goodlist.every((item) => { return item.checked == true })
                },
                set(val) {
                    // 修改checkall 的值
                    // console.log(val)
                    // 让每一个子复选框的checked = 全选的checked
                    this.goodlist.forEach(item => {
                        item.checked = val
                    });
                }
            }
        }
    }).mount('#app')
</script>
```



## 3、监听器 - watch

使用watch来侦听**data**中数据的变化，**watch中的属性（watch是对象格式）一定是data 中已经存在的数据**。（特殊情况除外 如: 监听路由的变化）

**使用场景：**数据变化时执行**异步或开销比较大的操作**。

**典型应用：**http://www.pinyinzi.cn/

![监听器](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/6ae51459d0c21c076baf58e26829a6265e3ee938.png?sign=87b53c566e524496faa45fc17ee38816&t=5f3648ac)

**案例：**给定三个输入框，第一个为姓输入框，第二个为名输入框，第三个为姓名组合结果框；要求当用户更新姓或名后，第三个输入框自动生成完整的姓名结果。

**语法**

~~~vue
new Vue({
	.....
	watch: {
		data中数据的名称: fn方法,
		....
	}
})
~~~

**参考代码：**

~~~html
<div id="app">
    <p><input type="text" v-model='firstName' placeholder="姓" /></p>
    <p><input type="text" v-model='lastName' placeholder="名" /></p>
    <p><input type="text" v-model='fullName' placeholder="全名" /></p>
</div>

<script src="./node_modules/vue/dist/vue.global.js"></script>
<script type="text/javascript">
    Vue.createApp({
        data() {
            return {
                firstName: '',
                lastName: '',
                fullName: ''
            }
        },
        methods: {

        },
        watch: {
            firstName: function (newvalue, oldvalue) {
                this.fullName = newvalue + ' ' + this.lastName
            },
            lastName: function (newvalue, oldvalue) {
                this.fullName = this.firstName + ' ' + newvalue
            }
        }
    }).mount('#app')
</script>
~~~

> 注意点：
>
> - 声明监听器，使用的关键词是`watch`
> - 每个监听器的方法，可以接受2个参数，第一个参数是新的值，第二个参数是之前的值

**注意：**当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，此时就需要deep属性对对象进行**深度监听**。 监听的时候可以配置参数 

**使用对象的数据形式改写上述案例参考代码：**

~~~html
<div id="app">
    <p><input type="text" v-model='userinfo.firstName' placeholder="姓" /></p>
    <p><input type="text" v-model='userinfo.lastName' placeholder="名" /></p>
    <p><input type="text" v-model='userinfo.fullName' placeholder="全名" /></p>
</div>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script type="text/javascript">
    Vue.createApp({
        data() {
            return {
                userinfo: {
                    firstName: '',
                    lastName: '',
                    fullName: ''
                }
            }
        },
        watch: {
            userinfo: {
                // handler是固定的写法,监控处理函数
                handler(val) {
                    console.log(val);
                    this.userinfo.fullName = val.firstName + ' ' + val.lastName
                    // 对象支持引用传值
                    //val.fullName = val.firstName + ' ' + val.lastName
                },
                deep: true,  // 深度监听
                immediate: true, //值为true或false，默认false 初始化是否监听
            }
        }
    }).mount('#app')
</script>
~~~



**案例3: watch监听器中查看dom的状态**

```js
<div id="app">
    <div id="box" v-if="flag" class="box echart">box</div>
	<button class="button" @click="flag = !flag">toggle</button>
</div>

Vue.createApp({
        data() {
            return {
                flag: false
            }
        },
        methods: {

        },
        watch: {

            async flag() {
                //第一种方式:  发生变化时,可以利用setTimeout 0 来包裹,将操作dom的逻辑放入到最后的事件循环中, 等待vue所有动作结束后执行
                // setTimeout(() => {
                //     console.log(document.querySelector('#box'))
                // }, 0);
                console.log(1, document.querySelector('#box'))

                //第二种方式:  nextTick会等到dom更新后执行 // vue2: this.$nextTick
                // nextTick(() => {
                //     console.log(document.querySelector('#box'))
                // })

                // 第三种方式
                await nextTick()
                console.log(2, document.querySelector('#box'))
            },

            flag: {
                handler() {
                    console.log(document.querySelector('#box'))
                },
                
                flush: 'post' //调整回调函数的刷新时机
                //当你更改了响应式状态，它可能会同时触发 Vue 组件更新和侦听器回调。
                //默认情况下，用户创建的侦听器回调，都会在 Vue 组件更新之前被调用。这意味着你在侦听器回				   //调中访问的 DOM 将是被 Vue 更新之前的状态。如果想在侦听器回调中能访问被 Vue 更新之后			      //的DOM，你需要指明 flush: 'post' 选项
            }
        }
    }).mount('#app')
```



案例: 

```js
    <div id="app">
        <div id="box" v-if="flag" class="box echart">box</div>
        <button class="button" @click="flag = !flag">toggle</button>
    </div>
</body>


<script>
    // watch会在状态发送变化之后, 立即执行, 此时dom还没有更新, 如果需要对更新后的dom进行操作, 有下面几种方式
    const { nextTick } = Vue;
    console.log(Vue);
    Vue.createApp({
        data() {
            return {
                flag: false
            }
        },
        methods: {

        },
        watch: {
            async flag() {
                //第一种方式:  发生变化时,可以利用setTimeout 0 来包裹,将操作dom的逻辑放入到最后的事件循环中, 等待vue所有动作结束后执行
                // setTimeout(() => {
                //     console.log(document.querySelector('#box'))
                // }, 0);
                console.log(1, document.querySelector('#box'))

                //第二种方式:  nextTick会等到dom更新后执行 // vue2: this.$nextTick
                // nextTick(() => {
                //     console.log(document.querySelector('#box'))
                // })

                // 第二种方式
                await nextTick()
                console.log(2, document.querySelector('#box'))
            },
            // 第三种方式
            flag: {
                handler() {
                    console.log(document.querySelector('#box'))
                },
                flush: 'post' // post：表示dom更新完成后调用
            }
        }
    }).mount('#app')
</script>
```

**面试题：vue中计算属性与监听器有什么区别？？**



## 4、**watch与computed的区别:**

1、watch监控现有的属性,computed通过现有的属性计算出一个新的属性

2、watch不会缓存数据，每次打开页面都会重新加载一次, 但是computed如果之前进行过计算他会将计算的结果缓存，如果再次请求会从缓存中,得到数据（所以computed的性能比watch更好一些）  

3、watch 一般用于在数据变化时执行异步或开销较大的操作，computed 一般用于简单运算的操作。

4、计算属性支持深度深度数据是否变化的监听的（默认的）,watch监听器默认不支持深度响应，仅支持字面量处理，但是其支持通过代码的改动来支持深度监听





## 6、生命周期(重点)

生命周期：从vue实例产生开始到vue实例被销毁这段时间所经历的过程。

每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如需要设置数据监听、编译模板、挂载实例到 DOM，在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，目的是给予用户在一些特定的场景下添加他们自己代码的机会。



 **所有生命周期钩子函数的 `this` 上下文都会自动指向当前调用它的组件实例。注意：避免用箭头函数来定义生命周期钩子，因为如果这样的话你将无法在函数中通过 `this` 获取组件实例。** 



Vue生命周期的**主要阶段**：

- 挂载（初始化相关属性）
  - beforeCreate    
    - **注意点**：vue实例创建前阶段，在此时不能获取data中的数据，也就是说`this.msg`得到的是`undefined`
  - created    
    - **注意点**：vue实例已经创建完成 ，在此时可以获取data中的数据和methods 中的方法,以及计算属性
    - 可以在该阶段调用接口,请求数据
  - beforeMount
    - **注意点：** 组件被挂载之前, 当这个钩子被调用时，组件已经完成了其响应式状态的设置，但还没有创建 DOM 节点 
  - mounted【页面加载完毕的时候就是此时】
    - **注意点**：默认情况下，在组件的生命周期中只会触发一次
    - 可以进行数据的请求
- 更新（元素或组件的变更操作）
  - beforeUpdate   在组件即将因为一个响应式状态变更而更新其 DOM 树之前调用。 
  - updated   在组件因为一个响应式状态变更而更新其 DOM 树之后调用。 
    - **注意点**：可以重复触发的
- 销毁（销毁相关属性）
  - beforeDestroy   在一个组件实例被卸载之前调用。 一般用于清除副作用
    - **注意点**：
  - destroyed   在一个组件实例被卸载之后调用。  一般用于清除副作用

**示例代码：**

```html
<!--html代码片段-->
<body>
    <div id="app">
        <input type="text" placeholder="用户名" v-model='inpvalue' />
        <button @click='add'>添加</button>
        <p id="myp">{{msg}}</p>
        <button @click='edit'>修改数据</button>
    </div>
</body>

</html>
<script type="text/javascript">
    // js代码片段
   const app = Vue.createApp({
        el: '#app',
        data: {
            "msg": "我是定义的数据",
            "inpvalue": ''
        },
        methods: {
            add() {
                console.log(this.inpvalue)
            },
            edit() {
                this.msg = '我被修改了'
            }
        },
        beforeCreate() {
            //组件实例创建前 
            console.log(this.msg) // undefined 表示此时data中得数据还没有初始化好
            console.log(this.add) // undefined 说明此时methods 中得方法还没有初始化好
           
        },
        created() {
            // 组件实例创建完成  第二个生命周期函数 
            console.log(this.msg) //可以输出  说明 此时data得数据已经初始化完毕 
            console.log(this.add) //add()   说明此时methods 中得方法 已经初始化完毕
        },
        beforeMount() {
            //第三个生命周期函数(表示 挂载前，还没有将渲染模板挂载在el #app元素上)
            console.log(document.querySelector('p'))// 输出 <p id="myp">{{msg}}</p>
                //说明 此时页面还没有重新渲染，还是旧页面，但是数据渲染出来的页面模板已经在浏览器的内存中，还没有将该渲染好得模板替换  el#app 中得模板
        },
        mounted() {
            // 第4个生命周期函数,渲染模板已经挂载在el #app元素上
              console.log(document.querySelector('p'))//输出 <p id="myp">我是定义的数据</p>
            // 表示页面已经完成了渲染，浏览器内存中新的渲染模板页面已经替换掉了 el#app 中得旧模板页					面，即el#app 容器已经过载了新的渲染模板
            document.onclick = function() {
                console.log('document点击事件')
            }
        },
        beforeUpdate() {
            // 数据更新前触发
            console.log(this.msg) //输出结果： 我被修改了
            console.log(document.getElementById('myp').innerHTML) // 输出结果：我是定义的数据
            // 说明 此阶段 是数据已经被修改了，但是页面中的 el# app 容器内的 DOM 树还没有重新挂载					到 el 上，页面上还是旧数据，但是浏览器 内存中已经更新了 新的DOM 树了，
        },
        updated() {
            // data 中的数据修改后，触发该生命周期函数
            console.log(this.msg) //输出结果： 我被修改了
            console.log(document.getElementById('myp').innerHTML) // 输出结果：我被修改了
            // 此时： 页面中的el #app 中的dom 已经重新挂载，页面渲染渲染了新数据的页面
        },
        beforeDestroy() {
            // 实例销毁前触发
        },
        destroyed() {
            // 实例销毁后触发
            document.onclick = null // 这样切换路由组件后，其他组件上就没有document点击事件了
        }
    })
</script>	
```

![生命周期](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/5d6a79ad2a3b74d4ad9d70f868de0545f9939b8f.png?sign=b2936e9953891efac26abe8060495936&t=5f365858)

关于8个生命周期涉及到的方法，可以参考Vue官网API：[https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90](https://cn.vuejs.org/v2/api/#选项-生命周期钩



## 7、了解虚拟DOM与diff算法概念

**什么是虚拟DOM？**

![虚拟DOM](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/7782f6424492630815195ed5722bdae78448601c.png?sign=e33c1bcebed7f86de415715793bb5444&t=5f60a247)

定义：指将真实的dom按照特定的语法转化（抽象）成一个js对象，这个**js对象称之为虚拟dom**。



**什么是diff（different）算法？**

差异比较算法的一种，把树形结构按照层级分解，只**比较同级**元素。不同层级的节点只有添加和删除操作

![11](https://s2.loli.net/2022/03/15/SenIoB7Wx9OgqiK.png)

​																	     **上图为新旧虚拟dom同层比较**





![1654820249935](C:\Users\wusheng\AppData\Roaming\Typora\typora-user-images\1654820249935.png)

**`虚拟DOM+diff算法`的方式与`传统DOM操作`相比，有什么好处？**

**传统DOM操作**：在一次操作中，往往会伴随多次个DOM节点更新，浏览器收到第一个DOM请求后并不知道还有若干次更新操作，因此会马上执行流程，最终执行若干次次。而且操作DOM频繁还会出现页面卡顿，影响用户体验。

**虚拟DOM+diff算法**：若一次操作中有若干次更新DOM的动作，此时 首先 会产生新的虚拟dom结构,新的虚拟dom结构会和上一次的虚拟dom结构作比较, 此时比较会采用diff算法同层比较,比较的内容有(虚拟dom节点的增加/删除/修改/属性的改变,内容的改变等), 然后将这若干次更新的diff内容记录到本地一个JS对象中，虚拟DOM不会立即操作DOM，等到新旧虚拟dom对比完后, 最后将这个本地记录差异变化的JS对象**一次性**应用到DOM树上,该js对象中哪里有变化,就局部更新页面对应的真实dom位置部分,这样就实现了性能的最大优化,避免了重复的渲染.

建议：面试之前一定要去找下比较正规的理论性的东西。



# 五、网络请求

## 1、fetch

优点：

- 浏览器内置的，相比jq而言，省去了导入js包的麻烦
- 可以看做是xhr的升级版
- 支持promise
- 支持多种请求类型，但是默认为get请求类型

官网地址：https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API

语法1：

~~~js
// 使用fetch发送get（默认get）形式请求，返回数据采用json格式，最终打印获取到的数据
fetch(url).then(res => res.json()).then(res => console.log(res))

// 涉及到的数据的转化方法
res.json()		//res 它只是一个 HTTP 响应，而不是真的JSON。为了获取JSON的内容，我们需要使用 json()
~~~

语法2：

~~~js
// 发送json数据
fetch(url, {
	method: 'POST', // or 'PUT'
	body: JSON.stringify({key:value}), // 发送的json数据
 	headers: new Headers({
    	'Content-Type': 'application/json'
  	})
}).then(res => res.json()).then(res => console.log(res))
~~~

语法3：

~~~js
// 发送表单数据
fetch(url, {
	method: 'POST', // or 'PUT'
	body: "username=zhangsan&age=11", // 发送的json数据
 	headers: new Headers({
    	'Content-Type': 'application/x-www-form-urlencoded'
  	})
}).then(res => res.json()).then(res => console.log(res))
~~~

案例：使用fetch获取接口地址`https://api.i-lynn.cn/college`，获取到数据之后，将数据中的`list`展示在页面上即可

**注意：网络请求需要在created周期中写**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">

    </div>
</body>
</html>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            list: ''
        },
        methods: {
            // res.json()// 将返回的数据转化为json格式（常用）
            getData() {
                fetch("https://api.i-lynn.cn/college")
                    .then(res => res.json()).then(res => {
                    console.log(res)
                })
            },
            getData1() {
                // 发送表单数据
                fetch("https://api.i-lynn.cn/college", {
                    method: 'POST',
                    body: "username=zhangsan&age=11", // 发送的表单编码格式
                    headers: new Headers({
                        // 参数是表单的编码格式
                        'Content-Type': 'application/x-www-form-urlencoded'
                    })
                }).then(res => res.json()).then(res => console.log(res))
            },
            getData2() {
                // 发送json数据
                fetch("https://api.i-lynn.cn/college", {
                    method: 'POST',
                    body: JSON.stringify({ username: 123, age: 456 }), // 发送的json数据
                    headers: new Headers({
                        // 参数是对象形式
                        'Content-Type': 'application/json'
                    })
                }).then(res => res.json()).then(res => console.log(res))
            }
        },
        created() {
            this.getData()
            this.getData1()
            this.getData2()
        }

    })
</script>
~~~

作业：自己写一个表单，表单可以输入任意一个合法的ipv4地址，点击查询按钮查询接口：https://api.i-lynn.cn/ip?query=获取ip地址对应的信息，将信息展示在页面上即可。



## 2、axios

- **1.文档：https://www.kancloud.cn/yunye/axios/234845**

axios 是一个基于 promise 的 **HTTP 库**，可以用在浏览器和node.js中。**axios是vue作者推荐使用的网络请求库**，它具有以下特性：

​				a.支持浏览器和node.js（降低学习成本） 

​				b.支持promise

​				c.能够拦截`请求和响应`（拦截器）

​				d.自动转换json数据

- **2.在使用axios之前需要在对应的模板文件中引入axios的js库文件**，随后按照以下用法使用axios：**

```JS
//使用 cdn:
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

- **3.如下为get和post请求用法集合**

~~~javascript
    const app = Vue.createApp({
        el: "#app",
        data: {
            list: ''
        },
        methods: {
            // get 请求方式1
            getData0() {
                axios.get('https://api.i-lynn.cn/college?id=100').then(ret => 		 					console.log(11, ret.data))
            },
            // get 请求方式2
            getData() {
                axios.get('https://api.i-lynn.cn/college', {
                    params: {
                        id: 10010,
                        name: 'zhangsan',
                        age: 26
                    }
                }).then(ret => console.log(11, ret.data))
            },
            // post 请求方式1
            getData1() {
                // 参数是对象形式的，axios发送的请求头是application/json
                axios.post('https://api.i-lynn.cn/college', {
                    firstName: 'zhang',
                    lastName: 'san'
                }).then(res => {
                    console.log(res)
                })
            },
            // post 请求方式2
            getData2() {
                // 参数是表单编码格式，axios发送的请求头是application/x-www-form-urlencoded 
                axios.post('https://api.i-lynn.cn/college', 		 				                         "firstName=zhang&lastName=san").then(res => {
                    console.log(res)
                })
            },
            // get 和 post 综合请求 1
            getData3() {
                // 先不指定请求类型，在配置中去指定请求类型（类似$.ajax）
                axios({
                    method: 'post',
                    url: 'https://api.i-lynn.cn/college',
                    // 超时时间：如果请求花的时候超过了预设时间，则请求取消
                    timeout: 1000,
                    // post 请求需要设置headers 请求头
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded'
                        // 'Content-Type': 'application/json'
                    },
                    data: "username=zhangsan&type=2",
                    // data: {
                    //     username: 'zhangsan',
                    //     type: 2
                    // }
                }).then(res => { console.log(res) })
            },
            // get 和 post 综合请求 2
            getData4() {
                // 先不指定请求类型，在配置中去指定请求类型（类似$.ajax）
                axios({
                    method: 'get',
                    url: 'https://api.i-lynn.cn/college',
                    // 超时时间：如果请求花的时候超过了预设时间，则请求取消
                    timeout: 1000,
                    // data: "username=zhangsan&type=2", // post请求方式
                    // data: {  // post请求方式
                    //     username: 'zhangsan',
                    //     type: 2
                    // },
                    params: { // get 请求方式
                        ID: 12345
                    },
                }).then(res => { console.log(22, res) })
            }
        },
        created() {
            this.getData0()
            // this.getData()
            // this.getData1()
            // this.getData2()
            // this.getData3()
            // this.getData4()
        }

    })
~~~

axios 请求总结：

```js
1. axios：是vue作者推荐在vue中使用的网络请求库
2. 简写方式有：              
      ①get请求：
     	 axios.get(url).then(res => 处理程序)
      ②post请求         
         axios.post(url,请求体,{options}).then(res => 处理程序) 
3.常规写法有：
        axios({
        	url,
        	method, 
        	headers, 
        	params, // get方式传参
        	data, // post 方式参数
        	....
        }).then()
     
 4.与fetch 对比：
  与fetch不一样，fetch最终的res就是我们的返回值，而axios这里最后的res并不是我们的返回值，而是axios的请求   响应对象，我们的返回数据在res.data中
   
```



- **4.当然axios除了支持传统的`GET`和`POST`方式以外，常见的请求方式还支持：**
  - put：修改数据
  - delete：删除数据

> **需要注意，针对POST请求，此处的参数提交格式以参数形式为准：**

```js
//1.发送json格式的数据：
   axios.post(url,{js对象}).then() // 头信息会自动被设置为application/json
//2. 发送表单格式的数据：
   axios.post(url,"a=1&b=2").then()  // 头信息会自动被设置成application/x-www-form-urlencoded
```

以上方的axios请求示例为例，接口响应结果（`ret`）的主要属性有：

​		a. **data：实际响应回来的数据（最常用）**

 	   b. status：响应状态码
 	
 	   c. statusText：响应状态信息



- **5.axios 全局默认配置**

​     在使用axios发送请求之前它允许我们通过**全局配置**做一些设置，这样可以方便后续的请求操作，例如：

```js
// 方式1：可以通过`axios.defaults`来进行全局默认配置。
axios.defaults.baseURL = 'http://localhost/app'; //【设置默认地址】
axios.defaults.timeout = 3000; //【设置超时时间】
axios.defaults.headers['_token'] = '123123123';//【设置请求头信息，通用头信息】
axios.defaults.headers.common['_token'] = '123123';//【通用头信息，common可以不写】


// 方式2：可以通过创建axios实例进行全局默认配置
 const instance = axios.create({
     baseURL: 'http://kumanxuan1.f3322.net:8001',
     timeout: 1000,
     headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
 });
```

- 6. 拦截器

```js
    // 设置请求拦截器
    instance.interceptors.request.use(function (config) {
        // Do something before request is sent
        // 请求
        config.headers.token = 123
        console.log('请求拦截', config)
        return config;
    }, function (error) {
        // Do something with request error
        return Promise.reject(error);
    });

    // 设置响应拦截器
    instance.interceptors.response.use(function (response) {
        // Do something with response data
        console.log('响应拦截', response)
        alert('请求成功')
        return response;
    }, function (error) {
        // Do something with response error
        return Promise.reject(error);
    });
```

**综合案例：**

自己写一个表单，表单可以输入任意一个合法的ipv4地址，点击查询按钮查询接口：https://api.i-lynn.cn/ip?query=获取ip地址对应的信息，将信息展示在页面上即可。

![image-20211014143523009](https://i.loli.net/2021/10/14/UoSRzcwZdvVelt4.png)

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./js/vue.js"></script>
    <!-- 使用之前记得先导入axios文件 -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>
<body>
    <div id="app">
        <div>
            <input type="text" v-model="ip" placeholder="请输入合法的ipv4地址" />
            <button @click="clickHandler">查询</button>
        </div>

        <div v-show="isShow">
            <div>查询的ip地址：{{res.ip}}</div>
            <div>国家/地区：{{res.country}}</div>
            <div>运营商：{{res.area}}</div>
        </div>
    </div>
</body>
</html>
<script>
    new Vue({
        el: "#app",
        data: {
            ip: "",
            res: {},
            isShow: false
        },
        // 点击事件处理程序
        methods: {
            clickHandler() {
                // 获取ip地址
                let ip = this.ip
                // get请求
                axios.get("https://api.i-lynn.cn/ip?query=" + ip).then((res) => {
                    console.log(res)
                    if (res.status == 200) {
                        this.res = res.data
                        this.isShow = true
                    }
                })
                // post请求
                axios.post("https://api.i-lynn.cn/ip", "query=" + ip).then((res) => {
                    if (res.status == 200) {
                        this.res = res.data
                        this.isShow = true
                    }
                });
                axios.post("https://api.i-lynn.cn/ip", { query: ip }).then((res) => {
                    if (res.status == 200) {
                        this.res = res.data
                        this.isShow = true
                    }
                });
            }
        }
    })
</script>
~~~



## 3、axios 与fetch的区别(面试题)

- 不同点: 
  - axios 相当于 ajax 的promise 封装, 底层还是对XMLHttpRequest的封装
  - axios 可以设置请求拦截和响应拦截
  - fetch 是ECMAScript 的一种新 的请求方式,语法简单,不需要额外引入,直接使用
  - fetch 不能自动自动转换json,axios 可以.
  
- 相同点:

  - 都是异步的请求方式

  - 他们返回都是promise 对象, 

    

# 六、组件

## 1、什么是组件

组件允许我们将 UI 划分为独立的、可重用的部分，并且可以对每个部分进行单独的思考。在实际应用中，组件常常被组织成层层嵌套的树状结构：

 组件 （Component）是 Vue.js 最强大的功能之一，**组件是一个自定义HTML元素（标签）**或称为一个模块，包括所需的模板（HTML）、逻辑（JavaScript）和样式（CSS）。



**组件化开发的特点：**

- 标准
- 分治（解耦）
- 重用
- 组合

组件也是有`全局（component）`与`局部（components）`之分。

**组件使用分析：**

 ![img](file:///C:\Users\Administrator\AppData\Roaming\Tencent\Users\1038050095\QQ\WinTemp\RichOle\TL)FF1V6RJ3PGD7BC4UY@T4.png) ![1](C:\Users\Administrator\Desktop\1.png)

## 2、组件的注册及使用 

在使用组件时需要注意以下几点：

- 组件模板`template`

  - 必须是单个根元素

  - ~~~html
    <!-- 单个根元素 div -->
    <div>
        <ul>
            <li></li>
        </ul>
        <ul>
            <li></li>
        </ul>
    </div>
    
    <!-- 没有包裹的根元素 -->
    <p></p>
    <p></p>
    ~~~

  - 支持模板字符串形式

- 组件名称命名方式

  - 短横线方式（推荐）
    - my-component
  - 大驼峰方式（只能在其他组件模板字符串中使用，不能在HTML模板中**直接**使用）
    - MyComponent

> 大驼峰式组件名不能在HTML模板中直接使用，如果需要在HTML模板中使用，需要将其进行特定规则转化：
>
> - 首字母从大写转为小写
> - 后续每遇到大写字母都要转化成小写并且在转化后的小写字母前加`-`
>
> 例如，`WoDeZuJian`这个大驼峰组件名在HTML中使用的时候需要写成`wo-de-zu-jian`



### 2.1、全局组件

全局组件注册形式如下：

~~~javascript
 const { createApp } = Vue
// 声明全局组件
app.component(componentName,{
    // 用于定义组件的视图内容，必须有一个跟标签
    template: '组件模版内容'
    // 存放该组件需要使用的数据
    data: function(){
        return {
            
        }
    },
})
~~~

上述示例中，`component()`的第一个参数是`组件名`（**实则可以看作是HTML标签名称**），第二个参数是一个对象形式的选项，里面存放组件的声明信息。全局组件注册后，任何Vue实例都可以使用。

代码演示：

~~~javascript
//html部分 
// 全局组件可以在不同实例使用
<body>
    <div id="app">
        <global-child></global-child>
    </div>
    <div id="app1">
        <global-child></global-child>  
    </div>
</body>

// js 部分 
// 声明一个全局的globalChild组件
    app.component("globalChild", {
        template: `<div><p>我是全局组件----{{globalname}}</p><p>我是第二个p</p></div>`,
        data() {
            return {
                globalname: "我是全局数据"
            }
        }
    })
    // 实例
    const app = Vue.createApp({
        data(){
			return{
                
            }
        }
    })
~~~

### 2.2、局部组件

1. 局部组件是在父组件的实例中注册子组件，通过某个 Vue 实例/组件的实例选项 components 注册。
2. 注册完只能在当前的父组件中使用.

例如，有以下代码：

~~~javascript
// html 
<body>
    <div id="app">
        <partchild></partchild>
    </div>
</body>

// js
const app = Vue.createApp({
        data(){
			return{
                
            }
        },
        components: {
            partchild: {
                template: `<p>我是局部组件---{{partname}}</p>`,
                data() {
                    return {
                        partname: "我是局部组件数据"
                    }
                }
            }
        }
    }).mount('#app')
~~~

### 2.3、组件的使用

1. 在HTML模板中，组件以**一个自定义标签的形式存在**，起到占位符的功能。通过Vue.js的声明式渲染后，占位符将会被替换为实际的内容，下面是一个最简单的模块示例：

```html
<div id="app">
    <my-component></my-component>
</div>
```

2. 组件的嵌套：

也可以在一个组件的组件模板中去使用**其他已经注册的组件（一般是指全局组件）**的组件，例如：

~~~html
<body>
    <div id="app">
        <!-- 使用局部组件departchild -->
        <departchild></departchild>
    </div>
</body>

<script type="text/javascript">
   
   const app = Vue.createApp({
        components: {
            // 注册局部组件departchild
            departchild: {
                template: `<div>
                                <div>我是局部组件departchild</div>
                                    <globalchild></globalchild>
                            </div>`
            }
        }
    })
    
     // 注册全局组件globalchild
    app.component('globalchild', { 
        template: `<div>我是全局组件globalchild</div>`
    })
    app.mount('#app')
</script>
~~~



### 2.4 补充 destroyed 生命周期

加分项,  destroyed生命周期的另一个应用. 将电脑的网络设置为低速3G 就可以测出来.

```js
<body>
    <div id="app">
        <global-child1 v-if="flag"></global-child1>
        <global-child2 v-else></global-child2>

        <button @click="flag=!flag">切换组件1/2</button>
    </div>
</body>

const app = Vue.createApp({
        data() {
            return {
                msg: '123',
                flag: true
            }
        },
        methods: {

        },
        components: {
            globalChild1: {
                template: `<div>我是全局组件1</div>`,
                data() {
                    return {
                        controller: new AbortController()
                    }
                },
                mounted() {
                    axios.get('https://api.i-lynn.cn/college', {
                        signal: this.controller.signal
                    }).then(function (response) {
                        //...
                    }).catch(e => {
                        console.log(e);
                    })
                },
                unmounted() {
                    // 取消请求
                    // this.controller.abort()
                }
            },
            globalChild2: {
                template: `<div>我是全局组件2</div>`,
                data() {
                    return {
                        controller: new AbortController()
                    }
                },
                mounted() {
                    axios.get('https://api.i-lynn.cn/college', {
                        signal: this.controller.signal
                    }).then(function (response) {
                        //...
                    }).catch(e => {
                        console.log(e);
                    })
                },
                unmounted() {
                    // 取消请求
                    // this.controller.abort()
                }
            }

        },

    })


    app.mount('#app')
```



## 3、组件间传值（重点)

### 3.1、父传子 (单向数据流)

- 父组件以属性的形式绑定值到子组件身上（传）

- 子组件通过使用属性props接收（收）

- **注意:**  父传子数据的传递为**单向数据流**, 即父组件传递给子组件的数据,在子组件中是只读的,不能修改.
  
  
  
  - props是单向绑定的（只读属性）：当父组件的属性变化时，将传导给子组件，但是反过来不会
  - props属性支持两种常见的写法形式
    - 数组（推荐）
      - 优点：书写简单
      - 缺点：不能设置默认值、数据类型
    - 对象
      - 优点：可以设置数据默认值与数据类型
      - 缺点：写法复杂

**示例代码**:

~~~html
<body>
    <div id="app">
        <child :day='day'></child>
	</div>
</body>

<script src="./js/vue.js"></script>
<script type="text/javascript">
    var child = {
        // props形式一：数组形式
        props: ['day'],
        // props形式二：对象形式
        props: {
            day: {
                default: '日',
                type: String
            }
        },
        template: '<p>星期{{day}}</p>'
    }
    const vm = new Vue({
        el: '#app',
        data: {
            day: '五'
        },
        components: {
            child
        }
    })
</script>
~~~

父传子的三种方式:

**第一种方式:**  使用props 属性, 直接将要传递的参数传递给子组件(在子组件中不能修改父组件传递过来的参数)

```js
    <div id="app">
        <!-- 使用局部组件 -->
        <part-child :username="username"></part-child>
    </div> 
  // 注意: 当子组件中的参数和父组件中传递的参数一致时,则使用子组件自身的数据
   const app = Vue.createApp({
        data() {
            return {
                username: '成龙'
            }
        },
        methods: {
         
        },
        components: {
            partChild: {
                template: `<div>
                             <p>我是局部组件--{{username}}</p> 
                           </div>`,
                data() {
                    return {
                        msg: '456',
                        count: 1000
                    }
                },
                props: ['username'],
                mounted() { 
                 
                }
            }
        }
    })
```

**第二种方式:**  子组件传递一个方法给父组件,然后该方法在父组件中调用将父组件中的数据传递给子组件, 子组件

中的该方法的形参就是父组件中的数据

```js
   <div id="app">
        <!-- 使用局部组件 -->
        <part-child @myevent="getvalue"></part-child>
    </div>
    
    const app = Vue.createApp({
        data() {
            return {
                msg: '123'
            }
        },
        methods: {
            getvalue(fn) {
                // 调用子组件传递过来的参数,参数为父组件中的数据
                fn(this.msg)
            }
        },
        components: {
            partChild: {
                template: `<div>
                             <p @click='transfer'>我是局部组件</p> 
                           </div>`,
                data() {
                    return {
                        msg: '456',
                        count: 1000
                    }
                },
                methods: {
                    partchildMethod(mydata) {
                        console.log(1, mydata);
                    },
                    transfer() {
                        // 将方法partchildMethod 传递给子组件
                        this.$emit('myevent', this.partchildMethod)
                    }
                }
            }
        }
    })
```

**第三种方式:** 通过 $parent  获取父组件的实例对象,然后进而获取父组件中的数据和和方法实现父传子

// 但是 不建议使用, 后期代码维护更新有风险

```js
 在vue中，有一个实例属性也可以实现“父传子”的效果：$parent;
 
```



### 3.2、子传父

- 子组件模版内容中用`$emit()`触发`自定义事件`，`$emit()`方法**至少**有2个参数
  - 第一个参数为自定义的事件名称（不要和内置的事件重名，例如click、change等）
  - 第二个参数为需要传递的数据（可选，可以是任何格式的数据）
- 父组件模板内容中的子组件占位标签上用v-on（或@）绑定子组件定义的自定义事件名，监听子组件的事件，实现通信 

**第一种方式:**

~~~html
 <div id="app">
        <!-- 使用局部组件 -->
        <part-child @myevent="getvalue"></part-child>
    </div>
    
    const app = Vue.createApp({
        data() {
            return {
                msg: '123'
            }
        },
        methods: {
           getvalue(val){	
           // val 就为获取参数
			console.log(val)  
		   }
        },
        components: {
            partChild: {
                template: `<div>
                             <p @click='transfer'>我是局部组件</p> 
                           </div>`,
                data() {
                    return {
                        msg: '456',
                        count: 1000
                    }
                },
                methods: {
                    transfer() {
                        this.$emit('myevent', this.count)
                    }
                }
            }
        }
    })
~~~

**第二种方式**:  父组件传递一个方法给子组件,子组件接收该方法,并且调用该方法, 然后将子组件的数据作为实参传递, 在父组件中执行传递的函数,函数的参数就是子组件中的数据. 



**第三种方法:**  使用ref , 通过在父组件中给子组件设置ref 属性,然后就可以 获取子组件的组件实例对象,然后就可以操作子组件的属性和方法



**父子组件传值案例效果：**

![image-20211013180908355](https://i.loli.net/2021/10/13/jLRkoSYuHi2Dtbl.png)

**演示代码**

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title></title>
	<script type="text/javascript" src="js/vue.js"></script>
</head>

<body>
	<div id="app">
		<!--使用子组件-->
		<publish @transfer='pushitem'></publish>
		<ul>
			<!--使用子组件-->
			<child v-for='(item,index) in list' :data='item'></child>
		</ul>
	</div>
</body>

</html>
<script type="text/javascript">
	const vm = new Vue({
		el: '#app',
		data: {
			msg: '我是父组件数据',
			list: [
				{
					name: "张三",
					content: '1111111'
				},
				{
					name: "李四",
					content: '222222'
				}
			]
		},
		methods: {
			pushitem(m) {
				console.log(m)
				this.list.push(m)
			}
		},
		components: {
			child: {
				props: ['data'],
				template: `<li>
					<h3>评论人:{{data.name}}</h3>
					<p>评论内容:{{data.content}}</p>
				</li>`,

			},
			publish: {
				template: `<p>
							<input type="text" placeholder="评论人" v-model='name'/>
							<input type="text" placeholder="评论内容" v-model='content'/>
							<button @click='pub'>发表</button>
						</p>`,
				data: function () {
					return {
						name: '',
						content: ''
					}
				},
				methods: {
					pub() {
						let obj = {
							name: this.name,
							content: this.content
						}
						//						console.log(obj)
						this.$emit('transfer', obj)
						// 每次点击完，清空输入框
						this.name = ''
						this.content = ''
					}
				}

			}
		}
	})
</script>
```



### 3.3、非父子组件之间的通信

使用状态提升,也就是通过ref链实现数据的通信(该方法一般不用)

```html
<body>
    <div id="app">
        <part-child></part-child>
    </div>
</body>

</html>
<script>
    const app = Vue.createApp({
        data() {
            return {
                msg: '123'
            }
        },
        methods: {

        },
        components: {
            //局部组件 包含了组件1 和组件2
            partChild: {
                template: `<div>
                            <globalChild1 ref='ref1'></globalChild1>
                            <globalChild2 ref='ref2'></globalChild2>
                           </div>`,
                data() {
                    return {
                        username: 'partChild'
                    }
                }
            },
        }
    })

    // 组件1
    app.component('globalChild1', {
        template: `<div>全局组件1</div>`,
        data() {
            return {
                // msg: "全局组件",
                nameArr: ['张三', '李四']
            }
        }
    })
	//组件2
    app.component('globalChild2', {
        template: `<div>
                        <p>全局组件2</p>
                   </div>`,
        data() {
            return {
            }
        },
        mounted() {
            this.$parent.$refs.ref1.nameArr.push('王五')
            // 使用ref 链找到父组件然后再早下面的子组件()
            console.log(this.$parent.$refs.ref1);
        }
    })

    app.mount('#app')
```



### 3.4、兄弟组件间传值

> EventBus又被称之为**中央事件总线**
>
> 在vue2中可以提供我们去创建中央事件总线 ,使用 new Vue(),然后再利用this.$emit 和 this.$on 来发送和接收参数.实现兄弟组件之间的传参.
>
> 在vue3中移除了中央事件总线，我们不可以再这么用了，，，但是官方给我们推荐了外部第三方的库来帮我们完成事件总线，官方推荐了两个： [mitt](https://github.com/developit/mitt) 或者 [tiny-emitter](https://github.com/scottcorgan/tiny-emitter) 

![事件中心](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/297ffa8474e3e1ba1b392c422284a9889d9181f8.png?sign=b6a91e2548fd10fabc71f4adf538d61d&t=5f3a3491)

**核心步骤**

- 建立事件中心

  - ```js
    // 需要借助第三方的插件
    // npm i mitt
    // 引入到页面中   <script src="./node_modules//mitt/dist/mitt.umd.js"></script>
    const emitter = mitt()
    ```

- 传递数据

  - ~~~javascript
     emitter.emit('自定义事件', 传递的数据)
    ~~~

- 接收数据

  - ~~~javascript
    emitter.on('transfer', data => {console.log(data) // data 就是接收到的数据 })        
    ~~~

- 销毁事件中心

  - ~~~javascript
      emitter.off('自定义事件')  //
    ~~~

使用案例:

```js
<head>
    <script src="./node_modules/vue/dist/vue.global.js"></script>
    <script src="./node_modules//mitt/dist/mitt.umd.js"></script>
</head>
<body>
    <div id="app">
        <global-child1></global-child1>
        <global-child2></global-child2>
    </div>
</body>
<script>
    //01:创建一个中央事件总线对象  
    const emitter = mitt() 
    const app = Vue.createApp({
        data() {
            return {

            }
        },
        components: {
            globalChild1: {
                template: `<div @click='emitFn()'>我是全局组件1--{{msg1}}</div>`,
                data() {
                    return {
                        msg1: '八戒'
                    }
                },
                methods: {
                    // 使用中央事件总线去发送数据
                    emitFn() {
                        emitter.emit('transfer', { name: this.msg1 })
                    }
                }
            },
            globalChild2: {
                template: `<div>我是全局组件2</div>`,
                data() {
                    return {
                        msg2: '悟空'
                    }
                },
                mounted() {
                    // 使用中央事件总线去接收数据
                    emitter.on('transfer', data => {
                        // data 参数就是数据
                        console.log(data);
                    })
                },
                unmounted() {
                    emitter.off('transfer')  //关闭
                }
            }
        }
    }).mount('#app')

</script>
```



### 3.5、跨级组件传参

 `provide` 和 [`inject`](https://cn.vuejs.org/api/options-composition.html#inject) 通常成对一起使用，使一个祖先组件作为其后代组件的依赖注入方，无论这个组件的层级有多深都可以注入成功，只要他们处于同一条组件链上。 



 `provide` 选项应当是一个对象或是返回一个对象的函数。这个对象包含了可注入其后代组件的属性。 

注意: 默认提供的数据不是响应式,为了实现响应式 可以使用计算属性包裹 提供的数据.

 inject  用于声明要通过从上层提供方匹配并注入进当前组件的属性。 

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./node_modules/vue/dist/vue.global.js"></script>
</head>
<body>
    <div id="app">
        <global-child1></global-child1>
        <button @click="msg='观世音大士'">修改msg</button>
    </div>
</body>
</html>
<script>
    // 注意:不使用计算属性的话,那么提供的数据就不是响应的数据了. 
    const { computed } = Vue;
    const app = Vue.createApp({
        data() {
            return {
                msg: '唐僧'
            }
        },
        // 为后代组件提供数据
        provide() {
            return {
                msg: computed(() => this.msg)  //适应计算属性包裹
            }
        },
        components: {
            globalChild1: {
                template: `<div>
                               <div>我是全局组件1--{{msg1}}</div>
                               <global-child2></global-child2>
                          </div>`,
                data() {
                    return {
                        msg1: '八戒'
                    }
                },
                components: {
                    globalChild2: {
                        template: `<div>我是全局组件2-{{msg2}}---{{msg}}</div>`,
                        data() {
                            return {
                                msg2: '悟空'
                            }
                        },
                        inject: ['msg'],  //provider 组件提供数据注入到这里
                        mounted() {

                        }
                    }
                }
            }
        }
    })
    // 如下config 配置就是为了调试时出现报警警告,使用此方法可以解决该警告
    app.config.unwrapInjectedRef = true;
    app.mount('#app')
</script>
```



### 3.6、ref

父取子的数据信息。（方向：子-父，但是区别于之前的子传父，之前是主动，现在是被动）

`ref`属性被用来给元素或子组件注册引用信息，引用信息将会注册在父组件的 `$refs` 对象上。如果在普通的 DOM 元素上使用`ref`属性，则引用指向的就是 DOM 元素；如果`ref`属性用在子组件上，引用就指向子组件**实例**。

- `ref`放在标签上，拿到的是原生节点。`ref`放在组件上 拿到的是组件实例
- 原理：在父组件中通过`ref`属性（会被注册到父组件的`$refs`对象上）拿到组件/DOM对象，从而得到组件/DOM中的**所有的信息**，也包括值

~~~html
<!-- 普通DOM  拿到的是原生节点-->
<p ref="p" @click='handleClick'>hello</p>
<!-- 子组件 拿到的是组件实例-->
<child-comp ref="child"></child-comp>

<script>
new Vue({
    el: '#app',
    data: {

    },
    mounted: function(){
        console.log(this.$refs.p);
        console.log(this.$refs.child);
        this.$refs.comp.msg = '123' // 修改值
    },
     methods:{
        handleClick(){
            console.log(this.$refs.child)
        }
    },
    omponents: {
        childComp: {
            template: `我是子组件`,
            data() {
                return {
                    mydata: '我是子组件'
                }
            }
        }
    }
})
</script>
~~~

> 注意：
>
> `ref`属性这种获取子元素/组件的方式虽然写法简单，容易上手，但是其由于权限过于开放，不推荐使用，有安全问题。（不仅可以获取值，还可以获取其他所有的元素/组件的数据，甚至可以修改这些数据。）



## 4、动态组件

通过使用保留的 `<component> `元素，动态地绑定到它的` is` 特性，==我们让多个组件可以使用同一个挂载点（位置），并动态切换。==

**示例代码**

~~~html
<body>
    <div id="app">
        <keep-alive>
    		<component :is="currentView"></component>
        </keep-alive>
    </div>
</body>

<script src="./js/vue.js"></script>
<script>
    // 多个组件
    var home = {}
    var posts = {}

    var vm = new Vue({
        el: "#app",
        data: {
            currentView: "home",
        },
        components: {
            home,
            posts,
        },
    });
</script>
~~~

> **keep-alive**的作用：
>
> `keep-alive`可以将已经切换出去的非活跃组件保留在内存中。如果把切换出去的组件保留在内存中，可以保留它的状态，避免重新渲染。

**案例：使用动态组件实现简易的步骤向导效果**

![简单步骤向导效果](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/74c301397cf23d1f0c58ce398c3d7fe73352ff44.gif?sign=82eb695c150acf8e3ce3ac7ec7e54009&t=5f5b549b)

**案例参考代码**

~~~html
<head>
    <title>Document</title>
    <!-- <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script> -->
    <!-- <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script> -->
    <script src="./node_modules/vue/dist/vue.global.js"></script>
    <style>

    </style>
</head>
<body>
    <div id="app">
        <button @click='change("step1")'>第一步</button>
        <button @click='change("step2")'>第二步</button>
        <button @click='change("step3")'>第三步</button>
        <keep-alive>
            <component :is="name"></component>
        </keep-alive>
    </div>
</body>

</html>
<script>
    const { createApp } = Vue;
    const app = createApp({
        data() {
            return {
                name: 'step1'
            }
        },
        components: {
            step1: {
                template: '<div>这是第一步的操作</div>',
                activated() {
                    console.log('step1-1');
                },
                created() {
                    console.log('created');
                },
                deactivated() {
                    console.log('step1-2');
                },
                unmounted() {
                    console.log('unmounted');
                }
            },
            step2: {
                template: '<div>这是第二步的操作</div>',
                activated() {
                    console.log('step2-1');
                },
                deactivated() {
                    console.log('step2-2');
                }
            },
            step3: {
                template: '<div>这是第三步的操作</div>',
                activated() {
                    console.log('step3-1');
                },
                deactivated() {
                    console.log('step3-2');
                }
            }
        },
        methods: {
            change: function (name) {
                this.name = name
            }
        }
    })
    app.mount('#app')
</script>
~~~

> 在动态组件中存在2个生命周期函数（需要配合keep-alive标签）：
>
> ​              activated：激活缓存组件的时候被触发
>
> ​              deactivated：离开缓存组件的时候被触发
>
> ​            当使用了keepalive组件后，组件在切换的时候就不会被销毁，而是被缓存起来了。【此处需要注意生命周期相关的执行情况】
>
> 上述2个周期函数与销毁的2个周期函数如果都存在，则只会激活其中的一对（要么激活系列，要么销毁系列，可以看作激活系列是销毁系列的替代）。
>
> 如果使用了keepalive，则只有第一次渲染的时候会走前4个生命周期函数，后续再激活组件的时候，前四个周期就不会再产生触发效果。(因为不会再重新创建组件了,组件被缓存了)



## 5、插槽

插槽也是组件传值的一种方式。

就是在使用组件的时候,将一些内容写在组件的标签内部, 但是因为使用组件的部分不是html而是组件或者应用的模板vue会编译模板为虚拟dom之后,再次渲染到页面中, 在编译的过程中不知道该讲这样的内容渲染到哪里,索性就不会渲染,总之, 如果我们不作处理的话, 组件标签内部写的内容是看不到的.

这时候,  需要组件配合, 需要指定是否要渲染这部分的内容, 还有就是要渲染到哪里, 这个时候就要用到slot插槽
也就是在组件的模板中利用slot标签才指定要将组件使用时标签内写入的内容插入到哪个位置. 也就是在组件的模板中利用slot标签才指定要将组件使用时标签内写入的内容插入到哪个位置.



组件的最大特性就是`重用`，而用好插槽能大大提高组件的可重用能力。

![小霸王](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/ccc81e4e187bb6ac614d3d69724cb4c5342fed73.jpeg?sign=b0c53aaaa69c1bdfe25e76e303c54362&t=5f5b5665)

**插槽的作用：**父组件（卡）向子组件（游戏机）传递内容。【插槽应该在子组件上】

![父组件向子组件传递内容](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/c5ddf742613c5886ff140e5d381f9ff76a803d8b.jpeg?sign=59bc2dccbbaf747f12c649b7c17d9415&t=5f3a3981)

通俗的来讲，**插槽无非就是在`子组件`中挖个坑，坑里面放什么东西由`父组件`决定。**（父-子）

插槽类型有：

- 单个（匿名）插槽
- 具名插槽
- 作用域插槽

### 5.1、匿名插槽 (2.6.0后更新)

> 匿名插槽一般就是使用单个插槽

**示例代码**

~~~html
<body>
    <div id="app">
        <!-- 插槽内容 -->
        <alert-box>Something bad happened.</alert-box>
    </div>
</body>

<script src="./js/vue.js"></script>
<script type="text/javascript">
    Vue.component("alert-box", {
        template: `
                <div class='demo-alert-box'>
                    <strong>Error:</strong>
                    <slot></slot>
                </div>
		`
    });

    const vm = new Vue({
        el: "#app",
    });
</script>
~~~

> 注意：子组件的`slot`标签中允许书写内容，当父组件不往子组件传递内容时，`slot`中的内容才会被展示出来。



### 5.2、具名插槽 (2.6.0后更新) 

`slot` 元素可以用一个特殊的特性 `name` 来进一步配置如何分发内容。多个插槽可以有不同的名字，具名插槽将匹配内容模板中有对应 `v-slot` 指令名对应的内容

 `v-slot` 有对应的简写 `#`，因此 ` 可以简写为 `  <template #header> 

**`上中下`形式网页布局示例代码**

~~~html
<body>
    <div id="app">
        <app-layout>
            <template v-slot:header>
                <h1>头部</h1>
            </template>
            <template v-slot:default>
                <p>主体1</p>
                <p>主体2</p>
            </template>
            <template v-slot:footer>
                <p>尾部</p>
            </template>
        </app-layout>
    </div>
</body>

</html>
<script>
    const app = Vue.createApp({
        data() {
            return {

            }
        },
        methods: {

        },
        components: {
            appLayout: {
                template: `<div class="container">
                            <header>
                                <slot name="header"></slot>
                            </header>
                            <main>
                                <slot></slot>
                            </main>
                            <footer>
                                <slot name="footer"></slot>
                            </footer>
                        </div>`,
                data() {
                    return {
                        count: 100
                    }
                }
            }
        }
    }).mount('#app')
~~~

> 具名插槽存在的意义就是为了解决在单个页面中同时使用多个插槽。



### 5.3、作用域插槽 (2.6.0后更新)

**应用场景：**父组件对子组件的内容进行加工处理

作用域插槽是一种**特殊类型**的插槽，**作用域插槽会绑定了一套数据，父组件可以拿这些数据来用**，于是，情况就变成了这样：样式父组件说了算，但父组件中内容可以显示子组件插槽绑定的数据。

**示例代码**

~~~html
<body>
    <div id="app">
        <child>
            <template v-slot='slotProps'>
                <div id='1'>
                    {{slotProps.userinfo.name}}--{{slotProps.userinfo.age}}
                </div>
            </template>
        </child>
    </div>
</body>

</html>
<script>
    const app = Vue.createApp({
        data() {
            return {

            }
        },
        methods: {

        },
        components: {
            child: {
                template: `
                    <div>
                        <p>我是作用域插槽</p>
                        <slot :userinfo='userinfo'></slot>
                    </div>`,
                data() {
                    return {
                        userinfo: {
                            name: "旺财",
                            age: 10
                        }
                    }
                }
            }
        }
    }).mount('#app')
</script>
~~~

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/08/27c279c5c2c07acefa4b7ec07a2acf64299d355f.png?sign=6948c21a258d740a9aa42ca003c89f4c&t=612e0c13)



# 七、vue 工程化开发

vue command line tool，简单的来讲，就是一个基于命令行的vue开发工具。

**Vue-CLI ≠ Vue**，Vue-CLI就是一个Vue工具。

vue脚手架工具

## 1、单文件组件

在很多 Vue 项目中，我们使用 app.component 来定义全局组件，紧接着用 new Vue({ el: '#container '}) 在每个页面内指定一个容器元素。这种方式在很多中小规模的项目中运作的很好，在这些项目里 JS 只被用来加强特定的视图。但当在更复杂的项目中，或者你的前端完全由JS驱动的时候，下面这些缺点将变得非常明显：

- 所有的组件都放同一个html文件中
- 没有构建步骤(build操作)，不能使用npm来管理项目
- 不能进行代码的打包压缩
- 没有针对单个组件的css样式支持
- 不能使用less/sass 这些css 预编译工具对css 进行高效编写 
- 不方便使用大量的模块化导入导出语句,提升开发效率
- 创建一个本地的服务器, 可以使代码在开发阶段在本地服务器上运行

针对于上述的问题，vue框架发布了`vue-cli`项目`生成`工具，Vue-cli是一个基于 Vue.js 进行快速开发的完整系统， 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。

![单文件组件](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/06/49ef84aa819fb546d90fbc68bdf652d5ef883ab3.png?sign=700ff63c1d28e5bf1b0fdd612a30ac6d&t=60daccd0)

单文件组件的要求：

- 后缀必须是“.vue”
- 需要使用三个标签将整个文件分成3部分
  - template标签：包裹的是html部分（视图部分）【必须要有的】
  - script标签：包裹的是JavaScript部分（逻辑部分）【必须要有的】
    - css-in-js：在JavaScript中写样式
  - style标签：包裹的css/scss/less等样式部分（样式部分）【可以没有】
    - 样式存在范围的问题的
      - 有“scoped”属性则表示该组件的样式代码只在当前组件生效
      - 如果没有“scoped”属性则表示该组件的样式会影响自己及后代，一般在工程化开发的模式中，只有根组件“App.vue”不写“scoped”属性（全局样式）
- 其他的语法与之前的一致
- 单文件组件只是工程化中的一个文件，无法单独运行，必须在项目中运行



## 2、工具安装

网址：http://npmjs.com

~~~shell
## 安装
# -g：全局安装
npm init vue@latest

## 安装成功后，检查
vue --version
vue -V
#  Vue和VueCLI是两回事

## 卸载（了解）
npm uninstall -g @vue/cli
~~~

![版本检查](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/0a637b8f10a340665538af6f079480dabf686f21.png?sign=d4b0bcdfcda0c91a18b1212f24892c5d&t=5f63af2f)

> 如果需要安装其他版本，可以使用`npm install -g @vue/cli@版本号`的方式进行指定版本。

如果最新版安装不成功，可以尝试以下几种方式去解决：

- 断网，使用热点共享流量去执行安装命令
- 安装其他版本
- 切换一下npm镜像源，切换成淘宝镜像
- 卸载nodejs重安装
- 重装系统/换电脑



## 3、vue2与vue3脚手架的区别

- vue2 使用的脚手架是webpack,
- vue3使用的脚手架是 vite, vite 是vue 团队出的
- 相比而言, vite 更新更快比webpack



## 4、目录结构介绍

- public：公共的文件夹, 里面存放的是浏览器访问的入口文件（index.html）

- src（**后期很多操作都在这个目录中完成**）
  - main.js：项目/程序入口文件 （该文件中JavaScript代码都会被执行）,
  - App.vue：根组件（万物之根）
  - components：存放自定义的`功能`组件（涉及到业务逻辑）
  -  vite.config.js :   可以对vite 这个脚手架工具实现二次配置
  - assets：静态资源目录（图片、视频、音频等就是静态资源），这里面的静态资源浏览器是无法直接访问的，而是给组件通过模块化的方式导入进组件使用的。
    - 项目中的静态资源有2个地方可以放
      - public：供在public/index.html中直接引入（link标签、script标签）的
      - src/assets：供单文件组件导入时需要的静态资源文件（import ...）
  - **views：（当前是没有的，但是后期要用）存放`视图`组件的**（只是涉及到页面的布局排版）

如何很好的划分功能组件与视图组件呢？

小技巧：可以被复用的就算它功能组件，不能被复用的就算它是视图组件。



补充：（readme.md文件中的内容）后续入职的时候项目给到的代码可能不不包含node_modules目录，需要自己执行`npm i`，随后项目才完整。



## 5、项目的运行及注意事项

### 5.1、项目的启停

![创建成功](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/74b3f1f5a30596606c16b93fc24ab018e6e98487.png?sign=531f60f30c89e95c6718fad45ee248d0&t=5f6728ae)

如上图所示，在创建项目完成后有提示我们后续的操作：

- 在命令行中进入项目目录
- 运行`npm run serve`命令来启动项目

按照上述命令执行后，我们会见到如下的效果，即表示项目运行成功：

![项目启动成功](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/1335f9133e19d0a1d6b2f629bc8b7bde9a0868d8.png?sign=5b56d2e029b8969783fd1c82dc299261&t=5f683757)

>  注意：默认端口号会从8080开始，如果再次启动其他项目后续会以8081、8082……进行监听。

如果需要停止正在运行的项目，可以选择以下两种方式任一：

- 关闭终端
- 在终端中按下组合键`Ctrl + C`（Cancel），随后选择`Y`并键入`回车`（如下图）
- 也可以按下两次`Ctrl + C`

![关闭项目运行](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/987aba14adc55be552add382c8c565d82dbf5b90.png?sign=ffaad83e946c9b1205a489388dc64adc&t=5f68396c)

> 部分同学的机器在启动vue项目的时候可能会出现卡在“40%”的进度并且长时间不动，如果这样，则直接`Ctrl + C`停止本次启动，重新再去尝试启动。

==关于项目运行时，如果修改了项目代码是否需要重启的说明：==

是否需要重启取决于我们修改了什么内容，如果只是修改了代码部分（js、css、vue文件等）是不需要开发者手动重启项目的，系统会自动重新编译（有点nodemon感觉）；**但是如果修改的是配置文件，则必须需要自己先去停止项目，然后再去启动项目（手动实现重启）。**



### 5.2、关于ESlint

1. ESlint用于规范项目的编码，大型项目一般多人开发，为了避免一些个人编程恶习`坑自己坑别人`，项目中使用了ESlint会起到`紧箍咒`的作用，强制开发人员注意代码规范。例如，在不使用ESlint的情况下，JS允许我们声明一个不变量但不使用。如果使用了ESlint，在上述情况下会报错如下：

![eslint报错演示](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/b83ccf704842a3a495c99aba44baa772ede48a7b.png?sign=787cecc32474ee8d85b6b6a6ffd1116c&t=5f6838c1)

关于ESlint的报错，有一份错误参照，可以访问以下地址查看：https://cn.eslint.org/docs/rules/

在前期学习阶段不建议去使用ESlint，所以待会会重新创建一个不带有`eslint`的项目来学习路由的使用。但是，以后企业中开发项目的时候都会启用eslint。

2.开发阶段关闭eslint

- 在项目根目录创建vue.config.js 文件，配置如下：

- ```js
  module.exports = {
      lintOnSave: false// eslint-loader 是否在保存的时候检查
  }
  ```

  

### 5.3、环境变量

环境变量=配置文件

问题：此处的配置文件与之前模块化拆分出来的config.js文件有啥区别？

答：虽然都能实现配置，但是还是有区别的。区别在于，config.js文件是模块化产物，在使用的时候需要导入；而此处的环境变量的配置文件是属于系统性质的文件，不需要（我们自己）导入，系统在运行的时候自动引入。

步骤：

①在vue中是支持环境变量的使用的，但是其默认没有给我们提供环境变量配置文件，需要自己创建：

​	a. 环境变量配置文件创建的位置在：**项目的根目录下**

​	b. 环境变量的配置文件名称固定：

   1. `.env`：该文件里放的是全局的环境变量的配置（该文件中的配置项在任何的时候都会被使用）

      

   2. `.env.development`：该文件里放的是开发环境下才使用的配置（该文件中的配置只有在运行`npm run serve`的时候会被加载）

         

      				3.  `.env.production`：该文件里放的是生产环境下才使用的配置（该文件中的配置只有在运行`npm run build`的时候才会被加载）

​    c. 三个文件的名称千万不能写错，我们也不用自己去考虑应该导入哪个，全是系统自动判断

4. 在上述三个配置文件中，配置项的名字必须以`VUE_APP_`开头，如果不是，则该配置项不会生效

5. 如果有一个配置项，例如`VUE_APP_NAME`同时存在于上述三个配置文件中，优先级是怎么样的？

     如果是开发模式（npm  run serve）：.env.development > .env

     如果是生产模式（npm run build）：  .env.production > .env

②在代码中去获取配置项的值采用以下语法：

~~~js
process.env.VUE_APP_XXXX_YYYY
~~~

③如果配置文件中的某配置项暂时性的不想要，则可以采用注释的方式临时取消，注释符是`#`



# 八、路由

## 1、路由的概念

路由的本质就是一种`对应关系`（此处的路由含义同之前nodejs的路由），根据不同的URL请求，返回对应不同的资源。那么url地址和真实的资源之间就有一种对应的关系，就是路由。

路由分为：`后端路由`和`前端路由`

- 后端路由：由服务器端进行实现并实现资源映射分发（nodejs、jsp、php等）
  - 概念：根据不同的用户URL请求，返回不同的内容（**地址与资源**产生对应关系）
  - 本质：URL请求地址与服务器资源之间的对应关系（映射）

![后端路由](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/a832ee01e808edae5e035257d6d9b65411ca0142.jpeg?sign=2a588882a568365ae19f525571bce452&t=5f3e3ebf)

- 前端路由：根据不同的**事件**来显示不同的页面内容，是事件与事件处理函数之间的对应关系
  - 概念：根据不同的用户事件，显示不同的页面内容（**地址与事件**产生对应关系）
  - 本质：用户事件与事件处理函数之间的对应关系

![前端路由](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/2bddc42178c6449942b3dfe53187a890a20f4c9f.png?sign=c895f41a55db1f871fee8c98db306c8e&t=5f3e41f2)



记住一句话：有请求就应该有响应，只不过区别在于，之前node是响应资源，现在在前端中通过事件来进行响应。



## 2、前端路由实现

> 面试题：请你说出前端路由是怎么实现的？或者有哪几种实现方式？
>
> 答：前端路由模式有两种实现方式：hash方式、history方式。

核心思想：通过**监听**地址栏的变化**事件**来实现资源的动态显示

前端路由有2种模式：

- hash模式

>  hash路由模式是这样的：http://xxx.abc.com/#/abx。 有带#号，hash值为 #/abc，它不会向服务器发出请求，因此也就不会刷新页面。并且每次hash值发生改变的时候，会触发hashchange事件。因此我们可以通过监听该事件，来知道hash值发生了哪些变化。

~~~javascript
window.addEventListener('hashchange', ()=>{
  // 通过 location.hash 获取到最新的 hash 值
  console.log(location.hash);
});
~~~

**hash路由原理：**

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title></title>
	<script type="text/javascript" src="js/vue.js"></script>
</head>
<body>
	<a href="#/a">去a页面</a>
	<a href="#/b">去b页面</a>
	<a href="#/c">去c页面</a>
	<a href="#/d">去d页面</a>
	<!-- 存放对应的页面 -->
	<div id="route-view"></div>
</body>
<script type="text/javascript">
	// hash 是指 url 中锚的部分，即（锚点链接）也就是从#开始的部分
	// 如： http://www.runoob.com/test.htm＃part2    输出 ＃part2 即为hash值
	// 获取元内容素
	var ctn = document.getElementById('route-view')
	// 默认渲染
	render('#/a')

	// 监听hashchange事件
	window.addEventListener('hashchange', function () {
		render(location.hash)
	})

	// 分支
	function render(router) {
		switch (router) {
			case '#/a':
				ctn.innerHTML = '这是a页面'
				break;
			case '#/b':
				ctn.innerHTML = '这是b页面'
				break;
			case '#/c':
				ctn.innerHTML = '这是c页面'
				break;
			case '#/d':
				ctn.innerHTML = '这是d页面'
				break;
			default:
				ctn.innerHTML = '404页面'
				break;
		}
	}
</script>
```

- history模式

> 形如：http://xxx.abc.com/xx/yy/zz。HTML5的History API为浏览器的全局history对象增加了该扩展方法。它是一个浏览器（bom）的一个接口，在window对象中提供了popstate事件来监听历史栈的改变，只要历史栈有信息发生改变的话，就会触发该事件。

~~~javascript
// 语法: history.pushState({},title,url); // 向历史记录中追加一条记录
// 页面部分:
<ul>
    <li onclick='goto({name:"home"},"","home")'>首页</li>
    <li onclick='goto({name:"category"},"","category")'>分类</li>
</ul>
<router-view></router-view>

// js部分:
	// 点击首页/分类事件
    function goto(a, b, c) {
        history.pushState(a, b, c)
        if (a.name == 'home') {
            document.querySelector('router-view').innerHTML = '首页'
        }
        if (a.name == 'category') {
            document.querySelector('router-view').innerHTML = '分类'
        }
    }
	// 监听浏览器前进和后退按钮时触发
    window.addEventListener('popstate', function(e) {
        // console.log(e.state)
        if (e.state.name == 'home') {
            document.querySelector('router-view').innerHTML = '首页'
        }
        if (e.state.name == 'category') {
            document.querySelector('router-view').innerHTML = '分类'
        }

    })
~~~

>注：浏览器地址没有#， 比如(http://localhost:3001/a); 它也一样不会刷新页面的。但是url地址会改变。**但它在服务器没有配置的情况下，不能手动刷新，否则返回404页面**



## 3、Vue Router

网址：https://router.vuejs.org/zh/，vuerouter是vue全家桶之一。

**此处建议创建一个不带`ESlint`、带Router的vue项目。**

### 3.1、介绍

**Vue Router 是 Vue.js 官方的路由管理器**。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由（套娃，父子路由）

- **模块化**的、基于组件的路由配置

- 路由参数、查询、通配符

- 带有自动激活（默认选中效果）的 CSS class 的链接

- HTML5 历史模式或 hash 模式




### 3.2、安装

如果在vue-cli创建项目时没有勾选上`vue-router`选项，此时就需要手动的来安装它（**切记，要进入项目中再去运行这个指令**）：

~~~shell
npm install vue-router@4
~~~

查看是否安装成功，查看此文件`/package.json`

![vue-router安装成功检查](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/da67726d850713a81b814a877c862431a1740a5c.png?sign=f20b3897f66ca81281c00986e18faee3&t=5f67297b)



### 3.3、Vue Router基本使用（了解）

Vue Router的基本使用步骤：

- 在src/创建路由文件的归档目录“router”
- 引入相关库文件
- VueRouter引入到Vue类中
- **定义路由组件规则**
- 创建路由实例
- 把路由挂载到Vue根实例中
- **添加路由组件渲染容器（router-view，组件）到对应组件中（占坑）**
  - 情况1：放在根组件中
  - 情况2：放在嵌套路由中的父组件中

~~~javascript
// 引入相关库文件
import { createRouter, createWebHistory } from 'vue-router'

// 创建路由实例对象
const router = createRouter({
  history: createWebHistory(), // 执行路由模式 hash/history模式
  // 定义留有规则
  routes: [
    {
      path: '/',
      redirect: '/home'
    },
    {
      path: '/home',
      name: 'home',
      component: HomeView
    },
    {
      path: '/category',
      name: 'category',
      component: () => import('../views/CategoryView.vue')
    },
    {
      path: '/car',
      name: 'car',
      component: () => import('../views/CarView.vue')
    },
    {
      path: '/mine',
      name: 'mine',
      component: () => import('../views/MineView.vue')
    }
  ]
})

export default router

// 暴露router让外界使用（默认导出，一个模块只能默认导出1次）
export default router

// =====================================
// 将路由对象挂载到根实例（main.js）, 从而让整个应用都有路由功能
const app = createApp(App)
app.use(router)
app.mount('#app')

<!-- html，添加路由组件渲染容器 -->
<div id="app">
<template>
  <header>
    <!-- <globalHeader></globalHeader> -->
    <!-- 声明式导航 -->
    <p>
      <router-link to="/home">首页</router-link>
      <router-link to="/category">分类</router-link>
      <router-link to="/car">购物车</router-link>
      <router-link to="/mine">我的</router-link>
    </p>
  </header>

  <!-- 一级路由的坑 -->
  <router-view></router-view>
</template>    
</div>


~~~

> - 后期在项目中以`index`命名的文件在引入时，可以省去文件名不写。
> - 在`import`的时候如果涉及到了路径，建议写`@`符号开头的路径（`@表示src目录`，这个操作是打包工具已经帮我们定义好了，vue-cli的功劳，后续webpack再去说明）
> - 名称的规范：
>   - 在`创建路由实例`的时候要去其属性名必须是`routes`
>   - 在`挂载路由实例到根实例`的时候要求属性名必须是`router`
>   - 请注意大小写

**示例代码：**

![routes/index.js](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/00810b3416ddd643264759afc7ad02b9d6833a82.png?sign=60800f7e2d3ebbcba55de10b29593aea&t=5f6871ac)

![main.js](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/d98d49a6c9f0358740b11c9b542b37ab51b44897.png?sign=2cf1003937bef9dc2ed1f55e790d3a28&t=5f6871e0)

![app.vue](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/1d97fb78f01c15d2cdaca96b6d110431802e46e8.png?sign=eb5a33ccb1361cb1f2cfcdd8d38b4a83&t=5f68720e)

**src/views/Hello.vue代码**

~~~vue
<template>
    <div>这是hello页面</div>
</template>
~~~

**src/views/News.vue代码**

~~~vue
<template>
    <div>这是新闻页面</div>
</template>
~~~

**实现效果：**

![实现效果](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/2fd0e496a9ce93a7398722534ca24f93abf8cdd0.gif)



### 3.4、路由模式切换

vue-router中默认使用的是hash模式的路由，也就是前面介绍的地址栏中URL带有“#”的形式，如果需要切换成history模式，则可以在创建路由实例时进行指定，指定的配置项为`mode`，可选值：

- hash：**默认**，设置路由模式为hash路由
- history：设置路由模式为history路由

例如，如果我们想设置路由模式从`hash`改变为`history`则可以配置路由入口文件为：

![1666628830830](C:\Users\wusheng\AppData\Roaming\Typora\typora-user-images\1666628830830.png)



### 3.5、导航方式

含义：从一个组件/地址去往另一个组件/地址的方式。

在页面中，导航实现有2种方式：

- 声明式导航：通过点击链接实现的导航方式，例如HTML中的“<a>”标签，Vue中的“<router-link>”所实现的。（其性质与a标签的性质类似）
- 编程式导航：通过调用JavaScript形式API实现的导航方式，例如location.href实现的跳转效果

#### 3.5.1、声明式导航

它就是先在页面中定义好跳转的路由规则，vueRouter中通过router-link组件来完成

~~~html
<router-link to="path">xxx</router-link>
<router-link :to="{path:'path'}">xxx</router-link>
<router-link :to="{name:'name'}">xxx</router-link>
~~~

#### 3.5.2、编程式导航

简单来说，编程式导航就是通过`JavaScript`来实现路由跳转

~~~javascript
this.$router.push("/login");
this.$router.push({ path:"/login" });
this.$router.push({ path:"/login",query:{username:"jack"} });
// 不要将path属性与params属性一起使用，这样会导致params路由参数获取不到
// name属性可以与params属性传参一起使用
this.$router.push({ name:'user' , params: {id:123} }); // vue3不能使用params 传递参数
this.$router.go( n );//n为数字  负数为回退
this.$router.back(); // 返回上一页
~~~

**注意点：**编程式导航在跳转到与当前地址一致的URL时会报错，但这个报错不影响功能：

![重复导航错误](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/ffae8bf7ff5e46907768198ed81ce996be3ee11f.png?sign=fdd95d31c85eedc806e8e5b0817fc555&t=5f68a439)

如果患有强迫症，可以在路由入口文件`index.js`中添加如下代码解决该问题：

~~~javascript
// 该段代码不需要记，理解即可
const originalPush = VueRouter.prototype.push;
VueRouter.prototype.push = function push(location) {
    return originalPush.call(this, location).catch((err) => err);
};
~~~

**面试题问题：`this.$router`与`this.$route`有什么区别？**

答：

`$router`是整个路由实例对象,即(new VueRouter())；`$route`是用户获取当前路由信息的。

页面跳转:使用$router ,获取当前路由信息 使用$route



### 3.6、路由重定向

- 概念：用户在访问地址 A 的时候，强制用户跳转到地址 C ，从而展示特定的组件页面
- 实现： 通过路由规则的` redirect `属性，指定一个新的路由地址，可以很方便地设置路由的重定向

**代码示例**

~~~html
<script type="javascript">
var router = new VueRouter({
    // routes是路由规则数组 
    routes: [
        // 每个路由规则都是个配置对象，至少有path和component两个属性（重定向除外）
        // path表示当前路由规则匹配的hash地址 
        // component表示当前路由规则对应要展示的组件
        { path: '/', redirect: '/user' },
        { path: '/user', component: User },
        { path: '/register', component: Register }
    ]
})
</script>
~~~

> component属性是可选属性，因此在写的时候需要注意，写错了也不会报错的。



### 3.7、嵌套路由（重点）

路由前缀： **/admin/user**/add    **/admin/user**/del   **/admin/user**/mod

相同部分可以**提取**出来，减少重复劳动。

————————————以上为nodejs中的概念————————————————

上述概念在vue中被称之为叫做嵌套路由。

套娃，可以按照之前的nodejs处的路由前缀去理解。（当有些路由有公共部分的前缀时，在vue中就可以使用嵌套路由）

嵌套路由最关键在于理解子级路由的概念：

比如我们有一个`/users`的路由，那么`/users`下面还可以添加子级路由，如:`/users/index`、`/users/add`等等，这样的路由情形称之为嵌套路由。

>  核心思想：在**父路由组件**的模板内容中添加子路由链接和子路由**填充位（占坑）**，同时在路由规则处为父路由配置**children属性**指定子路由规则
>
>  **注意：一级路由需要使用path+query方式跳转，这样才能正常显示该一级路由默认对应的二级路由，使用 name+params 方式不展示二级路由对应的组件**

~~~javascript
routes: [
  { 
      path: "/user", 
      component: User, 	//这个不能丢
      // 通过children属性为/user添加子路由规则
      children:[
          // 子路由地址前不能写“/”，如果写了则表示从根开始
          { path: "", component: Index }, // User组件默认展示的子路由组件，注意 父组件需要使用path跳转而不能使用name 跳转， 否则二级路由默认不展示
          { path: "/index", component: Index },
          { path: "add", component: Add },
      ]
  }
]
~~~

~~~html
<!-- 需要在 User组件中定义一个router-view 用于嵌套路由的渲染显示 -->
<router-view></router-view>
~~~



### 3.8、404路由

作用：用于处理当路由匹配不上的时候页面的展示（不做404路由，则页面显示白板页面）

由于Vue路由是**从上到下执行**的，**只要在路由配置规则中最后面放个*号（通配符）路由就可以了**，例如：

~~~javascript
const routes = [
    { path: "/hello", redirect: Hello },
    { path: "/about", component: About },
    { path: "/news", component: News },
    // 404路由 vue2写法
    { path: "*", component: NotFound },
    // vue3写法
    { path: '/:pathMatch(.*)*', name: 'not-found', component: NotFound },
];
~~~





### 3.9、动态路由参数（重点）

> 本节知识点就是为了restful服务的，看如果在vue中使用restful形式进行**参数传递**。

所谓动态路由就是路由规则中有部分规则是动态变化的，不是固定的值，需要去匹配取出数据（即`路由参数`）。

- 如何传递
  - 在声明路由的时候，将可变部分通过“`:变量名`”的形式进行替代
- 如何获取
- 
  - 获取this.$route来获取

~~~javascript
// 传递参数id
var router = new VueRouter({
    // routes是路由规则数组 
    routes: [
        { path: '/user/:id', component: User },
        // 此处的“:”只是在声明的时候写，在使用的时候不需要写“:”
    ]
})

// 组件视图中获取id值（html-vue形式组件）
const User = {
    template: '<div>User ID is {{$route.params.id}}</div>'
}

~~~

~~~vue
<template>
    <div>
        <!-- 单文件形式的组件, 可以在视图中直接接收路由参数，但是一般不这么用 -->
        这是news组件{{$route.params.id}}
    </div>
</template>
~~~

路由规则中的“:”只是在声明的时候写，在使用的时候不需要写“:”，例如如下代码：

![编程式导航](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/25307732297b5b553fed47d882adcf6f1c48dd0f.png?sign=044378c5358328bb077579da2ac5276e&t=5f69759e)



问题：如上代码，如果路由规则里声明需要传递参数，但是实际使用的时候没传递参数会怎么样？

答：如果声明需要传递参数，但是实际不传的话则会影响落地页的显示，显示成白板（但是不报错）。但是如果有404路由在规则的最后，则匹配404路由。



**注意：在实际开发的时候会有可能需要传参也可能不需要传参的情况，这个时候需要用到`可选路由参数`点。**

定义可选路由参数的方式很简单，只需要在原有的路由参数声明位置后面加上个`?`即可。例如：

~~~javascript
{ path: "showdetail/:id?", component: ShowDetail },
~~~



### 3.10、路由跳转传参的几种方式

声明式导航传参：

```js
// 传固定值
<router-link to="/about/1">About</router-link>
// 传数据
<router-link :to="'/about/' + id">About</router-link>
// 传对象 query方式  path与query 搭配
<router-link :to="{ path: '/test', query: { id: id, name: '张三' } }">About</router-link>
// 传对象 params方式 name与params 搭配
<router-link :to="{ name: 'About', params: { id: id, name: '李四' } }">About</router-link>

// 对应路由中的参数配置(:参数名1/:参数名2...)
{
    path: '/about/:id/:name',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
}
// 对应组件中获取参数
this.$route.params 或 this.$route.query(使用query方式传参)
```

编程式导航传参：

```js
// params 方式传参
this.$router.push({ name: "Test", params: { id: 123 } });
// query  方式传参
this.$router.push({ path: "/test", query: { id: 123, name: "李四" } });

// 对应组件中获取参数
this.$route.params 或 this.$route.query(使用query方式传参)
```



### 3.11、命名路由（可选）

命名路由：路由别名，顾名思义就是给路由起名字（外号）。

例如：阿列克赛·马克西莫维奇·彼什科夫					（高尔基）

通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。

~~~javascript
// 路由
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      name: 'user',
      component: User
    }
  ]
})
~~~

~~~html
<!-- 声明路由 -->
<router-link :to="{ name: 'user', params: { id: 123 }}">User</router-link>
~~~

问：一般什么使用用命名路由？

答：当路由本身的path写法比较长的时候，建议写命名的方式。而且需要注意，如果使用的是path写法，则当path发生变化后，其对应的导航地址也需要跟着变化。但是如果使用了别名则不用理会path内容的变化（只要名不变就没事）。

# 九、路由守卫

## 1. 全局前置守卫

概念： 顾名思义就是在路由跳转前执行一些操作。

你可以使用 `router.beforeEach` 注册一个全局前置守卫： 

```
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

- **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- **`from: Route`**: 当前导航正要离开的路由
- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - **`next()`**: 进行管道中的下一个钩子，通俗得讲就是对该路由进行放行。
  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
  - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

案例: 做一个登录守卫的判断

```js
// 路由前置守卫
router.beforeEach((to,from)=>{
    // 当跳转到非登录页,则判断当前用户是否登录
    if(to.path!='/login'){
        if(!localStorage.getItem('userinfo')){
           return {path:'/login'}
        }
    }else{
      if(localStorage.getItem('userinfo')){
        alert('已经登录,不需要重复登录')
        return {path:from.path}
     }
    }

})
```



##  2. 路由独享的守卫

定义： 顾名思义就是对应得某个路由路径独享得守卫。

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // 这些守卫与全局前置守卫的方法参数是一样的。
        // ...
      }
    }
  ]
})
```

## 3. 组件内的守卫

 你可以在路由组件内直接定义以下路由导航守卫： 

```js
const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```



## 4. 全局后置钩子

 后置和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身,因为触发时间是已经发生了跳转了,跳转完成了

案例: 修改页面标题

由于vue项目是单页应用,所以所有的页面组件展示的标题都一样,有时候咱们需要根据不同得页面组件显示不同的标题

```js
// 以下修改了路由配置表中的路由元信息meta
{
    path: '/home',
    name: 'home',
    meta:{
    title:'首页'
    },
}

// 设置全局后置钩子
// 路由全局后置钩子
router.afterEach((to,from)=>{
  document.title = to.meta.title // 修改页面标题
})

```







# 十、Vuex 全局状态管理

## 1. 为什么使用vuex

### 1.1、介绍

- 如果遇到组件间嵌套层次较多，比较复杂得化，多个组件之间共有一个数据，使用组件传值处理起来得话，就比较麻烦
- vuex 是vue 配套的数据管理工具，我们可以将组件共享数据保存到vuex 中，方便整个程序中得任何组件都可以获取和修改 vuex 中保存得公共数据

## 2.安装vuex

```js
npm i vuex@next --save  // 安装
```

## 3.使用vuex

```js
import { createStore } from 'vuex';
// 创建一个新的 store 实例
const store = createStore({
    state () {
      return {
        count: 0
      }
    },
    mutations: {
      add (state) {
        state.count++
      }
    }
  })

  export default store
```

## 4. vuex 核心概念  

### 4.1、模块介绍

#### state

概念： 提供全局唯一得公共数据源，所有的共享数据都要放在store 中得state 进行存储。 可以理解相当于 组件中的data

- **组件中访问state 中得值：第一种方式：使用常规语法**

  ```js
  //html部分
  <p>count：{{ $store.state.count }}</p>
  // js部分
  this.$store.state.数据名称
  ```

- **组件中访问state 中得值：第二种方式：使用mapState 辅助函数**       

  ```js
  //html部分
  <p>count:{{ count }}</p>
  // js部分
  import {mapState} from 'vuex'  // 从vuex 中按需导入
    computed:{
  	 ...mapState(['count'])，// 将该组件需要的vuex中得全局数据映射到该组件计算属性中
    },
  ```

  

#### mutations

概念：**同步**修改state中的数据, 通过mutations 修改数据虽然繁琐一些，但是可以集中监控所有数据得变化

> **注意： 只能通过mutations 修改store中得数据，不能直接修改store得数据**

```js
const store  = new Vuex.Store({
	state:{ //存放公共数据的
		count:0
	},
	mutations:{ //同步操作state数据
		add(state){ // 不传参 第一个参数永远是state
			state.count++
		},
		addStep(state,step){ // step为传递的参数
			state.count+=step
		}
	}
})
```

- **组件中修改state中的数据----第一种方式：使用常规语法**

```js
this.$store.commit(“add”)  // 不传参
this.$store.commit(“addStep”,10) // 传参
```

- **组件中修改state中的数据----第二种方式：使用mapMutations 辅助函数**

```js
// html 部分
<button @click="add">+1</button>
<button @click="addStep(10)">+10</button>>

// js 部分
import {mapMutations} from 'vuex'
methods:{
    ...mapMutations(['add','addStep']),  // 将vuex 中add和addStep方法映射到组件的methods 中
}
```

#### actions 

概念：可以**异步**修改state 中得数据,

> **注意： action 不能直接修改state中的数据，需要间接通过触发mutation中得方法修改数据。**

```js
const store  = new Vuex.Store({
	state:{ //存放公共数据的
		count:0
	},
    mutations:{
        ...
    }
    actions:{ //可以异步修改state 中的数据
		addAsync(context){ 
			setTimeout(()=>{
				context.commit('add')// 最终还得调用mutations中的同步方法
			},1000)	
		},
		addStepAsync(context,step){
			setTimeout(()=>{
				context.commit('addStep',step) // 最终还得调用mutations中的同步方法
			},1000)	
		}
	},
})
```



- **组件中异步修改state中的数据----第一种方式：使用常规语法**

```js
this.$store.dispatch(“addAsync”)  // 不传参
this.$store.dispatch(“addStepAsync”,10) // 传参
```

- **组件中修改state中的数据----第二种方式：使用mapActions辅助函数**

```js
// html部分
<button @click="addAsync">+1异步</button>
<button @click="addAsyncStep(10)">+10异步传参</button>
//js部分
import {mapActions} from ‘vuex’
methods:{
	...mapActions(['addAsync','addAsyncStep'])// 将vuex 中addAsync和addAsyncStep方法映射到组件的methods 中
}
```

#### getters

概念：用于对store中得数据进行加工处理成新的数据。类似与vue 得计算属性

> **注意：store中得数据发生变化时，则getter 中对应的数据也发生变化。getter不会修改store 中得数据，只是包装**

```js
const store  = new Vuex.Store({
	state:{ //存放公共数据的
		count:0
	},
	mutations:{ //同步操作state数据
	},
    actions:{ // 异步操作state数据
	},
    getters:{
		formatCount(state){ // 相当于组件中的计算属性，对state 中的数据进行处理
			return `包装处理后的${state.count}`
		}
	}
})
```

- **组件中第一种使用方式：常规使用方法**

```js
// html 部分
<p>{{$store.getters.方法名 }}</p>

// js部分
this.$store.getters.方法名 

```

- **组件中第二种使用方式：使用mapGetters辅助函数**

```js
// html部分
 <p>getters处理过的count: {{ formatCount }}</p>

// js部分
import {mapGetters} from 'vuex'
computed:{
	...mapGetters(['formatCount'])  // 将vuex中的getters方法映射到组件中计算属性中
}
```

#### mudules:

概念： 如果项目中使用单一的模块，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿，vuex 允许我们将 store 分割成多个模块（module）。每个模块拥有自己的 state、mutation、action、getter

![1](https://i.loli.net/2021/10/27/LDynfj8hYExIubK.png)

**注意： 分模块开发时，必须给每个模块设置命名空间   `namespaced:true`,** 

```js
//如下是 vuex 拆分出来的 moduleA 模块 
export default{
	namespaced:true,  // 拆分模块必须设置命名空间
	state:{
		num:0
	},
	mutations:{
		addNum(state){
			state.num++
		},
		addNumStep(state,step){
			state.num+=step
		}
	},
	actions:{
		addNumAsync(context){
			setTimeout(()=>{
				context.commit('addNum')
			},1000)
		},
		addNumStepAsync(context,step){
			setTimeout(()=>{
				context.commit('addNumStep',step)
			},1000)	
		}
	},
    getters: {
        fiveNumTimes(state) {
            return state.num * 5
        }
    }
}
```

- **组件中第一种使用方式：常规使用方式**

```js
<!-- 调用模块A的方法 -->
// html
<h3>num:{{$store.state.moduleA.num}}</h3> // 使用的时候，说明是那个模块下的num 变量

<button @click="addNum">+1</button>
<button @click="addNumStep(10)">+10 传参</button>
<button @click="addNumAsync">+1 异步</button>
<button @click="addNumStepAsync(10)">+10 异步传参</button>
//js
methods:{
    addNum(){
         // 同步不传参调用
   	   this.$store.commit('moduleA/addNum') //调用的时候，必须说明那个模块下的addNum方法	
    },
    addNumStep(step){
         // 同步传参调用
   		 this.$store.commit('moduleA/addNumStep',step)
    },
    addNumAsync(){
         // 异步不传参调用
   	    this.$store.dispatch('moduleA/addNumAsync') //调用的时候说明那个模块下的addNumAsync方法
    },
    addNumStepAsync(step){
         // 异步传参调用
   		 this.$store.dispatch('moduleA/addNumStepAsync',step)
    }
}
```

- **组件中第二种使用方式： 使用mapMutations 和mapActions 辅助函数映射。**

```js
<!-- 调用模块A的方法 -->
// html
<h3>num:{{num}}</h3>
<h3>getters处理过的num:{{ fiveNumTimes }}</h3>

<button @click="addNum">模块++</button>
<button @click="addNumStep(10)">模块++</button>
<button @click="addNumAsync">+1 异步</button>
<button @click="addNumStepAsync(10)">+10 异步 传参</button>
// js
computed: {
    // 映射state
    ...mapState("moduleA", ["num"]),
         // 映射getters
     ...mapGetters("moduleA", ["fiveNumTimes"]),
 },
methods:{
		// 需要说明是那个模块中的方法
		...mapMutations('moduleA',['addNum','addNumStep']), // ('模块名'，['方法1'，'方法2'])
		...mapActions('moduleA',['addNumAsync','addNumStepAsync'])
	}
```

### 4.2、vuex数据持久化

vuex 中的数据刷新页面数据会自动消失，这是我们不愿意看到的，那么怎么可以将vuex 中的数据持久化呢？

解决方案： 使用vuex-persistedstate 插件

第一步：在项目中安装插件

```js
npm install vuex-persistedstate --save
```

第二步：使用vuex-persistedstate默认存储到localStorage中

```js
import createPersistedState from "vuex-persistedstate";
const store = newVuex.Store({
 state: {},
 mutations: {},
 actions: {},
 plugins: [createPersistedState()]
})
```

第三步：也可以指定存储到sessionStorage中

```js
import createPersistedState from "vuex-persistedstate"
const store = new Vuex.Store({
   state: {},
   mutations: {},
   actions: {},
   plugins: [createPersistedState({
       storage:window.sessionStorage
   })]
})
```

第四步：也可以指定需要持久存储的state中对应某条数据;

```js
import createPersistedState from "vuex-persistedstate";
const store = newVuex.Store({
 state: {},
 mutations: {},
 actions: {},
 plugins: [createPersistedState({
 storage:window.sessionStorage,
     reducer(val)  {  //val为state
         return {
             // 只储存state中的token 
             assessmentData: val.token
         }
     }
 })]
}）
```



# 十一、Pinia 全局状态管理工具使用

注意: pinia 中没有mutations, 只有actions , 不区分同异步. 

首先需要下载Pinia

```shell
npm install pinia
```

第一步:  在main.js文件中引入 

```js
import { createPinia } from 'pinia' //引入Pinia 并解构出其中的createPinia方法
const pinia = createPinia() // 创建pinia
app.use(pinia) // 全局使用pinia
```

第二步: 在src/store/ 新建userStore.js,每一个业务模块都新建一个对应的  store.js文件,如 goodsStore.js 

```js
import { defineStore } from 'pinia'

export const userStore = defineStore('user', {
    state: () => ({ count: 0 }),
    getters: {
        double: (state) => state.count * 2,
    },
    actions: {
        increment() {
            this.count++
        },
    },
})

```

第三步: 在页面中使用:

```js
<template>
    <div class="home">
        <p>store中的数据 {{ myOwnName }}--{{ double }}</p>

        <p>
            <button @click="myOwnName++">myOwnName++</button>
            <button @click="add">add++</button>
        </p>
    </div>
</template>
<script>
// mapState 辅助函数 将状态属性映射为只读计算属性, 
// 使用 mapWritableState() 代替 就可以修改该属性了.            
import { mapState } from 'pinia';
// userStore 为函数形式
import { userStore } from '../store';
import { mapWritableState } from 'pinia'

export default {
    name: '',
    data() {
        return {
            show: true
        };
    },
    computed: {
        ...mapState(userStore, ['count', 'double']), // 将state中的属性映射到当前组件的实例对象上,该属性只读,不能修改
        ...mapWritableState(userStore, ['count']),// // 将state中的属性映射到当前组件的实例对象上 ,该属性可修改
        ...mapWritableState(userStore, {
            myOwnName: 'count',
        }),
    },
    mounted() {
        console.log('home', this);
        // console.log(1, userStore);
        // axios.get('http://kumanxuan1.f3322.net:8001/index/index').then(res => {
        //     console.log(res);
        // }).catch(e => { })
    },
    methods: {
        add() {
            setTimeout(() => {
                // 修改数据 可以使用 
                //01:第一种方式:不采用映射的方式直接修改   useStore().count++ 直接修改
                //02:第二种方式:采用映射mapState的方式, this.count++ 因为mapstate 为只读属性,
                //所以不可以修改
                //03:第三种方式:采用映射mapWritableState的方式, this.count++可以修改
                //04: 第四种方式如下: 
                 useStore().$patch({
                    count: useStore().count + 1,
                })
            }, 1000)
        }
    }
}
</script>
```





# 十二、 vue3中需要注意的问题

## 5、过滤器 - filter(vue3中已经废弃)

**作用：**格式化数据，比如将字符串格式化为首字母大写、将日期格式化为指定的格式等。

- 过滤器可以定义成全局过滤器和局部过滤器。
- **过滤器的本质就是一个方法**，使用过滤器实际上就相当于方法调用，仅是书写形式上的差异（使用的时候需要用“|”（shift + \），其也可以被称之为  **管道修饰符**）
  - 语法：
    - {{待修饰的数据|过滤器方法名}}
    - {{待修饰的数据|过滤器方法名(参数1,参数2....)}}
    - v-bind:动态属性="待修饰的数据|过滤器方法名"
- 这玩意在vue3中已经废弃了
  - vue3中解决办法是通过methods来替代

![过滤器](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/82ec23d614d0bf55ff0e2ecc1ac9414db2607490.png?sign=019afa9de124d3e56fb0fb0584616f73&t=5f364f40)

**声明语法：**

~~~javascript
// 全局过滤器
Vue.filter('过滤器名称',function(value){ //过滤器不传参
	//过滤器业务逻辑
	return ....
})

// 局部过滤器
el: '#app',
data: {},
filters: {
    过滤器名称1: function(value,a,b){ //过滤器传参ab 为传的参数
        return something
    },
    过滤器名称2: function(value,a,b){ //过滤器传参ab 为传的参数
        return something
    },
    // ....
}
// 案例  
// npm 安装 moment 时间格式插件
Vue.filter('formDate', function(data) {
	return moment(data).format('YYYY-MM-DD') // 将时间格式转换成YYYY-MM-DD
})
~~~

> 过滤器的处理函数中的第一个参数**固定**是`绑定的待处理数据`，后续可以根据需要添加自定义参数



**使用语法：**

~~~html
<!-- 过滤器使用 -->
<div>{{msg | upper}}</div>

<!-- 过滤器允许连续使用，“前 → 后”按顺序执行 -->
<div>{{msg | upper | lower}}</div>

<!-- 过滤器支持在v-bind中使用 -->
<div v-bind:id='id | formatId'></div>

<!-- 过滤器支持传参 -->
<div>{{msg | mysub(1,2)}}</div>
~~~



**案例：声明转字母为大写的全局过滤器和转字母为小写的局部过滤器并使用**

~~~html
<body>
    <div id="app">
        <h4>{{msg | toUpper}}</h4>
        <h4>{{msg | toLower}}</h4>
    </div>
</body>

<script src="./js/vue.js"></script>
<script type="text/javascript">
    // 全局过滤器：转字母为大写
    Vue.filter('toUpper',(val) => {
        return val.toUpperCase()
    })

    const vm = new Vue({
        el: '#app',
        data: {
            msg: 'HeLLo WoRld'
        },
        // 局部过滤器：转字母为小写
        filters: {
            toLower: (val) => {
                return val.toLowerCase()
            }
        }
    })
</script>
~~~



# 十三、vue中的内置组件



## 1、内置组件

### 1、transition组件：

`<transition>` 元素作为**单个**元素/组件的过渡效果。`<transition>` 只会把过渡效果应用到其包裹的内容上，而本身不会渲染成一个 DOM 元素，也不会出现在可被检查的组件层级中。

**transition特点**：

- 内部只能包裹一个元素，如果内部包裹多个元素则使用<transition-group></transition-group>

**transition 属性：**

- appear 属性初始化是否执行动画  值为boolean  true/false
- name  属性 修改动态类名  如：name='abc' 修改前是 v-enter-active   修改后为 abc-enter-active

**transition为包裹元素添加的动态类名有如下几个：**

- v-enter  元素**进场起点**添加的类名，（该时间太短，浏览器控制台捕捉不到）

- v-enter-active  元素在整个**进场过程**中添加的类名
- v-enter-to 元素**进场终点**添加的类名
- v-leave 元素**离场起点**添加的类名 （该时间太短，浏览器控制台捕捉不到）
- v-leave-active  元素在整个**离场过程中**添加的类名 
- v-leave-to 元素在**离场终点**添加的类名

代码如下：

案例： 可以用于后台系统页面切换时的动画

```html
 // html部分
<router-view v-slot="{ Component }">
    <transition name="slide-fade">
      <component :is="Component" />
    </transition>
  </router-view>

// css 
<style scope>
.slide-fade-enter-active {
  transition: all 0.3s ease-in-out;
}

.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
</style>
```



### 2、keep-alive 组件：

- `<keep-alive>` 包裹动态组件时，保留组件的状态，而不需要重新创建。避免多次重复渲染，降低性能，所以可以使用keep-alive 提升页面性能

- 和 `<transition>` 相似，`<keep-alive>` 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在组件的父组件链中。

  代码演示：切换路由A/B/C 每个组件中的input 输入的内容都能缓存

  ```vue
  // App.vue中代码 
  <template>
   <router-link to="/a">A</router-link>
   <router-link to="/b">B</router-link>
   <router-link to="/c">C</router-link>
   <keep-alive>
        <router-view></router-view>
   </keep-alive>
  </template>
  ```

  ```vue
  // A组件
  <template>
    <div>
      A页面
      <input type="text" name="" id="" />
    </div>
  </template>
  <script>
  export default {
    name: "item_A",
    data() {
      return {};
    },
  };
  </script>
  ```

- keep-alive组件属性

  -  include：  只有名称匹配的组件才会被缓存

    ```vue
    <router-link to="/a">A</router-link>
    <router-link to="/b">B</router-link>
    <router-link to="/c">C</router-link>
    <keep-alive include="item_A">
        <router-view></router-view>
    </keep-alive>
    ```

  - exclude：  名称匹配的组件不缓存

    ```vue
    <router-link to="/a">A</router-link>
    <router-link to="/b">B</router-link>
    <router-link to="/c">C</router-link>
    <keep-alive exclude="item_A">
      <router-view></router-view>
    </keep-alive>
    ```

  - 结合router，缓存部分页面

    ```js
    // 设置路由配置
    const router = new VueRouter({
      routes: [
        { path: '/a',
         component: () => import('@/views/a.vue'),
         meta: { keepAlive: true } 
        },
        { path: '/b',
          component: () => import('@/views/b.vue'),
          meta: { keepAlive: false }
        },
        { path: '/c', 
          component: () => import('@/views/c.vue'),
          meta: { keepAlive: false }
        }
      ]
    })
    ```

    ```vue
    // app.vue 
    <router-link to="/a">A</router-link>
    <router-link to="/b">B</router-link>
    <router-link to="/c">C</router-link>
    
    <router-view v-slot="{ Component }">
        <keep-alive :exclude="arr">
          <component :is="Component" />
        </keep-alive>
      </router-view> 
    
    ```
<script>
    export default {
      data() {
        return {

        }
      },
      computed: {
        arr() {
          let newArr = []; // 不缓存的组件名组成的数组
          if (this.$route.meta.iskeepalive == false) {
            // 当路由切换的时候,此时依赖的项发生变化,计算属性重新计算,
            newArr.push(this.$route.name)
          }
          return newArr
        }
      }
    }
    </script>
    ```


​    

# 十四、sass 和less 预编器的使用

sass 和less 都是一门css预编译处理语言,说白了就是方便大家在开发项目时设置样式. 可以使用sass 或less 写样式

#### **01. sass语法:**

第一步: 在项目中下载sass 

```shell
npm i sass
```

第二步: 将项目中的style 设置lang 属性为scss

```css
<style lang='scss'></style>
```

第三步: 语法:

```css
1. 嵌套语法:
.box {
    p {
        background-color: red;
    }
}

2. & 语法:
ul {
    li {
        &:hover {
            color: green;
        }
    }
}

3. 变量 $ (Variables: $)
$w: 100px;
$red: red;
$green: green;
$border2: 2px solid black;

.box {
    width: $w;
    height: 100px;
    background-color: $red;
    border: $border2;

    p {
        background-color: $green;
    }
}
4. @import 导入其他样式文件
   @import "foo.scss";
   @import "foo.css";

5. 定义minxin
@mixin wrap {
    width: 200px;
    height: 200px;
    background-color: deeppink;
}

6. 带参数的minxin
@mixin wrap2($w,$h) {
    width:$w;
    height: $h;
    background-color: deeppink;
}

.bigbox {
    @include wrap; // 使用minxin 函数
    margin: 10px;
}
.bigbox1 {
    @include wrap2(100px,100px); // 使用minxin 函数
    margin: 10px;
}
```



#### **02: less 语法:**

需要下载 less 到项目中;

```shell
npm i less
```

修改style 属性

```css
<style lang='less'>
 // less 语法
</style>
```



## 6、全局方法

### 1、Vue.use()

**用法**：安装 Vue.js 插件。如果插件是一个对象，必须提供 `install` 方法。如果插件是一个函数，它会被作为 	install 方法。install 方法调用时，会将 Vue 作为参数传入。该方法需要在调用 `new Vue()` 之前被调用。

模拟实现一下Element-ui 实现原理

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
// 这样就可以全局使用组件了。elemnt-ui 提供的组件了。
```

- 第一步：在components 目录下创建两个组件 Ubutton.vue 和 Uinput.vue 代码如下：

  ```vue
  <!--Ubutton.vue  -->
  <template>
    <button>按钮</button>
  </template>
  <script>
  export default {
    name: "Ubutton",
    data() {
      return {};
    },
  };
  </script>
  <style scoped>
  /* @import url(); 引入css类 */
  </style>
  ```

  ```vue
  <!-- Uinput.vue -->
  <template>
    <input type="text" name="" id="" placeholder="搜索框" />
  </template>
  <script>
  export default {
    name: "Uinput",
    data() {
      return {};
    },
  };
  </script>
  <style scoped>
  /* @import url(); 引入css类 */
  </style>
  ```

- 第二步：在utils 目录创建一个index.js文件，代码如下：

  ```js
  // 引入对应的组件
  import Ubutton from '@/components/Ubutton.vue';
  import Uinput from '@/components/Uinput.vue';
  // 定义数组存放引入的组件
  const components = [Ubutton, Uinput];
  const ElementUI = {
      install(Vue) { //install是固定写法，写其他方法不识别,参数为Vue构造函数也是固定的
          // 当在main.js 中使用Vue.use(ElementUI) 会自动调用 install 方法
          // 全局注册组件
          components.forEach(item => {
              Vue.component(item.name, item)
          })
      }
  }
  export default ElementUI
  ```

- 第三步：在main.js 中使用，这样就可以在全局使用这两个组件了 而不需要引入

  ```js
  // main.js
  import ElementUI from './utils'
  Vue.use(ElementUI)
  
  new Vue({
    router,
    render: h => h(App),
  }).$mount('#app')
  ```

  

