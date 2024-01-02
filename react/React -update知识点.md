# React

# 一、概述

官网：https://react.zcopy.site/

## 1、介绍

**React起源于Facebook的内部项目**，它是一个用于构建用户界面的javascript库，Facebook用它来架设公司的Instagram网站，并于2013年5月开源。

React拥有较高的性能，代码逻辑非常**简单**，越来越多的人已开始关注和使用它。认为它可能是将来Web开发的主流工具**之一**。

**特点：**

- 声明式

你只需要描述UI看起来是什么样式，**就跟写HTML一样**，React负责渲染UI,(也就是数据驱动视图渲染)

- 基于组件

组件是React最重要的内容，组件表示页面中的部分内容

- **学习一次，随处使用【react原生生态比vue好】**
  - ReactJs：web开发
  - react-native：原生app开发（RN）
  - react360：vr应用

学习react版本是：18.2.0



## 2、开发工具的安装

- Chrome 浏览器扩展插件：https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=zh-CN

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/c1407e83f4b0a56beca4baadaac2de471975a092.png?sign=6007a47eebf330bd919ee70a42746fe7&t=5f8ee1ad)

- 搜索插件迷网站安装对应的浏览器扩展插件 React Developer Tools
- vscode安装react开发扩展

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/4d2506168810f2fb7195cd6b9f766ed998dc365d.png?sign=54df8319890c5709784d4eff96570639&t=5f8ee2a8)



## 3、React初识

目标：实现基于react的hello world的显示。

注意: react 官网给的案例 引入的js 文件为16,但是语法为18,需要修改引入的js为18版本(react.js,react-dom.js)

React开发需要引入多个依赖文件，其中react.js、react-dom.js这两个文件是我们创建react应用程序必须要引入的依赖文件。

- react.js
  - 核心，提供创建元素，组件等功能

- react-dom.js
  -  提供DOM相关功能

[下载](https://reactjs.org/docs/add-react-to-a-website.html)对应的react.js和react-dom.js的开发版本的js类库文件到本机中后，通过HTML的`script`标签引入到当前的网页中，如下（**注意顺序，先引核心文件，再引其他文件**）：

React的第一个程序：

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hello world</title>
    <!-- 
        01: 需要注意引入的文件顺序,先引入react 在引入react-dom 
    -->
    <script src="./js/react.js"></script>
    <script src="./js/react-dom.js"></script>
</head>
<body>
    <div id="root"></div>
    <script>
     // 1. 获取页面的元素
    const domContainer = document.querySelector('#root');
     // 2. 创建一个ReactDOMRoot对象,类似于vue 中的new Vue();该对象有一个render 方法,可以渲染组件
    const root = ReactDOM.createRoot(domContainer);
     // 3. 使用ReactDOMRoot对象去渲染组件
     // root.render(React.createElement('div', { className: 'box', id: 100 }, '我是一个  div'));
    root.render(React.createElement('div', { id: '1' }, React.createElement('p', { id: 2 }, '我是p元素')))
    </script>
</body>

</html>
~~~

```jsx
React.createElement(参数1,参数2,参数3), 该函数的返回值就是一个虚拟dom结构, 一个描述dom结构的js对象
第一个参数是必填，传入的是似HTML标签名称，eg: ul, li
第二个参数是选填，表示的是属性，eg: className
第三个参数是选填, 子节点，eg: 要显示的文本内容
```



# 二、JSX语法

## 1、概述

由于通过React.createElement()方法创建的React元素有一些问题：代码比较繁琐，结构不直观，无法一眼看出描述的结构，不优雅，开发时写代码很不友好。

React使用JSX来替代常规的JavaScript，JSX可以理解为的JavaScript语法扩展，它里面的标签申明要符合**XML规范**要求（**必须需要有一个唯一的根元素**）。React不一定非要使用JSX，但它有以下优点：

- JSX执行**更快**，因为它在编译为JavaScript代码后进行了优化
- 声明式语法更加直观，**与HTML结构相同**，**降低了学习成本**，提升开发效率速

- jsx语法中一定要有一个**顶级元素包裹（XML一大特点）**，否则编译报错，程序不能运行



## 2、JSX重构Hello world

在项目中尝试JSX最快的方法是在页面中添加这个 `<script>` 标签，引入解析jsx语法的babel类库，注意后续的`<script>`标签块中使用了JSX语法，则一定要申明类型`type="text/babel"`，否则babel将不进行解析jsx语法。

~~~html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
~~~

重构hello world代码：

~~~jsx
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JSX语法</title>
    <!-- jsx语法要求：jsx语法属于js增强语法，浏览器不认识。需要通过babel进行转化，因此需要引入		
    babel.js，要求babel.js文件在最前面 -->
    <script src="js/react.js"></script>
    <script src="js/react-dom.js"></script>
    <script src="js/babel.js"></script>
</head>

<body>
    <div id="root"></div>
    <!-- babel默认不解析script代码段，只解析text/babel属性的script代码段 -->
    <script type="text/babel">
    // 01: 获取元素
    const container = document.getElementById('root');
    // 02: 创建root 根实例对象
    const root = ReactDOM.createRoot(container);
    // 03: 使用jsx 语法定义react 组件
    // 04: jsx模板语法必须有根标签包裹
    // jsx 语法的特点: 在js 中直接写标签dom,但是浏览器不能识别该写法,需要使用另一个工具babel
    // 在页面汇总引入babel,同时设置script的类型为 text/babel
    const el = (
        <div>
            <p>我是p元素</p>
        </div>
    )
    root.render(el)
    </script>
</body>

</html>
~~~

**面试题:**  root.render( )  方法都干什么啦?

```jsx
render 函数(组件): 
第一步: 判断当前组件是函数组件还是类组件,当为函数组件的时候,当直接调用函数, 将函数组件return 返回的虚拟dom 转成真实dom. 然后渲染页面, 当组件为类组件的时候, 创建组件实例, new 类名(); 然后再调用组件实例上的render 生命周期函数. 将虚拟dom 转成真实dom 渲染页面.
```



## 3、JSX语法基础

### 3.1、基础

在JSX语法中，要把JS代码写到`{ }`中，所有标签必须要闭合。

注意：在jsx语法中不支持“//”注释形式以及“<!---->”注释形式，只能使用“{/* */}”注释形式。

~~~jsx
let num = 100;
let bool = false;

// JSX 语法
var vNode = (
    <div>
        {/* 展示数据 */}
        {num}
        <hr />
        {/* 三目运算 */}
        {bool ? "条件为真" : "条件为假"}
    </div>
);
// console.log(vNode) 此处 vNode 就是一个虚拟DOM结构
// 01: 获取元素
const container = document.getElementById('root');
// 02: 创建root 根实例对象
const root = ReactDOM.createRoot(container)
root.render(vNode);
~~~

~~~jsx
const src = "http://www.mobiletrain.org/images/index/new_logo.png";
const style = { fontSize: "20px", color: "red" };
const html = "<a href='http://www.baidu.com'>百度一下</a>";
const username = '千锋教育';
// 获取元素
const app = document.getElementById("app");
// 创建虚拟DOM
const vNode = (
    <div>
        { /*标签的属性如果需要被JSX解析，则属性的值不能加引号*/ }
        <img src={src} />
        <hr/>
        <p style={style}>北川3次地震为汶川地震余震</p>
        <hr/>
        <p>{username.substr(0, 2)}</p>
        <hr/>
        <p className="cl1">iPhone12开售排队</p>
        { /*
             输出HTML字符串（了解）
             注意点：react默认不解析html字符串
             原因是：安全问题
             如果真要输出解析的html字符串请按照以下的语法
        */ }
        <p dangerouslySetInnerHTML={{__html:html}}></p>
    </div>
);
// 01: 获取元素
const container = document.getElementById('root');
// 02: 创建root 根实例对象
const root = ReactDOM.createRoot(container)
root.render(vNode);
~~~



### 3.2、数组渲染

#### 3.2.1、直接渲染

~~~jsx
let arr = ["张三", "李四", "王五", "罗翔"];
// 01: 获取元素
const container = document.getElementById('root');
// 创建虚拟DOM
const vNode = (
    <div>
     {/* jsx中如果是一维数组，直接写上就可以遍历渲染了,该方式不推介,只能遍历一维数组,因为无法分割代码*/}
        {arr}
    </div>
);
// 02: 创建root 根实例对象
const root = ReactDOM.createRoot(container)
// 渲染
root.render(vNode);
~~~

上述输出的结果如下：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/300444482c3a5c11e3fdf15003ba6142ba0b951d.png?sign=14d92baf0356eaf1441e41093edb4065&t=5f92987b)

#### 3.2.2、处理并渲染

~~~jsx
let arr = ["张三", "李四", "王五", "罗翔"];
// 获取挂载点
const el = document.getElementById("app");
// 创建虚拟DOM
const vNode = (
    <div>
        {/* 处理并渲染 */}
        <ul>
            {/* 给循环体包裹一层{}，不包就错 */}
            {
                arr.map((item, index) => {
                    return <li key={index}>{item}</li>;
                })
            }
            <hr />
            {
                /* 
                给循环体包裹一层{}，不包就错，如果循环体就1行
            	{}与return可以被省略（箭头函数）
            	*/
            }
            {
                arr.map((item, index) => (
                    <li key={index}>{item}</li>
                ))
            }
        </ul>
    </div>
);
const root = ReactDOM.createRoot(el)
// 渲染
root.render(el);
~~~

面试题：react中循环使用的是map，能否替换为forEach，为什么？

答：不能，因为两者的返回值不一样，forEach没有返回值。



# 三、项目构建

React团队推荐使用create-react-app（相当于vue的`vue-cli`）来创建React新的单页应用项目，它提供了一个**零配置**的现代构建设置。

React脚手架（create-react-app）意义：

- 脚手架是官方提供，零配置，无需手动配置繁琐的工具即可使用

- 充分利用Webpack，Babel，ESLint等工具辅助项目开发

- 关注业务，而不是工具配置

create-react-app会配置我们的开发环境，以便使我们能够使用最新的 JavaScript特性，提供良好的开发体验，并为生产环境优化你的应用程序。为了能够顺利的使用create-react-app脚手架，我们需要在我们的机器上安装Node >= 14 和 npm >= 6.x版本。

npm i -g @vue/cli

vue create xxxx

## 1、初始化项目

在终端中使用以下命令来构建react项目：

~~~shell
# 免安装形式
npx create-react-app my-app
# 上面这种安装方式不需要全局安装create-react-app,如果需要全局安装，则可以执行下面的命令
# 创建项目时名称不能为这些名字：react、reactdom、reactscript等不允许使用
# npm i -g create-react-app
# create-react-app your-app

// 也可以指定安装的 react-create-app指定版本  npm i -g create-react-app@5
// 安装是否成功 查看版本 create-react-app -V 或 create-react-app --version
~~~

项目创建需要消耗的时间可能会有点久，在项目创建完毕后可以执行以下指令：

~~~shell
# 进入项目目录
cd my-app
# 启动项目
npm start
~~~

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/7e386902e80b9ef5ba3b68333fe7a44d4bd3eb33.png?sign=ef7d37b07b0a7dbae3f3f22912b5c9ed&t=5f93d66a)

> 如果本机安装了`yarn`（一款Facebook自家的包管理工具，类似npm），则安装好给予的项目启动命令提示是`yarn start`。



## 2、目录结构

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/d7961534de022d85cb6692d86bcd12573ab86e2b.png?sign=c6f3956be4f2b83e38b7ded996d9a923&t=5f93d80d)

- public目录下
  - manifest.json：清单文件（说明性文件），主要是一些项目的描述信息

  - robots.txt：用于声明当前项目哪些路径、目录允许搜索引擎抓取相关的页面描述信息(页面描述信息一般在meta标签的content属性就是网页的描述信息)(君子协定可以不遵守)

    - 是否允许哪一些搜索引擎可以抓取该项目的一些描述信息
    - User-agent:   搜索引擎
    - Disallow: url路径  (不允许搜索引擎爬取网站seo内容)

     案例:  百度中搜索天猫 不允许的.  
- src目录下
  - `*.css`：样式文件
  - App.js：类似于App.vue，就是react里面的根组件（**在react中，组件后缀是js，但是以后写react组件的时候后缀请使用jsx，为了便于区分组件与封装的js文件**）
  - App.test.js：测试文件
  - index.js：类似于vue中的main.js，是项目执行的主入口文件
  - reportWebVitals.js：谷歌新增的性能优化库文件,,这个没什么用.因为咱们国内的服务器一般都访问不了谷歌,所以没啥用.									
  - setupTests.js：针对项目index.js的一个单元测试文件

> 了解了react的目录结构后，可以对初始化的项目进行文件清理。**此处将`src`与`public`目录中的内容全部删除即可，后期如果需要自己往里面写内容。**



# 四、组件

![组件](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/279e26e948d53d33a2a05e10e7c29aa736fe80a1.png?sign=9f63925b42a763bb945ad25cda41928f&t=5f5073bf)

组件允许我们将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。从概念上类似于JavaScript函数，它接受任意的入参（props），并返回用于描述页面展示内容的React元素（JSX）。

在react中，组件的形式有2种：

- 函数组件（拥抱函数式开发方式，面向过程）
  - 无状态（函数组件也被称之为无状态组件，相当于vue中的data）
  - 无生命周期
  - **有Hooks（辅助函数的集合，这些辅助函数可以帮助开发者在函数组件下快速开发）**
- 类组件（面向对象）
  - 有状态
  - 有生命周期
  - 有this
  - 没有hooks



## 1、组件的创建方式

> 在react17之后，允许在项目不用“import React from "react";”，但是在之前的版本是不行的。建议写，肯定不会错。



### 1.1、函数创建组件

通过函数创建的组件有以下特点：

- 函数组件（无状态组件,类似于vue中的data,即没有数据）：使用JS的函数创建组件
- 函数名称以大写字母开头（建议）
- 函数组件**必须有返回值**，表示该组件的结构（虚拟DOM），且内容必须有顶级元素
- 函数组件是没有生命周期的

例如，新建组件文件`src/App.jsx`：

> 约定：组件后缀可以是`.js`也可以是`.jsx`，为了方便区分组件与项目的业务代码，建议组件用`.jsx`，业务代码后缀用`.js`。

~~~jsx
// 使用快捷键 rfc 即可创建函数组件模板
// 17.0后的版本react的导入可有可无,不使用React变量
// 函数名首字母必须大写
// 函数组件必须有返回值,返回值为一个jsx语法的dom模板结构
// 函数组件无状态,无生命周期,
// 函数组件有hooks
// 组件根元素还可以使用 <></> 或<React.Fragment></React.Fragment> 但是这种写法一般很少用
import React from 'react' 
function Hello() {
    return (
        <div>这是第一个函数组件</div>
    )
}
export default Hello
~~~

要想输出效果，可以再创建项目入口文件`src/index.js`：

~~~javascript
import React from "react";
import ReactDOM from 'react-dom/client';
import App from "./你创建的组件路径";
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  // React.StrictMode 在开发时,使用严格模式,对于打包后的生产环境代码没有任何影响,主要用来在控制台做一些提示和警告的
  // 例如 react 中有一些语法已经废弃,没有再维护,并没有删除,就会出一些提示,建议可以不用设置严格模式,因为当你使用第三方包的时候,
  // 第三方包中可能使用了废弃的语法,这时候控制台就会出现提示和警告.
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
~~~

箭头函数写法:

```jsx
import React from 'react'
const Hello = () => {
    return (
        <div>这是第一个函数组件</div>
    )
}
export default Hello
```



### 1.2、类组件

类组件有以下特点：

- 使用ES6语法的class创建的组件（有状态组件）
- 类名称为大写字母开头（建议）
- 类组件要继承React.Component父类，从而可以使用父类中提供的方法或者属性
- 类组件**必须**提供render方法，用于页面结构渲染，结构必须要有顶级元素，且必须有return返回
- **特点:有状态, 有生命周期**

~~~jsx
// 使用快捷键 rcc 即可创建类组件模板
// 类组件的主体是一个类 
// 类组件有状态,有生命周期,无hooks
// 类的名称需要遵循驼峰式写法
// 类必须有一个render方法,返回一个页面模板结构jsx语法
// 类必须要继承React.Component 

import React from "react";
// 创建class类，继承React.Component，在里面提供render方法，在return里面返回内容
class App extends React.Component {
    render() {
        return <div>这是第一个类组件</div>;
    }
}

export default App;
~~~

除了上述的写法以外，还可以对`React.Component`进行按需导入，写成以下形式：

~~~jsx
// 引入react和Component
import React,{Component} from "react";

// 类组件
class App extends Component {
    render() {
        return <div>这是第一个类组件</div>;
    }
}

// 导出
export default App;
~~~



## 2、组件传值（父-子）

组件间传值，在React中是通过**只读**属性props来完成数据传递的。

props：接受任意的入参，并返回用于描述页面展示内容的React元素。

### 2.1、函数组件

函数组件传值使用props：以形参的形式给函数传递`props`参数。（与vue的思想是一样的）

~~~jsx
// 1.子组件
import React from 'react';
// 1. 父传子  在父组件中找到子组件,给子组件设置自定义属性 自定义属性={变量}
// 2. 在子组件中,使用函数的形参接受父组件传过来的参数
const Child = (props) => {
    console.log(11, props) // props为参数对象
    return (
        <div>
            我是子组件--{props.tit}
        </div>
    );
}

export default Child;

// 2.父组件
const Parent = () => {
    const msg = '成龙'
    return (
        <div>
            <div>我是父组件</div>
            {/* 使用子组件 */}
            <Child tit={msg}></Child>
        </div>
    );
}
~~~

> React的父传子的方式与Vue类似，都是通过调用子组件给子组件传递自定义属性方式进行传值的。
>
> 

### 2.2、类组件

在父组件中通过自定义属性向子组件传值后，如何在子级类组件中获取传递过来的值呢？

我们可以在子级类组件中通过`this.props`属性来获取传递到子组件的值，如下：

~~~jsx
import React,{ Component } from 'react'
// 注意: 类组件接受的父组件传递的参数props会自动注册到子组件的实例上.所以访问需要使用this.props.属性名
class Cmp extends React.Component{
    render(){
      return (
        <div>{ this.props.name } -- 我是一个类</div>
      );
    }
}
export default Cmp
~~~

问题?
如果类组件和函数组件交叉使用时,怎么传递和获取参数呢? 



# 五、 事件处理

## 1、事件绑定

React元素的事件处理和DOM元素原生的很相似，但是有一点语法上的不同。React元素的事件绑定采用`on+事件名`的方式来绑定一个事件，注意，这里和原生的事件是有区别的，**原生的事件全是小写**，如`onclick`, React里的事件是驼峰如`onClick`，**React的事件并不是原生事件，而是合成事件**。

合成事件和原生事件的区别: 合成事件不存在兼容性问题. 合成事件的e.nativeEvent 的值为原生事件对象

在React里，类组件与函数组件绑定事件是差不多的，只是在类组件中绑定事件函数的时候需要用到`this`，代表指向当前的类的实例，在函数中不需要调用`this`关键词。

**函数组件事件绑定**

~~~jsx
import React from "react";
// 01: 将事件处理函数写在函数组件外
// 不建议将函数的事件处理函数写在函数组件外,因为当在事件处理程序中使用hooks函数时,
// hooks 函数的语法 只能在函数组件内使用,不能在函数组件外使用
const clickHandler = () => {
    console.log("海纳百川有容乃大，壁立千仞无欲则刚。");
};

const App = () => {
       // 02: 将事件处理函数写在函数组件内
       const clickHandler = (e) => {
           console.log('点击事件1');
           // e.nativeEvent为原生事件,合成事件和原生事件对象没有太大的区别
           console.log(e);
       }
    return <button onClick={clickHandler}>老林说</button>;
};

export default App;
~~~

**类组件事件绑定**

~~~jsx
import React, { Component } from "react";

    // 01: 类组件中的执行函数也可以写在类组件外,但是不建议,因为这样就无法操作类中的属性和方法(如该组件的数	据和方法)
    const clickHandler = (e) => {
        console.log('handle1', e);
    }
class App extends Component {
    render() {
        return (
            <div>
                // 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。
                <button onClick={this.clickHandler}>老林说</button>
            </div>
        );
    }
    //02: 类组件的事件执行函数 定义在类内部,与 render函数平级 
    clickHandler() {
        console.log("海纳百川有容乃大，壁立千仞无欲则刚。");
    }
}

export default App;
~~~

> 注意点：
>
> - 在写事件的时候，调用时如果不涉及传参的话，一定不要加`()`，加了就错。
> - 在类组件中写事件处理程序的时候，不能写标准的`function xxx () {}`，写了就报错，一定要用简化的写法或者箭头函数形式
> - 事件处理属性名称（事件绑定时用的属性）一定要使用符合react的小驼峰写法



## 2、事件对象

React中可以通过事件处理函数的参数获取到事件对象，它的事件对象叫做：合成事件，**即兼容所有浏览器**，无需担心跨浏览器兼容问题。这个对象和之前学习的事件对象所包含的方法和属性都基本一致，不同的是React中的事件对象并不是浏览器提供的，而是它自己内部所构建的。此事件对象拥有和浏览器原生事件相同的接口，包括`stopPropagation()`和 `preventDefault()`，如果我们想获取到原生事件对象，可以通过`e.nativeEvent`属性来进行获取。

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>老林说</button>
            </div>
        );
    }
    clickHandler(e) {
        console.log("海纳百川有容乃大，壁立千仞无欲则刚");
        // react构建的事件对象
        console.log(e);
        // 浏览器原生的事件对象
        console.log(e.nativeEvent);
        // 事件对应的DOM对象
        console.log(e.target);
        // 事件对应的DOM对象的内嵌HTML
    	console.log(e.target.innerHTML);
    }
}

export default App;
~~~



## 3、this指向问题(重点)

在JSX事件函数方法中的this，默认不会绑定this指向。如果我们忘记绑定，当我们调用这个函数的时候this的值为undefined。所以使用时一定要绑定好this的指向！

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/000bb708d55935b83a611f37318ee1d89c32bb26.png?sign=a0d08668cfee717e94664b0682dfaa23&t=5f95992c)

例如，像下面这段代码回调函数中的`this`输出为`undefined`：

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>
                    老林说
                </button>
            </div>
        );
    }
    clickHandler() {
        console.log(this); // undefined
    }
}

export default App;
~~~

解决上面出现的`this`指向问题的方式有以下几种：

Ø  **第一种方式**: 构造方法中绑定(**该方法无法解决参数传递的问题**)

```jsx
import React,{ Component } from 'react'
export default class extends React.Component {
    constructor(props){
        super(props) // 必须使用super 否则控制台报错
        // 在构造方法中指定this指向 当前组件的实例对象
        this.clickHandle = this.clickHandle.bind(this,100)
    }
    clickHandle(m,e){
        console.log(this);//  此时的this指向当前组价的实例对象
        console.log(m);
        console.log(e); // 事件对象
    }
    render(){
        return (
            <div><button onClick = {this.clickHandle}>点我点我点我</button></div>
        )
    }
}

```

Ø  **第二种方式**: 声明式使用bind绑定+传参

```js
// 绑定事件
<button onClick={this.fun.bind(this，参数1,参数2,...)}>按钮</button>
// 定义方法函数
fun(参数1,参数2,e事件对象){
    
}
```

Ø  **第三种方式**: 行内箭头函数绑定+传参

```js
// 绑定事件
<button onClick={(evt) => this.fun(evt，参数1,参数2,...)}>按钮</button>
//定义方法函数
fun(evt事件对象,参数1,参数2,...)
```

Ø  **第四种方式**: 定义事件方法使用箭头函数来绑定(**该方法无法解决参数传递问题**)

```jsx
// 通过箭头函数定义事件方法，也能解决this指向问题
  clickHandle = (e) => {
        console.log(this); // 当前组件实例对象
        console.log('dianji');
        console.log(e);  // 事件对象
    }
    render(){
        return (
            <div><button onClick = {this.clickHandle}>点我点我点我</button></div>
        )
    }
```



## 4、事件方法传参

React中对于事件方法传参的方式有着非常灵活的用法。以传递参数`username`值为`zhangsan`为例，常见的有以下几种方式：

- 通过`this.事件方法.bind`（bind为绑定数据）方式进行传参（**推荐**），例如：
  - `onClick={this.clickHandler.bind(this,'zhangsan')}`
    - 对应的形参接收：`clickHandler(username)`
  - `onClick={this.clickHandler.bind(this,'zhangsan')}`   有事件对象参数
    - 对应的形参接收：`clickHandler(username,event)`

- 使用箭头函数传参，例如：
  - `onClick={() => this.事件方法('zhangsan')}`
    - 对应的形参接收：`clickHandler(username)`
  - `onClick={(e) => this.事件方法('zhangsan',e)}`  有事件对象参数
    - 对应的形参接收：`clickHandler(username,event)`



# 六、State状态

> 如果将state与vue中的某个点做类比的话，则其相当于vue组件中的`data`，作用就是用于存储当前组件中需要用到的数据。(也就是组件的私有数据)

`state`状态只在class类组件才有，**函数组件没有此功能！**

## 1、基本使用

- 状态（state）即数据，是组件内部的私有数据，只能在组件内部使用
- state的值是对象，表示一个组件中可以有多个数据
- 通过this.state.xxx来获取状态（对应vue的this.xxxx）
- state数据值**只能**通过this.setState()来修改（与vue不同）
- Do not mutate state directly. Use setState()
- state可以定义在类的构造函数中也可以写在类的成员属性中

**第一种设置方式**

~~~jsx
import React, { Component } from "react";

class App extends Component {
    // 构造函数初始state
    constructor(props) {
        super(props);
        this.state = {
            count: 0,
        };
    }
    render() {
        return <div>{this.state.count}</div>;
    }
}

export default App;
~~~

**第二种设置方式（推荐）**

~~~jsx
import React, { Component } from "react";

class App extends Component {
    // 类成员方式初始化
    state = {
        count: 0,
    };
    render() {
        return <div>{this.state.count}</div>;
    }
}

export default App;
~~~

> 切记：不要直接通过`this.state.xxx = xxxx`形式去更改state的值。否则会包警告，警告如下：Do not mutate state directly. Use setState()。



## 2、修改state状态

在vue中，data属性是利用`Object.defineProperty`处理过的，更改data的数据的时候会触发数据的`getter`和`setter`，但是React中没有做这样的处理，如果直接更改的话，react是无法得知的，所以，需要使用特殊的更改状态的方法`setState`。

`setState`接受2个参数，第一个参数负责对state自身进行修改（必须的），我们称之为`updater`；第二个参数是一个回调函数（可选），因为`setState`方法是异步的，如果想在更新好状态后做进一步处理，此时就可以用到第二个参数了。

语法：**this.setState(updater[,callback])** 

 	**注意:  在callback回调函数中,可以获取修改的的状态, 回调函数的是在修改完state后才触发.**

```js
getdata() {
      console.log(1, this.state);
        // 03: 修改state 中的数据,该方式属于异步方式
        // 方式1: 对象方式
      this.setState({
        msg: 123
        }, () => {
        console.log(console.log(2, this.state));
        })
        console.log(3, this.state);
 }
```

`updater`参数传递的时候支持两种形式：

- 对象形式
- 函数形式

**对象形式**

~~~jsx
this.setState({
    count: this.state.count + 1
})
~~~

**函数形式**

~~~jsx
this.setState((state,props) => {
    return {
        count: state.count + 1
    }
})
~~~

上述两种参数形式的`updater`建议使用**函数形式**。因为对象形式在批量使用的时候会存在问题，因此建议使用函数形式。

**思考?**  setstate 方法修改数据会不会影响state 对象中的其他数据

...





## 3、小结

**Q1:  如下操作执行count 的值是多少?** 

解答: 使用对象式修改的话,值为1 , 

​         使用函数式修改的话,为累加 

原因: react为了提高渲染性能,会将this.setState()该方法中的对象进行合并,由于对象的属性都是count,所以以最后的对象会覆盖前面的对象count. 如果setState中的值为函数的话,那么合并的时候,所有的函数合并成一个数组,那么执行的时候,数组中的函数会循环依次执行.所以结果累加

```jsx
  state = {
        msg: "我是数据",
        count: 0
    };
   render() {
        return (
            <div>
                <p>{this.state.count}</p>
                <button onClick={() => { this.handle2() }}>count++</button>
                <button onClick={() => { this.handle3() }}>count++</button>
            </div>
        );
    };
    handle2() {
        this.setState({
            count: this.state.count + 1
        });
        this.setState({
            count: this.state.count + 1
        })
        this.setState({
            count: this.state.count + 1
        })
    };
   handle3() {
        this.setState((state) => {
            return {
                count: state.count + 1
            }
        }
        this.setState((state) => {
            return {
                count: state.count + 1
            }
        })
    }
```



**Q2：state与props有什么区别？**

​     a. state是组件自身数据的管理对象而props是别的组件传递来的数据的管理对象；

​     b. state是可以修改的，而props是不能修改的；

​     c. state只能在类组件中使用，而props既可以在类组件中使用也可以在函数组件中使用；  



**~~Q3：this.setState()是同步的还是异步的？**~~

​     ~~该方法既有同步实现也有异步实现。如果代码进入了react代码流程，则是异步的（居多），否则（原生js，典 	 型的有定时器）是同步的。~~

- ~~在组件生命周期或React合成事件中，setState是异步~~

  ```jsx
  // 为什么被设计成异步?
  如果每次调用 setState都进行一次更新，那么意味着render函数会被频繁调用，界面重新渲染，这样效率是很低的；
  最好的办法应该是获取到多个更新，之后进行批量更新
  ```

- ~~在setTimeout或者原生dom事件中，setState是同步~~

```jsx
//在组件生命周期或React合成事件中，setState是异步；
//在setTimeout或者原生dom事件中，setState是同步；

//1. setState 可能是异步更新（有可能是同步更新） 
this.setState({
    count: this.state.count + 1
}, () => {
    // 联想 Vue $nextTick - DOM
    console.log('count by callback', this.state.count) // 回调函数中可以拿到最新的 state
})
console.log('count', this.state.count) // 异步的，拿不到最新值

//2. setTimeout 中 setState 是同步的
setTimeout(() => {
    this.setState({
        count: this.state.count + 1
    })
    console.log('count in setTimeout', this.state.count)
}, 0)

//3.自己定义的 DOM 事件，setState 是同步的。再 componentDidMount 中
componentDidMount() {
    // 自己定义的 DOM 事件，setState 是同步的
    document.body.addEventListener('click', this.bodyClickHandler)
}
bodyClickHandler = () => {
    this.setState({
        count: this.state.count + 1
    })
    console.log('count in body event', this.state.count)
}
componentWillUnmount() {
    // 及时销毁自定义 DOM 事件
    document.body.removeEventListener('click', this.bodyClickHandler)
    // clearTimeout
}
```



## 4、react 中的虚拟DOM 和Diff 算法

执行过程: 
01: 初始渲染时,也就是执行render方法的时候, react 会根据state (Modal)和jsx结构, 创建一个虚拟DOM对象(也叫虚拟dom 树)

02:  根据虚拟dom 渲染成真实dom ,渲染到页面中

 ![img](file:///C:\Users\wusheng\AppData\Roaming\Tencent\Users\1038050095\QQ\WinTemp\RichOle\7E``J{2}7PZQO@GOHUQVQ`Y.png) 

03: 当数据改变的时候(比如 setState())的时候, 根据新的数据创建新的虚拟DOM树

04: 新的虚拟Dom树和旧的虚拟dom 树比较, 使用diff算法比较, 找到相应需要更新的内容(如下红色部分标记 patch 就是打补丁)

05: 最终react 只将变化的部分更新到页面真实dom中. 



 ![img](file:///C:\Users\wusheng\AppData\Roaming\Tencent\Users\1038050095\QQ\WinTemp\RichOle\}X`3YGT9ZS6NXC`MIB{SM82.png) 



```jsx
// 代码演示
// 当点击num++ 的时候, 只有 包含num 的p 标签渲染了.
import logo from './logo.svg';
import React, { Component } from 'react';
class App extends Component {
  state = {	
    flag: true,
    mytit: 'abc',
    num: 0
  }
  render() {
    return (
      <div className="App">
        我是app 组件
        <p>{this.state.mytit}</p>
        <p onClick={() => {
          this.myhandleFn()
        }}>{this.state.num}</p>
      </div>
    );
  }

  myhandleFn = () => {
    this.setState({
      num: this.state.num + 1
    })
  }

}

export default App;

```



# 七、Props进阶

## 1、children属性

在vue中对应的是插槽Slot（匿名插槽）。

`children`属性表示组件标签的子节点，**当组件标签有子节点时**，接受传值的子组件中的props里就会有该属性，与普通的props一样，其值可以使任意类型 字符串/数组/对象...。

例如，在父组件中有如下代码：

~~~jsx
import React, { Component } from "react";
import Cmp from './Components/Cmp'

class App extends Component {
    state = {
        content: "蚂蚁集团A股发行价确定",
    };
    render() {
        return <Cmp>{this.state.content}</Cmp>;
    }
}

export default App;
~~~

上述代码中的`{this.state.content}`即为子组件标签中的子节点，那么在子组件的props属性里就存在一个children属性可以被我们使用。如下：

~~~jsx
import React, { Component } from "react";

class Cmp extends Component {
    render() {
        return <div>{this.props.children}</div>;
    }
}

export default Cmp;
~~~

> 需要注意，**如果子组件标签里只存在一个子节点，则children属性值为一个字符串；如果子组件标签里存在多个子节点，那么children属性的值为一个索引数组。**

简而言之，上述写法形式有点像Vue里的插槽，但是不是，children这一小结所讲的内容简单来说就是父传子的另外一种写法而已：原先父传子是将值写在了**组件标签的属性中**，只不过现在写在了**组件标签里**而已。



## 2、props-type

> 关于JavaScript的class中的静态成员与常规成员
>
> ~~~javascript
> class App {
> 	static uname = 'zhangsan'
>  	username = "lisi"
> }
> console.log((new App).username);
> console.log(App.uname);
> // 常规的属性是在对象里的，如果要用得先实例化
> // 静态属性是类里面的，使用的时候不要实例化
> // 静态成员要优先于常规的成员
> 
> // 在vue中如何传参,设置参数规则
> new Vue({
>      el: "#app",
>      data() {
>          return {}
>      },
>      // props: ["one","two"],
>      props: {
>          one: {
>              type: String,
>              default: "1",
>          },
>          two: {
>              type: Number,
>              default: 1,
>          },
>      },
> })
> ~~~

React是为了构建大型应用程序而生，在一个大型应用开发过程中会进行多人协作，往往可能根本不知道别人使用你写的组件的时候会传入什么样的参数，这样就有可能会造成应用程序运行不了但是又不报错的情况。所以必须要对于`props`设传入的数据类型进行校验。

为了解决这个问题，React提供了一种机制，让写组件的人可以给组件的`props`设定参数检查，需要安装和使用[prop-types](<https://www.npmjs.com/package/prop-types>)：

~~~shell
# 现在的react的版本已经直接将其作为依赖，在node_modules中已经有了，因此可以免于安装
npm i -S prop-types
~~~

在使用时，无论是函数组件还是类组件，都需要对`prop-types`进行导入：

~~~jsx
import Pts from "prop-types"
~~~

随后依据使用的组件类型选择对应的应用方式，如果是函数组件则按照下面的方式应用：

~~~jsx
function App(props){
    // 函数组件声明过程
    return "";
}
// 为App方法组件挂上验证规则
App.propTypes = {
    // 待验证的属性名：PropTypes.类型规则[.isRequired] 
    // isRequired不可以单独使用,否则报错
    propname:Pts.string,
    // ... 
}
~~~

如果是类组件则按照下面的方式应用：

~~~jsx
class App extends Component{
    // 类内部完成检查(该种方式好像已经废弃了,使用没有效果啦)
    static propTypes = {
       // 待验证的属性名：PropTypes.类型规则[.isRequired]
       prop-name:Pts.string
    }
	// 渲染
	render() {
        return "";
    }
}
~~~

本质上来上面类组件的写法与方法组件的写法是一样的，只不过考虑到类组件的代码完整性，我们把规则的验证做成了类的静态成员属性的方式，如果还原成最初的代码则如下：

~~~jsx
class App extends Component {
    render() {
         return <div>{this.props.flag}--{this.props.num}</div>;
    }
}

App.propTypes = {
    flag: PropTypes.string,
    num: PropTypes.number.isRequired,
};
~~~

需要注意，`isRequired`规则必须放在最后且不能独立于其他规则存在。更多的验证规则，可以参考[React官网](https://reactjs.org/docs/typechecking-with-proptypes.html)。



## 3、默认值(defaultProps)

如果`props`有属性没有传来数据，为了不让程序异常，我们可以依据业务情况给对应的属性设置默认值。

依据组件类型的不同选择对应的操作方式，如果是函数组件则采用如下方式：

~~~jsx
function App(props){
    // 函数组件声明过程
    return "";
}
// 为App方法组件挂上默认值
App.defaultProps = {
    // 属性名：默认值
    title: "标题",
    // ...
}
~~~

如果使用的是类组件则使用如下方式：

~~~jsx
class App extends Component {
    // 添加静态成员属性`defaultProps`设置props的默认值
    static defaultProps = {
        // 属性名：默认值
        title: "标题"
        // ...
    }
}

// 方式2 将静态属性定义在类外
App.defaultProps ={
    title: '标题'
}
~~~



# 八、生命周期（重点）

回顾：Vue生命周期

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeDestroy
- destroyed
- activated
- deactivated

**函数组件无生命周期一说。**

生命周期函数指在某一时刻组件会自动调用并执行的函数。React每个类组件都包含`生命周期方法`，我们可以重写这些方法，以便于在运行过程中特定的阶段执行这些方法。例如：

我们希望在第一次将其呈现到DOM时设置一个计时器`Clock`。这在React中称为“安装”。

我们也想在删除由产生的DOM时清除该计时器`Clock`。这在React中称为“卸载”。

参考：https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

**完整的生命周期图**

![完整的生命周期图](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/d96aceebbcf9bf3dccc95b3afc7b2f75ef3e59ec.png?sign=81ab7fb514b47c66446ba45f557afe0f&t=5f96fd9b)



**常用的生命周期图**

![常用的生命周期图](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/06b9e675773182f54d91647cbf48ec2b5aa43de9.png?sign=2119db1b051cbdc67293ac5351be7884&t=5f96fddc)

- 常用简单周期函数

  - constructor：构造函数，一般在这里做初始化操作(如: 变量的赋值操作,state 赋值,bind 的绑定..等等)

  - render：渲染周期函数，在这里做用户界面的渲染，必须要有该周期函数

  - componentDidMount：组件挂载完毕周期，**该周期一般用于做网络请求**,初始化只执行一次,组件更新不执行

  - getSnapshotBeforeUpdate：在更新前获取快照（更新前的一个备份）周期函数，一般在这里获取更新之前的一些信息的，例如获取组件在更新之前的高度、宽度,以及滚动条高度等等。该周期函数不能单独使用，一定要配合componentDidUpdate一起使用，否则报错。该周期函数**必须要有返回值**，返回我们需要在更新前获取的数据

  - componentDidUpdate(props,state,snapshot)：组件更新完毕的周期函数，在组件渲染完毕后触发，该周期可能触发多次

    -  参数1: props 对象,父子组件传值
    -  参数2: state对象,  数据对象
    -  参数3: snapshot 该参数为 getSnapshotBeforeUpdate 生命周期的返回值(return 值)

    ```jsx
    // getSnapshotBeforeUpdate 生命周期的使用
    
    import React, { Component } from 'react';
    import axios from 'axios';
    import '../assets/css/child1.css';
    class Child1 extends Component {
        constructor() {
            super()
        }
        state = {
            count: 1,
            nameArr: [1, 2, 3]
        }
        render() {
            return (
                <div>
                    子组件1
                    <p><button onClick={this.addlistFn}>给数组添加内容</button></p>
                    <ul id='ulbox'>
                        {this.state.nameArr.map((item, index) => <li key={index}>{item}</li>)}
                    </ul>
                </div>
            );
        };
        addlistFn = () => {
            // 点击改变数组的长度, 尾增一个li,给ul 盒子设置设置固定高度,随着li 的变化, 
            // ul出现滚动条, 
            this.state.nameArr.push('小白')
            this.setState({
                nameArr: this.state.nameArr
            })
        };
        componentDidUpdate(props, state, snap) {
            // console.log(props, state, snap);
            // 设置ul的滚动条的高度 = ul整个盒子的高度(包括滚动的距离)  - 盒子的高度
            // 这样可以让滚动条永远指向当前最新的也就是新增的li 的显示位置
            document.querySelector('ul').scrollTop = document.querySelector('ul').scrollHeight - snap
        };
        getSnapshotBeforeUpdate(props, state) {
            return document.querySelector('ul').offsetHeight
        };
    }
    
    export default Child1;
    
    ```

  - componentWillUnmount：组件解除挂载之前触发，一般在该周期中做组件的销毁善后处理（副作用代码的清理，例如：定时器、事件监听等，专门做卸磨杀驴）

    ```js
    // 可以通过父组件中选项卡切换不同的子组件实现 该生命周期的调用
    // 测试接口地址: https://api.i-lynn.cn/college
    // 父组件 
    import React, { Component } from 'react';
    // 分别引入对应的子组件
    import Child1 from '../components/threeChild1'
    import Child2 from '../components/threeChild2'
    class Threeclass extends Component {
        state = {
            flag: true
        }
        render() {
            return (
                <div>
                    <p>我是父组件三</p>
                    <p><button onClick={() => { this.changeFn() }}>点击切换</button></p>
                    {/* 根据flag的值来切换显示对应的哪个组件 */}
                    {this.state.flag ? <Child1 /> : <Child2 />}
                </div>
            );
        };
        changeFn() {
            this.setState({
                flag: !this.state.flag
            })
        }
    }
    export default Threeclass;
    
    // 子组件
    class Threechild1 extends Component {
        source = axios.CancelToken.source() // 设置请求对象
        render() {
            return (
                <div>
                    我是子组件1
                </div>
            );
        }
        componentDidMount() {
            document.onclick = function () {
                console.log('document');
            }
            // 发起网络请求(注意如果是get 请求,标记设置在第二个参数对象中,如果是post请求,设置在第三个         参数中)
            axios.get('https://api.i-lynn.cn/college', {
                cancelToken: this.source.token  // 标记该网络请求是可以取消的网络请求
            }).then(res => {
                console.log(res);
            }).catch(e => { })
        }
        componentWillUnmount() {
          console.log('组件1卸载');
            document.onclick = null;
             // 取消网络请求
            this.source.cancel()
        }
    }
    export default Threechild1;
    ```
    
    

- 相对复杂的生命周期函数

  - getDeriviedStateFromProps(props,state)：（应该写在子组件中）

    -  此方法是react16.3之后新增，该生命周期在render之前执行, 表示从props中获取数据来更新state这个 阶段。一般用于父子传参的情况, 它返回一个对象来更新 state，如果返回 null 则不更新任何内容。必须有返回值
    -  执行时间: 初始化传props, props改变时,  自身的state 修改时
    - 参数props: 表示从父组件接受的属性对象
  - 参数state :表示自身的state 对象数据
  
- 注意点
  
  - 1. 必须需要写在子组件中
    2. 必须要有state
    3. 该周期函数必须返回state对象或者null（如果返回null则表示不更新state，如果返回对象则表示更新state数据,）
  
  ​          4. 如果props 中的属性和自身state 中的属性有一样的,则会覆盖,否则会合并操作
  
- 作用：
  
    1. 能够实现props里的数据可读可写
  2. 数据可以实现集中管理   
  
  ```jsx
  // 父组件
  import Fourchild from '../components/fourChild'; // 引入子组件
  import React, { Component } from 'react';
  class Fourclass extends Component {
      render() {
          return (
              <div>
                  <p>我是父组件</p>
                  <Fourchild msg='张三'></Fourchild>
              </div>
          );
      }
  }
  export default Fourclass;
  
  // 子组件
  import React, { Component } from 'react';
  class Fourchild extends Component {
      state = {
          num: 0,
          msg: '李四'
      }
      render() {
          console.log('render');
          return (
              <div>
                  我是子组件
              </div>
          );
      };
      // 该生命周期函数在render 前执行
      static getDerivedStateFromProps(props, state) {
          console.log(props, state);
          // 1. props为父组件传过来的参数
          // 2. state 为自身的数据state
          // 3. return 的返回值可以为null; 也可以为一个对象,返回的对象会自动和自身的state合并, 需要根据			  业务场景判断
              // 如果自身state中有相同的属性, 则使用自身的state,此时就返回null,否则会覆盖该属性
              // 如果自身的state中没有该属性,则return props,
          if (state.msg) {
              return null
          } else {
              return props
          }
      }
  }
  
  export default Fourchild;
  ```
```jsx
 
- shouldComponentUpdate(props,state)：
  
    - 当 props 或 state 发生变化时，shouldComponentUpdate() 会在render渲染执行之前被调用。是否重新渲染该组件, 首次渲染时不会调用该方法。此方法仅作为性能优化的方式而存在。
    - 该生命周期函数必须有返回值默认为 true, 为true则组件继续渲染，为false则当前组件不会渲染。
    
    - 参数1: props 更新后的props属性对象
    
    - 参数2: state 组件自身的state 数据对象
    
    - 你也可以考虑使用内置的 **PureComponent** 组件，而不是手动编写 shouldComponentUpdate()。**PureComponent** 会对 props 和 state 进行浅层比较(PureComponent内部进行比较)，并减少了跳过必要更新的可能性。【性能优化在深度讲解】
    
    - 注意: 如果props得数据结构比较深不是简单的number string boolean 之类的,而是数组,对象那么使用PureComponent 无法判断是否要重新渲染.
    
    
    
  **案例:** 当**this.setState()**修改了state中的数据后，当父组件中的state修改后,父组件就会重新渲染,这样父组件中包含的子组件也会都重新渲染. ,所以这样就需要使用shouldComponentUpdate 或PureComponent 对渲染进行优化,减少没必要的组件渲染.
  
  ```jsx
  //父组件 
  // 点击+10 或+20 组件1 和组件2 的render都会重新渲染,使用shouldComponentUpdate周期解决没必要的执行
  import Child1 from '../components/fiveChild1' // 引入子组件1 
  import Child2 from '../components/fiveChild2'// 引入子组件2 
  import React, { Component } from 'react';
  class Fiveclass extends Component {
      state = {
          money1: 100,
          money2: 200
      }
      render() {
          return (
              <div>
                  <p>我是父组件</p>
                  {/* 使用子组件 */}
                  <div>
                      <Child1 money1={this.state.money1}></Child1>
                      <button onClick={() => { this.add1() }}>+10</button>
                  </div>
                  <div>
                      <Child2 money2={this.state.money2}></Child2>
                      <button onClick={() => { this.add2() }}>+20</button>
                  </div>
  
              </div>
          );
      };
      add1() {
          this.setState({
              money1: this.state.money1 + 10
          })
      };
      add2() {
          this.setState({
              money2: this.state.money2 + 20
          })
      }
  }
  
  // 组件1 
  import React, { Component } from 'react';
  class Fivechild1 extends Component {
      render() {
          console.log('我是子组件1')
          return (
              <div>
                  <p>我是子组件1---{this.props.money1}</p>
              </div>
          );
      };
      shouldComponentUpdate(props, state) {
          // props 为最新传过来的值
          // state 为当前组件本身的state对象数据
          // this.props 为上一次传过来的props值,也就是还未更新前的props
          //console.log(props, this.props)
          if (props.money1 == this.props.money1) {
              // 说明当前props没有更新
              return false // false 表示不继续渲染该组件
          } else {
              return true  // true 表示继续渲染该组件
          }
      }
  }
  export default Fivechild1;
  
  // 组件2 同组件1代码类似
  import React, { Component } from 'react';
  
  class Fivechild2 extends Component {
      render() {
          console.log('我是子组件2')
          return (
              <div>
                  <p>我是子组件2---{this.props.money2}</p>
              </div>
          );
      }
      shouldComponentUpdate(props, state) {
          // props 为最新传过来的值
          // state 为当前组件本身的state对象数据
          // this.props 为上一次传过来的props值,也就是还未更新前的props
          //console.log(props, this.props)
          if (props.money2 == this.props.money2) {
              // 说明当前props没有更新
              return false
          } else {
              return true
          }
      }
  }
  export default Fivechild2;
  
  // 如上简化方法：只需要将类组件的继承关系改成继承`PureComponent`即可，这样一来，框架会自动判断值是否有变化进一步决定组件是否需要被渲染。
  // 组件1 
  import React, {Component, PureComponent } from "react";
  class Fivechild1 extends PureComponent {
      render() {
          console.log('我是子组件1')
          return (
              <div>
                  <p>我是子组件1---{this.props.money1}</p>
              </div>
          );
      };
  }
  export default Fivechild1;
```

  

**面试题：**

Q1：为什么把网络请求放在componentDidMount中，而不是构造函数或者render中？

- 构造函数作为初始化用途，网络请求不能算作初始化，所以不在构造函数中写网络请求
- render会重复触发，而且会在setState后再次触发render，而网络请求之后一般会伴随至少一个setState，容易触发死循环
- 基于js的异常机制（上面错了，下面不执行了），建议网络请求往后放，至少保证用户能够看到页面

Q2：vue中网络请求放在哪里？为什么？

Q3：如何取消网络请求？如下demo

```jsx
 //请求测试地址:  https://api.i-lynn.cn/college
 // 如下: 取消一个axios 请求的demo 共三步:
 // 第一步:创建一个souce对象,注意:每一个请求对应一个source对象,如果需要取消多个请求,那么需要创建多个     source(如source1,source2...)
 // 第二步:指定当前的请求是可以取消的请求
		// get方式请求在第二个参数指定 axios.get(url,{cancelToken: this.source.token})
        // post方式请求在第三个参数指定 axios.post(url,{参数},{cancelToken: this.source.token})
 // 第三步:取消该请求
import React, { Component } from 'react';
import axios from 'axios'
class Threechild1 extends Component {
    source = axios.CancelToken.source() // 第一步:创建一个souce对象
    render() {
        return (
            <div>
                我是子组件1
            </div>
        );
    }
    componentDidMount() {
        document.onclick = function () {
            console.log('document');
        }
        // 发起请求
        axios.get('https://api.i-lynn.cn/college', {
            cancelToken: this.source.token //  第二步:指定当前的请求是可以取消的请求
        }).then(res => {
            console.log(res);
        })
    }
    componentWillUnmount() {
        console.log('组件1卸载');
        document.onclick = null
        // 取消请求
         this.source.cancel('取消请求')  // 第三步:取消该请求

    }
}
export default Threechild1;
```



友情提示：

- 自react v16.9开始，以下周期被废弃
  - componentWillMount/UNSAFE_componentWillUpdate：在组件挂载之前触发的周期
  - componentWillUpdate/UNSAFE_componentWillUpdate：在组件更新之前触发的周期
  - componentWillReceiveProps/UNSAFE_componentWillReceiveProps：在组件接收新的props之前触发的周期

- componentDidCatch：类似于vue中的第11个周期，用于自定义组件错误的，一般不用



# 九、表单处理

## 1、特别说明

有以下示例代码：

~~~jsx
import React, { Component } from "react";
// 编辑功能的案例
class App extends Component {
    state = {
        msg: "hello world",
    };
    render() {
        return (
            <div>
                <input type="text" value={this.state.msg} />
            </div>
        );
    }
}

export default App;
~~~

通过运行后我们可以在浏览器的`console`控制台找到React给予我们的提示：

> **Warning**: You provided a `value` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultValue`. Otherwise, set either `onChange` or `readOnly`.

通过上述的警告提示，我们可以得知，在React中并不存在类似于Vue的双向数据绑定操作。此处需要注意以下几点：

- Vue中的`v-model`是语法糖

- 在React里使用的是**单向**数据流

由于在React里数据流是单向的，所以我们就必须得考虑一个问题：怎么获取用户在表单中输入的数据呢？解决办法：

- 给表单项添加`onChange`事件，通过事件处理实现双向绑定效果【受控组件】
- 给表单项的value/checked，设置成defaultValue/defaultChecked，结合ref对象实现双向效果【非受控组件】

**React推荐我们使用受控组件。**



## 2、受控组件

将`state`与表单项中的`value`值绑定在一起，由`state`的值来控制（onChange事件）表单元素的值，称为受控组件。

绑定步骤：

- 在state中添加一个状态，作为表单元素的value值

- 给表单元素绑定change事件，将表单元素的值设置为state的值

```jsx
import React, { Component } from 'react';
// 将表单中的value 的值由state 中的数据去实现控制. 这种表单叫做 受控组件
// 受控组件的特点: 实现了双向数据的绑定
// 语法: value + onChange 实现
class Mycls extends Component {
    state = {
        username: "尉迟恭",
        Sex: '男',
        checkedArr: ['money', 'cool'],
        seled: '美人'
    }
    render() {
        const { username, Sex, checkedArr, seled } = this.state
        return (
            <div>
                {/* 输入框 */}
                <p>
                    <input type='text' name='username' value={username} onChange={this.editFn.bind(this, 'username')} />
                </p>
                {/* 单选 */}
                <p>
                    <input type='radio' name='Sex' value='男' checked={Sex == '男'} onChange={this.editFn.bind(this, 'Sex')} /> 男
                    <input type='radio' name='Sex' value='女' checked={Sex == '女'} onChange={this.editFn.bind(this, 'Sex')} /> 女
                </p>
                {/* 复选 */}
                <p>
                    <input type='checkbox' value='money' checked={checkedArr.includes('money')} onChange={this.editFn.bind(this, 'checkedArr')} />money
                    <input type='checkbox' value='car' checked={checkedArr.includes('car')} onChange={this.editFn.bind(this, 'checkedArr')} />car
                    <input type='checkbox' value='cool' checked={checkedArr.includes('cool')} onChange={this.editFn.bind(this, 'checkedArr')} />cool
                </p>
                {/* 下拉框 */}
                <p>
                    <select value={seled} onChange={this.editFn.bind(this, 'seled')}>
                        <option value='江山' >江山</option>
                        <option value="美人" >美人</option>
                        <option value="兄弟" >兄弟</option>
                    </select>
                </p>
				<p><button onClick={()=>this.submitFn()}></bu>提交表单</button></button></p>
            </div>
        );
    };
    editFn = (key, e) => {
        // console.log('编辑');
        // 使用setState 来更新当前的state 状态
        // 思路: 将key 设置成变量 
        // 方式1: 通过传参的形式确定要修改的属性
        // 方式2: 通过事件对象获取当前标签的属性name 来获取要修改的数据
        // console.log(e.target.name);
        // 对象方式修改state
        // this.setState({
        //     [key]: e.target.value
        // })

        // 函数方式修改state
        this.setState((state) => {
            if (key == 'checkedArr') {
                // 复选框的操作
                // console.log(e.target.value);
                if (state.checkedArr.includes(e.target.value)) {
                    // 有当前的属性
                    state[key] = state.checkedArr.filter((item) => {
                        return item != e.target.value
                    })
                } else {
                    // 没有该属性
                    state.checkedArr.push(e.target.value)
                }
            } else {
                state[key] = e.target.value
            }

            return state
        })
    };
	submitFn(){
       console.log(this.state)
	}

}

export default Mycls;

```



## 3、非受控组件

没有和state数据源进行关联的表单项，而是**借助ref**，使用元素DOM方式获取表单元素值

使用步骤:

- 调用React.createRef()方法创建ref对象
- 将创建好的ref对象添加到文本框中
- 通过ref对象获取到文本框的值
- 注意:函数组件不能使用ref属性,否则报错,函数组件中使用hook函数.

> 提示：一般表单项少的时候可以考虑使用非受控组件。

```jsx
// 非受控表单组件
import React, { Component, createRef } from 'react';
import Sevenchild from '../components/sevenChild';
class Sevenclass extends Component {
    // 第一种方式:创建ref对象
    constructor() {
        super()
        this.ref1 = createRef() 
        this.ref2 = createRef()
        this.ref3 = createRef()
    }
    // 第二种方式: 创建ref对象
    ref1 = createRef();
    ref2 = createRef();
	ref3 = createRef();
    state = {
        username: '',
        pwd: ''
    }
    render() {
        let { username, pwd } = this.state
        return (
            <div>
                <p>
                    <span>用户名:</span>
                    <input ref={this.ref1} type="text" defaultValue={username}/>
                </p>
                <p>
                    <span>密码:</span>
                    <input ref={this.ref2} type="text" defaultValue={pwd} />
                </p>
                <Sevenchild ref={this.ref3}></Sevenchild>
                <p>
                    <button onClick={() => { this.submit() }}>提交</button>
                </p>
            </div>
        );
    };
    submit() {
        console.log(this.ref1); // 获取当前节点
        console.log(this.ref2); // 获取当前节点
        console.log(this.ref3); // 获取组件实例对象
    }
}
export default Sevenclass;
```

==作业：实现受控组件的方法封装（涵盖支持普通的文本框、单选按钮组、复选框、下拉菜单等）==



# 十、组件通信

## 1、父→子

**方式一**:  父组件将自己的状态传递给子组件，子组件当做属性来接收，当父组件更改自己状态的时候，子组件接收到的属性就会发生改变  

```jsx
// 父组件
import React, { Component } from 'react';
import EightChild from '../components/eightChild'
class Eightclass extends Component {
    state = {
        msg: 'hello world'
    }
    render() {
        return (
            <div>
                <p onClick={() => { this.handle1() }}>我是父组件</p>
                <EightChild tit={this.state.msg}></EightChild>
            </div>
        );
    };
    handle1() {
        this.setState({
            msg: '你好世界'
        })
    }
}
export default Eightclass;

// 子组件
import React, { Component } from 'react';
class Eightchild extends Component {
    render() {
        // console.log(this.props);
        return (
            <div>
                <p>我是子组件--{this.props.tit}</p>
            </div>
        );
    }
}
export default Eightchild;
```

**方式二**:  在父组件中定义一个方法,该方法返回子组件需要的数据(甚至是整个state),将该方法传给子组件,在子组件中调用获取父组件的数据

```jsx
// 父组件:
import React, { Component } from 'react';
import EightChild from '../components/eightChild'
class Eightclass extends Component {
    state = {
        msg: 'hello world'
    }
    render() {
        return (
            <div>
                <p>我是父组件</p>
                <EightChild fun1={this.transferFun}></EightChild> // 方式一
                 <EightChild fun1={this.transferFun.bind(this)}></EightChild> // 方式二
            </div >
        );
    };
    transferFun = () => { // 箭头函数可以保证函数 的this指向 //方式一
        return this.state.msg
    }
    transferFun(){ //方式二
        return this.state.msg
    }
}
export default Eightclass;

// 子组件:
class Eightchild extends Component {
    render() {
        // console.log(this.props);
        return (
            <div>
                <p>我是子组件--{this.props.fun1()}</p>
            </div>
        );
    };
    componentDidMount() {
        // 调用父组件传过来的方法获取父组件传的数据
        console.log(this.props.fun1())
    }
}
export default Eightchild;

```

**方式三**:  在子组件中定义一个方法,接受一个形参, 在父组件中调用子组件的该方法,将数据传递给子组件,通过ref属性获取子组件的实例对象,进而调用子组件上的方法

注意: 使用ref 属性只能使用在类组件上,函数组件上不能使用ref属性,报错

```jsx
// 父组件:
import React, { Component, createRef } from 'react';
import EightChild from '../components/eightChild'
class Eightclass extends Component {
    state = {
        msg: 'hello world'
    }
    ref1 = createRef()
    render() {
        return (
            <div>
                <p>我是父组件</p>
                <EightChild ref={this.ref1}></EightChild>
            </div >
        );
    };
    componentDidMount() {
        this.ref1.current.receiveFn(this.state.msg)
    }
}
export default Eightclass;

// 子组件 
import React, { Component } from 'react';
class Eightchild extends Component {
    render() {
        // console.log(this.props);
        return (
            <div>
                <p>我是子组件--</p>
            </div>
        );
    };
    receiveFn(data) {
        console.log(data) // data 就是父组件传过来的数据
    }
}
export default Eightchild;
```



## 2、子→父

- **方式一:**  在父组件中定义一个方法,该方法接受一个形参,父组件将该方法传递给子组件,子组件中调用父组件传递的方法,并将数据传给给方法.

  ```
  因为在react中props是可以传任意类型数据，而函数是不是也是任意类型数据之一
  1.父组件 props传过来当前自身的函数体  
  2.子组件通过props得到 
  3.this.props.名称()执行
  ```

  在父组件中定义对应方法并且通过属性传给子组件

  ```jsx
  // 父组件
  import React, { Component } from 'react';
  import Ninechild from '../components/nineChild';
  class Nineclass extends Component {
      state = {
          fatherMsg: '123'
      }
      render() {
          return (
              <div>
                  <p>我是父组件--{this.state.fatherMsg}</p>
                  {/*将父组件中的方法传递子组件*/}
                  <Ninechild fun1={this.transferChildFn}></Ninechild>
              </div>
          );
      }
      transferChildFn = (data) => {
          console.log(data); // 该data数据就是子组件传递过来的数据
          this.setState({
              fatherMsg: data
          })
      }
  }
  export default Nineclass;
  
  //子组件
  import React, { Component } from 'react';
  class Ninechild extends Component {
      state = {
          msg: '我是子组件的数据'
      }
      render() {
          return (
              <div>
                  <p>我是子组件</p>
              </div>
          );
      };
      componentDidMount() {
        this.props.fun1(this.state.msg) //在子组件中调用父组件传过来的方法,并将子组件数据传递给父组件
      }
  }
  export default Ninechild;
  ```

  **方式二:** 在子组件中定义一个方法, 该方法的返回值为子组件的数据, 然后在父组件中给子组件绑定ref属性,获取该子组件上的方法,并调用拿到子组件的数据

  ```jsx
  // 父组件:
  import React, { Component, createRef } from 'react';
  import Ninechild from '../components/nineChild';
  class Nineclass extends Component {
      state = {
          fatherMsg: '123'
      }
      ref1 = createRef()
      render() {
          return (
              <div>
                  <p>我是父组件--{this.state.fatherMsg}</p>
                  {/*父组件通过ref属性获取子组件的实例*/}
                  <Ninechild ref={this.ref1}></Ninechild>
              </div>
          );
      }
      componentDidMount() {
          // console.log(this.ref1.current.sendFatherFn())
          this.setState({
              fatherMsg: this.ref1.current.sendFatherFn() // 使用子组件的数据
          })
      }
  }
  export default Nineclass;
  
  // 子组件: 
  import React, { Component } from 'react';
  class Ninechild extends Component {
      state = {
          msg: '我是子组件的数据'
      }
      render() {
          return (
              <div>
                  <p>我是子组件</p>
              </div>
          );
      };
      sendFatherFn() {
          return this.state.msg // 子组件的该方法将子组件的数据返回,这样父组件就可以获取到子组件的数据了
      }
  }
  export default Ninechild;
  ```

  

## 3、跨组件通信（数据共享）   

网址：https://zh-hans.reactjs.org/docs/context.html  

在react没有类似vue中的provider和inject(依赖注入)来解决这个问题。在实际的项目中，当需要组件间跨级访问信息时，如果还使用组件层层传递props，此时代码显得不那么优雅，甚至有些冗余。在react中，我们还可以使用context来实现**跨级父子组件间的通信**。

函数组件的 使用context 实现的跨组件通信相对简单些,后面讲到函数组件说.

~~~javascript
import React, { Component, createContext } from "react"

const {
	Provider,
	Consumer
} = createContext()
~~~

> 提示：在React的context中，数据被看成了商品，发布数据的组件会用provider身份，接收数据的组件使用consumer身份。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/6e6a35f08f8620236171af7b7df86c6e497fad47.png?sign=75f71781d763be66f76b1ff2a37791d1&t=5f98f468)

- 创建Context对象

Context需要在父级或祖先级组件中使用,同时也需要在获取数据的子组件中使用, 所以需要将该对象定义成全局对象(即定义一个单独的js文件中), 然后在父级/祖先级 组件中引入,同时也在获取数据的子组件中引入.

~~~javascript
// 定义全局context
// 由于这个操作后期可能被复用，建议独立文件去创建。此处以`src/Context/index.js`为例
import { createContext } from "react"
export default createContext() // createContext函数的的参数就是提供的默认值,如果后续没有给provider赋值value属性,那么 createContext({a:1,b:2,c:3});{a:1,b:2,c:3}就是value的默认值
~~~

- 发布消息

在App.jsx组件中发布消息，这样所有的组件都可以消费它的消息。

~~~jsx
import React, { Component } from "react";
import Cmp1 from "./Components/Cmp1";
import Cmp2 from "./Components/Cmp2";
// 导入context对象
import ContextObj from "./Context/index";
let { Provider } = ContextObj;


class App extends Component {
    state = {
        count: 12345,
    };

    render() {
        return (
            <div>
                {/* 将需要接受数据的子组件放到Provider组件内,否则子组件接受不到数据
                value 属性指定要传递的数据*/}
                <Provider value={this.state.count}>
                    <Cmp6></Cmp6>
                    <Cmp7></Cmp7>
                </Provider>
            </div>
        );
    }
}

export default App;
~~~

- 组件消费

在子组件中通过Api完成消费动作，从而实现消息通信。消费的方式有2种：

方式1：通过组件消费

~~~jsx
import React, { Component } from "react";

import ContextObj from "../Context/index";
let { Consumer } = ContextObj;

class Cmp1 extends Component {
    render() {
        return (
            <div>
                <Consumer>
                     {/* 注意:Consumer组件中只能是函数形式, */}
                    {(value) => {
                        return <div>获取到的值是：{value}</div>;
                    }}
                </Consumer>
            </div>
        );
    }
}

export default Cmp1;
~~~

方式2：通过绑定静态成员属性来消费

~~~jsx
import React, { Component } from "react";
import ContextObj from "../Context/index";

class Cmp2 extends Component {
    static contextType = ContextObj; //contextType 是内置固定属性,不能修改
    render() {
         // this.context 就是共享的数据, 因为context在赋值给静态成员属性contextType时,做了一个属性映			射,将context内的context属性映射到组件实例上,所以如下就可以通过this.context获取对应的数据
        return <div>{this.context}</div>;
    }
}

export default Cmp2;
~~~



## 4、兄弟组件间通信

这里我们采用自定义事件的方式来实现非嵌套组件间的通信。

我们需要使用一个 events 包：第三方的包不属于react全家桶的包

```undefined
npm install events --save
```

新建一个 ev.js，引入 events 包，并向外提供一个事件对象，供通信时使用：

```cpp
import { EventEmitter } from "events";
export default new EventEmitter();
```

父组件：

```jsx
import React, { Component } from 'react';
import Child1 from "./Foo";
import Child2 from "./Boo";

export default class App extends Component{
    render(){
        return(
            <div>
                <Child1 />
                <Child2 />
            </div>
        );
    }
} 
```

Child1组件:

```JSX
import React,{ Component } from "react";
import Bus from "./ev"
export default class Boo extends Component{
    emitFn = () => {
        // 触发 (自定义事件,传递的参数)   和vue一样
        Bus.emit("transfer",this.state.msg)
    }
    state={
        msg:'舒克'
    }
    render()
        return(
            <div>
                 组件1
                <button onClick = {()=> this.emitFn() }>点击我</button>
            </div>
        );
    }
}
```

Child2组件:

```jsx
import React,{ Component } from "react";
import Bus from "./ev"
export default class Foo extends Component{
    getData = (msg) => {
         console.log(msg) // 这就是传递过来的参数
    }
    componentDidMount(){
        // 组价2 中接收组件1传过来的自定义事件
        Bus.on("transfer",this.getData);
    }
    render(){
        return(
            <div>
                 组件2
            </div>
        );
    }
}
```

自定义事件是典型的发布/订阅模式，通过向事件对象上添加监听器和触发事件来实现组件间通信。



# 十一、HOC(重点)

Higher - Order Components：在原有组件基础之上**加工**后新生成得到的新组件。【高阶组件】

 **高阶组件是参数为组件，返回值为新组件的函数** 

~~~jsx
const NewComponent = HOC(YourComponent) 
~~~

通俗的来讲，`高阶组件`就相当于手机壳，通过包装组件，增强组件功能。例如最近成热点的华为Mate 40`环删保护壳`： 也可以将高阶组件理解为一个包装器

![](https://storage.lynnn.cn/assets/markdown/91147/documents/2020/10/4f524eb810cea9ceacbe324e73a770343a267593.jpeg?sign=ec3e937178bb72dea4305f755f7d8092&t=5f9993ca)

HOC实现步骤：

- 创建一个函数

- 指定函数参数，**参数应该以大写字母开头**

- 在函数内部创建一个类组件，提供**复用**的状态（如有）及逻辑代码，并返回

- 在该组件中，渲染参数组件，同时将状态通过prop传递给参数组件（可选，如有）

- 调用该高阶组件方法，传入要增强的组件，通过返回值拿到增强后的组件，并将其渲染到页面 

比如，我们想要我们的组件通过自动注入一个版权信息：

~~~jsx
// 如下结构和函数的组件结构很相似,所以可以使用rfc,创建一个函数组件然后在此基础上修改
import React, { Component, Fragment } from "react";
const withCopyright = (Cmp) => {
    return class Hoc extends Component {
        render() {
            return (
                <Fragment>
                    <Cmp></Cmp>
                    <div>&copy; 2020 千峰教育</div>
                </Fragment>
            );
        }
    };
};

export default withCopyright;

// Fragment是一个伪标签，渲染的时候是不会显示在页面中的，因此也不会影响视图显示
// 此处注意:如果该Cmp 组件 在使用withCopyright 方法之前,是作为它的父组件下的子组件,并且接受了来组父组件和参数props,那么通过 withCopyright 方法,导致原先的父子组件变成了 爷孙组件,且孙组件无法接受爷组件的数据,此时了使用中间组件传递一下,而withCopyright 返回的组件就是中间组件 写法为:
<Fragment>
	<Cmp {...this.props}></Cmp>
	<div>&copy; 2020 千峰教育</div>
</Fragment>
~~~

使用方式：

~~~jsx
import React, { Component } from "react";
// 引入HOC函数
import Hoc from './Hoc/Hoc_copyright';

class App extends Component {
    render() {
        return (
            <div>
                <h1>网站首页</h1>
            </div>
        );
    }
}

export default Hoc(App);
~~~

这样只要我们有需要用到版权信息的组件，都可以直接使用withCopyright这个高阶组件包裹即可。

**高阶组件使用案例**:  使用hoc封装组件

封装一个loading提示组件; 当网络请求的数据比较慢,没有返回数据之前,显示loading, 返回数据后渲染数据

01:定义hoc 高阶组件

```jsx
import React, { Component } from 'react';
const Hoc = (Com) => {
    return class Newcom extends Component {
        render() {
            console.log('props', this.props);
            return <>
                {
                    this.props.schoolArr.length > 0 ? <Com {...this.props}></Com> : 	
                    <div>loading......</div>
                }
            </>
        }
    }
}

export default Hoc;
```

02: 定义子组件

```jsx
import React, { Component } from 'react';
import Hoc from './hoc';
// 定义子组件
class Myclschild extends Component {
    render() {
        return (
            <div>
                我是子组件
                <ul>
                    {
                        this.props.schoolArr.map((item) => {
                            return <li key={item.id}>{item.school_name}</li>
                        })
                    }
                </ul>
            </div>
        );
    }
}
export default Hoc(Myclschild);
```

03: 定义父组件

```jsx
import React, { Component } from 'react';
// 引入子组件
import Myclschild from './Myclschild';
import axios from 'axios';
class Mycls extends Component {
    state = {
        schoolArr: []
    }
    render() {
        return (
            <div>
                {/* 使用子组件,传递axios 返回的数据 */}
                <Myclschild schoolArr={this.state.schoolArr}></Myclschild>
            </div>
        );
    };
    componentDidMount() {
        // 请求数据
        axios.get('https://api.i-lynn.cn/college').then(res => {
            // console.log(res);
            this.setState({
                schoolArr: res.data.data.list1
            })
        })
    }
}

export default Mycls;
```



# 十二、Redux（重点）

## 1、简介 

2013年Facebook提出了Flux架构的思想，引发了很多的实现。2015年，Redux出现，将Flux与函数式编程结合一起，很短时间内就成为了最热门的前端架构。

简单说，如果你的UI层非常简单，没有很多互动，Redux就是不必要的，用了反而增加复杂性。

如果你的项目的迭代变得越来越复杂，组件的数量和层级也变得越来越多，越来越深，此时组件间的数据通信就变得异常的复杂和低效，为了解决这个问题，引入了状态管理（**redux**）从而很好的解决多组件之间的通信问题。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/a6087b34b9f365ef23b0a53fb6ca225099d5cfb6.png?sign=fbbd37dae9446bbcef3b79c60c2d31e3&t=5f99a237)

如果需要使用Redux请先进行安装：

网址：https://redux.js.org/introduction/getting-started

~~~shell
npm i -S redux
~~~

作用：项目进行大规模数据管理的工具。

提醒：所有的代码需要从0开始写（不像vuex，选择了vuex后会自带一些代码）



## 2、三大原则（重点）

- 单一数据源
  - 整个应用的`state`（这个state不是组件中的state，请不要混淆）被储存在一棵对象结构树中，并且这个对象结构只存在于唯一一个store中
- State是只读（相对，相对旧数据）的
  - 唯一改变state的方法就是触发dispatch+action，action是一个用于描述已发生事件的**普通对象**（action普通对象必须要有`type`属性，值是什么无所谓，其余属性也无所谓）。
- （最终修改数据的方法）使用**纯函数**（一个函数的返回结果**只**受到其形参的影响，并且形参不能被修改,则其就是纯函数）来执行修改store中的数据
  - 为了描述action如何改变state tree ，我们需要编写reducer【**看作是vuex中的mutations里的方法】，==reducer必须是纯函数==，它接收先前的state和action，并返回**新的state（不会合并的，自行注意这个坑）

> 纯函数是函数式编程的概念，必须遵守以下一些约束。
>
> - 不得改写参数
>
> - 不能调用系统 I/O 的API
>
> - 不能调用Date.now()或者Math.random()等不纯的方法，因为每次会得到不一样的结果

请注意：由于reducer被要求是纯函数，所以reducer函数里面不能改变State，必须返回一个全新的数据（不会自动合并原始数据的，因此一定要注意：别把原始数据搞丢了）。

**操作原理图**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/c2b874cdd9bc6f0579afebeedc5f958131a78040.png?sign=ec995f5c878596b98a9e1b3827aa4c4c&t=5f99b163)

a. store通过reducer创建了初始状态

b. view通过store.getState() 获取到了store中保存的state挂载在了自己的状态上

c. 用户产生了操作（事件），调用了actions 的方法

d. actions的方法被调用，创建了带有标示性信息的action（描述对象，描述如何修改数据）

e. store.dispatch方法发送actions任务到了reducer中

f. reducer接收到action并根据标识信息判断之后返回了新的state（自己注意合并的问题）

g. store的state被reducer更改为新state的时候，store.subscribe方法里的回调函数会执行，此时就可以通知view去重新获取state

- store.getState()：用于获取仓库中初始的数据（一次性）
- store.dispatch()：用于派发修改数据的任务，参数是action普通对象
- store.subscribe(callback)：视图组件用于订阅新数据的方法（二次及以后的数据更新，使之产生类似于vue的响应式store数据）



## 3、redux的基本使用

**案例：在组件中展示一个按钮，点按钮后给redux中的数字+9，数字初始为0。实现一个计数器的效果**

步骤：

- 创建store
- 创建视图组件（展示store中的数据）
- 修改
- 回显数据到视图组件



**实现步骤**

a. 创建默认数据源：

~~~js
// 1. 这是仓库store/index.js

// a. 导入createStore方法，作用用于产生仓库
import { createStore } from "redux";

// b. 创建数据源, 默认数据源是一个普通对象，可以有很多的数据
const defaultState = {
    // 定义初始化的数据
    count: 0,
    num:100
};

// c. 创建纯函数reducer（函数名随意）
// 作用：负责返回state（如果不涉及数据修改,直接返回state,如果设置修改数据,则返回修改完的state）
// 语法：reducer(state = defaultState,actions)
function reducer(state = defaultState, actions) {
    //该处为修改数据源的操作
     if (actions.type === '+') {
        return {
            ...state, // 保留旧数据,否则修饰,因为reducer方法不是修改state,而是替换state
            count: state.count + actions.payload
        }
    }
    return state;
}

// d. 创建仓库
// 产生仓库的时候需要往仓库中存放数据源，因此需要传递上面定义的reducer函数
const store = createStore(reducer);

// e. 导出仓库
export default store;

~~~

为了方便调试redux（可选安装)，建议去谷歌商店安装`redux devtools`(或到插件迷网站下载)，在使用的时候需要参考其[说明页面](https://github.com/zalmoxisus/redux-devtools-extension#usage)

> redux工具条在安装好之后不能直接使用，需要配置仓库代码，然后才能使用。

~~~js
// d. 产生仓库
// 产生仓库的时候需要往仓库中存放数据源，因此需要传递reducer过去
const store = createStore(
    reducer,
    // 必须要加上一段插件的配置工具，才能在浏览器中使用redux扩展,才能看到redux中state数据.
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
~~~

显示效果：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/01/8a98ff5a63061eb039045fe1c42cd955d518311c.png?sign=7f00fc5ce078fb8192e4014f51649cb6&t=600a9550)



b. 建立视图组件并且展示数据源 

~~~react
import React, { Component } from "react";
// 需要导入store;
import store from "../store/index";
class Counter extends Component {
    // 在constructor中获取store中的数据
    constructor(props) {
        super(props);
        // 获取store数据（一次性，不具备响应式）
        this.state = store.getState(); 
        // 重新订阅store中的数据更新state, 每次修改store中的数据都会触发该方法
        // 非简写方式01:
        store.subscribe(() => {
            //console.log('subscribe');
            this.setState(() => {
                return store.getState()
            })
        })
        // 简写方式02:
        store.subscribe(() => this.setState(() => store.getState()));
    }
    render() {
        console.log(this.state);
        return (
            <div>
                <div>当前Store中的数据是：{this.state.count}</div>
                <button onClick={this.addCount.bind(this)}>点击+9</button>
                <hr />
                <div>当前Store中的数据是：{this.state.age}</div>
                <button onClick={this.addAge.bind(this)}>点击+1</button>
            </div>
        );
    }

    // 点击+9
    addCount() {
        // 描述数据如何更改的对象，其必须有type属性
        let action = { type: "mod_count", payload: 9 };
        // 通过store.dispatch去派发actionr任务（会将该action派发给所有的reducer也就是调用reducer方			法，每个reducer都接收action任务,然后判断执行，因此一定要注意type的取值）
        store.dispatch(action);
    }

    // 点击+1
    addAge() {
        let action = { type: "mod_age", payload: 1 };
        store.dispatch(action);
    }
}

export default Counter;
~~~

c. 修改操作

视图组件中的代码：

~~~js
handler() {
    // 3. +9这个修改操作需要通过普通对象去描述（actions）
    const action = {
        // type是用于在reducer方法中做条件判断用的
        type: "add",
        // 另一个属性用于声明本次修改具体的值是多少
        payload: 9,
    };
    // 派发修改任务
    store.dispatch(action);
}
~~~



仓库文件的代码：

~~~js
function reducer(state = defaultState, actions) {
    console.log(actions);
    //判断是否是加法操作
    if (actions.type === "add") {
        return { 
            ...state, // 记得携带旧数据,否则新返回的对象中是没有旧数据的
            count: state.count + actions.payload //actions.payload获取到参数,该count变量会覆													 盖掉上面我的count 变量
        };
    }
    // 在返回之前写修改数据源的操作
    return state;
}
~~~

d. 回显新的数据

~~~js
// 构造函数
constructor(props) {
    super(props);
    // 2. 在视图组件中获取初始的仓库数据
    // getState()方法是store对象内置的方法
    this.state = store.getState();
    // 4. 订阅新的数据(当state中的数据被修改,会触发subscribe内的箭头函数callback,		   	   
         //store.subscribe(callback)
    store.subscribe(() => {
        // 获取新数据修改当前的state
        this.setState(() => store.getState());
    });
}
~~~



f. 当处理多个reducer 方法时,由于不同的reducer 操作的是不同模块的数据,所以需要单独维护

```js
// 定义store 仓库数据
// 01: 引入 combineReducers 这个方法 将多个reducers 结合成一个reducer
import { legacy_createStore as createStore, combineReducers } from 'redux';

// 02: 定义初始数据
const defaultState = {
    count: 0,
    num: 100
}

//03: 创建纯函数 修改state 数据
function reducer(state = defaultState, actions) {
    if (actions.type == '+') {
        return {
            ...state,
            count: state.count + actions.payload
        }
    }

    return state
}
// 另一个reducer 维护自己的模块数据
function reducer1(state = defaultState, actions) {
    if (actions.type == 'jia') {
        return {
            ...state,
            num: state.num + actions.payload
        }
    }

    return state
}
// 04: 创建仓库,使用combineReducers 将多个reducer结合成一个reducer,
const store = createStore(combineReducers({ reducer, reducer1 }), window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());


//05: 导出仓库 
export default store

```

f1. 多个reduce在页面中显示数据并操作

```jsx
import React, { Component } from 'react';
import store from '../../store';
console.log(store);
class Test extends Component {
    constructor() {
        super();
        this.state = store.getState();
        console.log(this.state);
        store.subscribe(() => {
            this.setState(() => {
                return store.getState()
            })
        })
    }
    render() {
        return (
            <div>
                我是test 组件
                <p>{this.state.reducer.count}<button onClick={this.addCount}>+1</button></p>
                <p>{this.state.reducer1.num}<button onClick={this.addNum}>+10</button></p>
            </div>
        );
    }

    addCount = () => {
        store.dispatch({ type: '+', payload: 1 })
    }
    addNum = () => {
        store.dispatch({ type: 'jia', payload: 10 })
    }
}

export default Test;
```



## 4、redux-thunk

当redux 中操作异步的actions 时, 可以需要借助 redux-thunk 这个中间件,  异步操作redux 类比咱们vuex 中异步修改store 仓库中的数据. 需要在异步action中调用同步的mutations

01: 首先需要将 安装 一个异步操作redux时需要的中间件,不安装后期控制台报错提示安装中间件

```shell
npm i redux-thunk
```

02: 然后将所有的页面中派发的action统一管理, 便于后面的模块化拆分

在store 目录下新建actions目录, 将如上页面中的所有配发修改reducer的actions 任务都统一 管理

```js
// 注意; 
// 同步的action 是普通对象, { type: '+', payload }
// 异步的action 是函数,该函数由store自动调用执行, 函数执行再调用同步的action (这点和vuex 中的action 有点类似)

export function addCountAction(payload){
  	return { type: '+', payload }
}

export function addNumAction(payload){
  	return { type: 'jia', payload }
}

// 异步修改 
export function addNumAsyncAction(payload){
    // 异步action 返回的是一个函数, 写法上属于闭包的柯里化写法. 
    // 当然也可以将其修改为一个axios 数据请求
  	return (dispatch)=>{
        setTimeOut(()=>{
            dispatch(addNumAction(payload)) 
        },1000)
    }
    
    // return (dispatch) => {
    //     axios.get('https://api.i-lynn.cn/college?page=1').then(res => {
    //         // console.log(res);
    //         dispatch(editlistActions(res.data.data.list1))
    //     })
    // }
}
```

03:  修改store 下的index.js 文件 

```js

// 定义store数据

// 引入applyMiddleware 中间件,使用该中间件应用redux-thunk
// 引入 compose 为了处理 redux-devtool 还可以再浏览器调试工具中正常显示.(这个配置参照上面给出的redux调试工具中代码具体的修改部分 )
import { legacy_createStore as createStore, applyMiddleware, compose } from 'redux';
// 引入 redux-thunk 异步插件
import reduxthunk from 'redux-thunk';

// 02: 定义初始数据
const defaultState = {
    count: 0,
    num: 100
}

//03: 创建纯函数 修改state 数据
function reducer(state = defaultState, actions) {
    if (actions.type == '+') {
        return {
            ...state,
            count: state.count + actions.payload
        }
    }

    return state
}
// 另一个reducer 维护自己的模块数据
function reducer1(state = defaultState, actions) {
    if (actions.type == 'jia') {
        return {
            ...state,
            num: state.num + actions.payload
        }
    }
   
    // 如果没有符合的action则返回原state 对象
    return state
}

// 定义redux-devtools配置
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
    combineReducers({ reducer, reducer1 },
    // 配置异步thunk 和 redux-devtools
    composeEnhancers(applyMiddleware(reduxthunk))

)

// 第五步: 导出store
export default store

```

04: 在页面中使用 

```jsx
import React, { Component } from 'react';
import store from '../../store';
// console.log(store);
//引入 对应的action 
import {addCountAction,addNumAction,addNumAsyncAction} from '../../store/actions'
 class Test extends Component {
    constructor() {
        super();
        this.state = store.getState();
        // console.log(this.state);
        store.subscribe(() => {
            this.setState(() => {
                return store.getState()
            })
        })
    }
    render() {
        return (
            <div>
                我是test 组件
                <p>{this.state.reducer.count}<button onClick={this.addCount}>+1</button></p>
                <p>{this.state.reducer1.num}<button onClick={this.addNum}>+10</button></p>	
                {/*异步修改store中的num*/}
              <p>{this.state.reducer1.num}<button onClick={this.addNumAsync}>+10</button></p>
            </div>
        );
    }

    addCount = () => {
        store.dispatch(addCountAction(1))
    }
    addNum = () => {
        store.dispatch(addNumAction(10))
    },
    addNumAsync = () => {
        //派发异步action任务
        store.dispatch(addNumAsyncAction(100))
    }
}

export default Test;
```





## 5、react-redux(便于全局使用)

网址：https://react-redux.js.org/

React-Redux是Redux的官方针对React开发的扩展库，默认没有在React项目中安装，需要手动来安装。react-redux是依赖于redux，所以必须先安装redux。

我们可以理解为react-redux就是redux给我们提供的高阶组件加工厂。

使用react-redux 可以很方便我们去操作redux

~~~shell
npm i -S react-redux
~~~

React-redux所能解决的问题是：

- 每个组件使用仓库的时候,需要 import store from '路径' ,仓库store没有实现全局化(而vue在每个组件可以直接访问全局数据,无需导入 this.$store.state)

- 使用它以后我们不需要在每个组件中再去 手动订阅数据的更新了store.subscribe(...)(vue不需要) 。
- 为了解决上述问题. 使用本节的react-redux, 但该方法并不是为了简化代码的，它们存在的意义是解决前面所遇到的问题

**使用步骤**

- 在项目入口文件中定义Provider

  - 该步骤的操作有点类似于之前组件通信中的context那块的操作

  - 将整个仓库作为商品提供给App根组件，后续的所有的组件都可以获取到仓库store中的数据

  - 注意：与context不一样，这里绑定数据使用的属性是“store”

  - 入口 index.js 文件中的示例代码：

  - ~~~js
    // 导入
    import React from "react";
    import ReactDOM from "react-dom";
    // 导入provider
    import { Provider } from "react-redux";
    import store from "./store/index";
    
    // 导入需要展示的组件
    import App from "./Login";
    
    // 渲染视图
    // 在展示app组件的时候需要按照组件的形式进行操作
    ReactDOM.render(
        //使用Provider组件包裹,将要共享的数据设置为store属性,这个每个后代组件都能获取到该store数据了
        // 此时 store 数据就实现了全局化了
        <Provider store={store}>
            <App></App>
        </Provider>,
        document.getElementById("root")
    );
    ~~~

- 在需要使用redux的组件中使用

  - 这个步骤与vuex中map系列函数（mapState，mapMutations，mapActions、mapGetters）的思想是一样的

  - 思想：将仓库中的属性和方法映射成当前组件自身的属性和方法

  - 在实际使用的时候组件中不再需要使用store对象了（包括初始的获取数据：store.getState()、store.dispatch(）、store.subscribe()）

  - 步骤

    - 在需要使用redux的组件前面导入react-redux提供的高阶组件：connect

    - 编写映射方法（请注意，这个方法映射不是类组件的方法，而是在类组件外写的方法）

      - mapStateToProps(state)
        - 作用：将仓库中的state数据源映射成本组件的属性props，返回一个props对象
        - 参数：仓库中的state
      - mapDispatchToProps(dispatch)
        - 作用：将派发action的方法映射成当前组件自身的属性，该方法也要求返回一个对象，该对象中存放的就是派发action的方法集合
        - 参数：dispatch如同之前的store.dispatch()
      - 编写时，可以写箭头函数，也可以写常规函数

    - 应用高阶组件connect，写法是固定的 connect(参数1,参数2)(组件名)

      - ~~~js
        // 在组件最后导出的时候改写成如下：
        // 从react-redux 导入的connect 参数1,参数2 固定的
        // 参数1为mapStateToProps 函数,且该函数必须返回一个对象,否则报错
        // 参数2为mapDispatchToProps 函数, 且该函数必须返回一个对象,否则报错
        export default connect(mapStateToProps,mapDispatchToProps)(ComponentName)
        ~~~
  
  - 组件中实际使用时的参考代码：以jsx为例
  
  - ~~~react
    import React, { Component } from "react";
    // 第一步：在需要使用redux组件中导入一个由react-redux提供的hoc
    import { connect } from "react-redux";
    class Counter extends Component {
        render() {
            return (
                <div>
                    <div>当前Store中的数据是：{this.props.tool.count}</div>
                    <button onClick={this.props.addCount}>点击+9</button>
                    <hr />
                    <div>当前Store中的数据是：{this.props.user.age}</div>
                    <button onClick={this.props.addAge}>点击+1</button>
                </div>
            );
        }
    }
    
    // 第二步：在类外面定义俩个映射方法
    // 将redux中的state数据源属性映射到本组件自身的props属性中
    function mapStateToProps(state) {
        // return state.user;
        // return state.tool;
        return state;
    }
    // 将修改数据的方法dispatch映射成自身组件的props属性
    function mapDispatchToProps(dispatch) {
        // 该方法返回一个对象，对象中都是方法
        return {
            addCount() {
                dispatch({type:'+',payload:1});
            },
            addAge() {
                dispatch({type:'jia',payload:2});
            },
        };
    }
    
    // 第三步：应用HOC
    // connect函数的俩个参数顺序不能颠倒
    export default connect(mapStateToProps, mapDispatchToProps)(Counter);
    ~~~




## 6、Redux模块化

> 如果redux需要使用多个模块，请使用combineReducers方法将多个reudcer函数进行组合，得到一个根reducer对象，将对象传递给创建仓库的方法：https://redux.js.org/tutorials/fundamentals/part-3-state-actions-reducers#combinereducers

针对redux的模块化，在一个常规项目中会将其代码拆分成以下几个部分：

- Reducers，建立同名目录，存放模块化之后的reducer 
- types : 建立同名目录,存放types (这样可以集中管理type)  

具体实现，以项目的代码为准。

```jsx
//store 目录下的index.js 

// 1.导入createStore 方法 用它来创建store
import { createStore, combineReducers } from 'redux';
// 2. 创建一个默认数据对象, 可以定义多个数据
const defaultState = {
    count: 0, // 该数据为user模块中的数据
    num: 100  // 该数据为shoppingCar模块中的数据
}
// dispatch派发一个actions时,所有的reducer方法都被执行,所以reducer中的type值都不一样
//3. 创建 user模块的 reducer 纯函数, 返回state 对象
function user(state = defaultState, actions) {
    // 此处可以操作state中的数据,
    if (actions.type === '+') {
        return {
            ...state,
            count: state.count + actions.payload
        }
    }
    if (actions.type === '-') {
        //console.log(3, state.count, actions.payload)
        return {
            ...state,
            count: state.count - actions.payload
        }
    }
    return state
}

//4. 创建shoppingCar模块的reducer纯函数,返回state 对象
function shoppingCar(state = defaultState, actions) {
    if (actions.type === 'add') {
        return {
            ...state,
            num: state.num + actions.payload
        }
    }
    return state
}
// 5. 使用combineReducers 可以将多有的reducer方法进行合并,参数为对象,对象的key为模块的名称
// 有点类似于vuex的命名空间
const reducers = combineReducers({ user: user, shoppingCar: shoppingCar })
// 6. 创建仓库
const store = createStore(reducers, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
// 7. 导出仓库
export default store
```

```jsx
// 页面操作部分
import React, { Component } from 'react';
import { connect } from 'react-redux';
class Thirteenclass extends Component {
    render() {
        return (
            <div>
                <h1>测试redux</h1>
                <p>store中的数据count: {this.props.user.count}</p>
                <p>
                    <button onClick={this.props.addFn}>+1</button>
                </p>
                <hr />
                <p>store中的数据num: {this.props.shoppingCar.num}</p>
                <p>
                    <button onClick={this.props.addNum}>+1</button>
                </p>
            </div>
        );
    }
}
// 将state 映射成自身组件的props
function mapStateToProps(state) {
    return state
}
// 将dispatch映射成自身组件的props
function mapDispatchToProps(dispatch) {
    return {
        addFn() {
            // 同步调用
            //dispatch({ type: '+', payload: 1 }) 
            // 异步调用 如下:payload的值为接口返回的数据,不是固定的值(该接口返回一个随机数)
            fetch('https://api.i-lynn.cn/rand').then(res => res.json()).then(res => {
                dispatch({ type: '+', payload: res.payload })
            })
        },
        addNum() {
            dispatch({ type: 'add', payload: 1 })
        }
    }
}
// 应用高阶组件connect，写法是固定的 connect(mapStateToProps,mapDispatchToProps)(组件名)
export default connect(mapStateToProps, mapDispatchToProps)(Thirteenclass);
```

模块化目录及文件内容实例:

~~~js
//第一步: 拆分types, store目录下/types/index.js(这样便于集中化管理
const types = {
    addCount: '+',
    addNum: 'add'
}
export default types

// 第二步: 拆分 reducers, store目录下/reducers/ 不同的模块创建不同的js并导出该函数
#reducers目录
 #user.js文件
 	import types from "../types/types"
    const defaultState = {
        count: 0,
        num: 100
    }
	//创建reducer 纯函数, 返回state 对象,
	function user(state = defaultState, actions) {
        // 此处可以操作state中的数据
        if (actions.type === types.addCount) {
            return {
                ...state,
                count: state.count + actions.payload
            }
        }
        return state
    }
	export default user
 #shoppingCar.js文件
   // 代码同user.js
 
 
 // 第三步: 拆分actions,将页面中的所有触发reducer的actions 统一管理
 
~~~





# 十三、路由

声明：本次使用的路由包版本是5.3.3【当前最新6.0】

## 1、介绍

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/cea272f101176b948ac627959723ae3a93a0e4b9.png?sign=a44bce7e29d23549df61fd1be8bbc8ea&t=5f9abd07)



React Router官网：https://reactrouter.com/

使用用React Router前需要先进行安装：

~~~shell
npm i react-router-dom@5
~~~

React Router现在的主版本是5，思想：**一切皆组件**。



## 2、路由的使用 

### 2.1、相关组件 

如前面介绍里说的，自Router 4之后的思想是`一切皆组件`，所以在正式开始学习React路由前需要先对几个组件要有所掌握：

- **Router组件**（别名，真实是不存在的，为了简写路由模式的组件名称）：**包裹整个应用**（单个具体的组件/**根组件**），一个React应用只需要使用一次
  
  - 注意：**在react5中，不存在类似于vue的路由配置文件，对于前端路由模式的选择，我们可以通过该组件完成**
  - Router类型： HashRouter和BrowserRouter
    - HashRouter： 使用URL的哈希值实现 （localhost:3000/#/first）
    - BrowserRouter：使用H5的history API实现（localhost:3000/first）
  - **区别**：
    - 两者在开发阶段，除了地址栏上的表现形式不一样以外，其它没区别
    - 两者在生产阶段，hash路由可以直接上生产，无需做任何配置，而history模式则上生产需要配置的，配置服务器环境 (后台人员服务器配置一下如果404,指定index.html或首页)，否则项目是不能刷新的，一刷新就会404 
- **Link**/navLink组件：用于指定导航链接（a标签）就是做声明式导航的（类似于vue中的`router-link组件`）
  
  - 最终Link会编译成a标签，而to属性会被编译成 a标签的href属性
- ##### **Route组件**：既可以编写路由规则同时也是路由规则对应的组件的展示容器【路由规则】{path: xx,component:xxx}
  
  - path属性：路由规则，这里需要跟Link组件里面to属性的值一致
  - component属性：展示的组件
  - 语法：<Route path="路径" component={组件}></Route>
  - 该组件除了具备定义路由规则功能外，还有类似于vue中`router-view`的功能

**各个组件之间的关系**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/2bdaba50f309c0a59921c7e188597c744931f703.png?sign=6e90bed85333f028465d58ca7faae7c7&t=5f9abe65)

> **注意：`Link`和`Route`组件必须被`Router`组件给包裹，否则报错。**



### 2.2、声明式导航

- 在`src/index.js`主入口文件中定义一个路由模式（可选，也可以在具体的某个组件中使用Router）

~~~jsx
import React from "react";
import ReactDOM from "react-dom";

// 设置路由模式
import {BrowserRouter as Router} from 'react-router-dom'

// 定义 provider
import { Provider } from "react-redux";
import store from "./Store/index";

import App from "./App";

ReactDOM.render(
    <Provider store={store}>
        {/* 使用Router包裹根组件,这样全局都可以使用该路由 */}
        <Router>
            <App></App>
        </Router>
    </Provider>,
    document.getElementById("root")
);
~~~

- 在 src/router/index.jsx中定义路由规则,使用Route 组件,一般语法: <Route path='路径' component={组件}></Route>


~~~jsx
//  定义路由规则:

// 由于react中定义路由规则是通过组件实现的,所以首先创建一个组件(函数组件和类组件都可以)
// 由于路由不涉及到状态,生命周期,所以使用函数组件更加简单
import React from 'react';
// 1. 引入路由组件Route
import { Route } from 'react-router-dom';
// 2. 引入路由地址对应的页面组件
import News from '../views/New'
import About from '../views/About'
//3. 配置路由规则(类似于vue中的routes)
const Index = () => {
    return (
        <>
            <Route path='/news' component={News}></Route>
            <Route path='/about' component={About}></Route>
        </>
    );
}

export default Index;
~~~

- 在根组件中引入路由规则(**除其他特殊的路由规则外(嵌套路由规则),其他普通的路由规则都需要在根组件中使用**)

```jsx
import React, { Component } from 'react';
// 引入link组件 相当于a标签实现页面跳转
import { Link } from 'react-router-dom';
// 引入路由规则组件
import Rules from './router/Index'
class App extends Component {
  render() {
    return (
      <div>
        <ul>
           <Link to='/news'><li>新闻</li></Link>
           <Link to='/about?id=100'><li>关于</li></Link>
            {/*
				传参方式1:search, 在对应页面中使用props.location.search
                传参方式2:state 隐式传参,该方法参数不会出现在地址栏,用户看不到,在对应页面		   						    props.location.state , 该方法可以作为页面埋点的方式
			*/}
           <Link to={{ pathname: '/news', search: 'id=99', state: { a: 1, b: 2 } }}>新闻				</Link>
            {/* 传参方式3:restful动态路由传参 ,需要在路由规则配置参数 <Route path='/news/:id' 					component={News}></Route>,在对应页面拿参 props.match.params*/},
            <Link to={{ pathname: '/news/111'}}>新闻</Link>
        </ul>
        {/*使用路由规则*/}   
        <Rules />
      </div>
    );
  }
}
export default App;
```

在写上述代码时注意，路由自带组件的顺序嵌套关系，组件`<Link></Link>`和组件`<Route></Route>`必须被组件`<Router></Router>`给包裹着。

> 需要注意：
>
> 刨除样式的影响，`Route`组件在HTML代码中的位置决定了渲染后其在页面中显示的位置。如果`Route`放在最后，则其显示的时候也在最后；若其放在渲染内容的最前面，相应的显示也会在最开始。



### 2.3、编程式导航

react-router-dom中通过history对象中的push/go等方法实现编程式导航功能，这一点与之前的vue路由还是很相似的。

形如：

~~~jsx
// 方式1 
this.props.history.push('/home?参数')
// 方式2  
this.props.history.push({
  pathname: "/home",
  search: "from=404",	// 表示传递查询字符串
  state: {				// 隐式传参，地址栏不体现
    username: "admin",
  },
});

//方式3  给定给定的数字（正数或负数）决定去往历史栈中的哪个地址，正数往未来，负数往过去
this.props.history.go(-1)
this.props.history.goBack()
~~~

> 请勿在根组件中写编程式导航，因为根组件默认是没有props对象，因为只有通过路由跳转的组件, 该组件的props 中才有 history, loaction match 这三个属性,否则没有. 解决方法使用 react-router-dom中提供的高阶组件强化函数 withRouter(组件) 被包裹组件就有三个参数了  如下: 
>
> ```jsx
> import {withRouter } from 'react-router-dom';
> 
> export default withRouter(App);
> ```
>
> 

## 3、路由参数

路由参数：在Route定义渲染组件时给定动态绑定的参数。

React路由传参方式有三种：

- ==动态路由参数（param）==
  - 以“/film/detail/:id”形式传递的数据
  - 在目标页面路由中传递
  - 在落地组件中通过`this.props.match.params`得到
  - 一般用于restful规范下的开发
- 查询字符串（query）
  - 通过地址栏中的 `?key=value&key=value`传递
  - 在落地组件中通过`this.props.location.search`得到
  - 由于得到的数据是带“?”的，还需要进一步加工处理之后才能使用，因此建议少用或者不用
- **隐**式传参（state)，通过地址栏是观察不到的
  - 不合适写在声明式导航中，写在编程式导航中更加合适
  - 一般数用于**埋点**数据
    - 简单的讲，埋点是将部分标记隐藏起来，等待用户去触发，因为这个事情不想让用户看到（需要做一些数据的收集，后续做分析），因此会使用隐式传参的方式（大数据分析）
  - 在落地组件中通过`this.props.location.state`得到

接收示例：

~~~jsx
constructor(props){
    super(props)
    this.state = {
        // 接收动态路由参数
        news_id: this.props.match.params.id,
        // 接收查询字符串并处理
        query: querystring.parse(this.props.location.search.slice(1)),
        // 接收state
        state: this.props.location.state
    };
}
~~~



## 4、嵌套路由

~~~js
const routes = [
    // 父路由规则
    {path:'/',redirect:'/home'},
    {
        path: "/admin",
        component: Admin,
        // 子路由规则
        children: [
            {
                path: "/admin/index",
                component: Index
            },
            {
                path: "user",
                compnent: User
            }
        ]
    }
]
~~~

在有一些功能中，往往请求地址的前缀是相同的，不同的只是后面一部份，此时就可以使用多级路由（路由嵌套）来实现此路由的定义实现。

例如，路由规则如下

````
/admin/index
/admin/user
/admin/goods
/admin/add
````

它们路由前缀的admin是相同的，不同的只是后面一部分。

**思想：**

- 借助react路由默认是**非严格匹配模式**的便利（路由规则是：/abc）
  - 例如。上述路由都有`/admin`开头，那么我们可以在路由定义时定义一个组件的路由规则“/admin”。如果这样做，则上述4个路由都会匹配上这个路由规则。匹配的这个组件我们称之为父组件
  
  - 扩展：如果某个路由需要使用严格模式，请在这个路由上加上一个属性：exact
  
    如:  <Route path='/admin' component={Admin} exact />
  
  - 注意: 如果该一级路由有二级路由,那么该**一级路由**一定不能设置严格模式
  - 注意:如果需要匹配**根路由(/)**,那么需要设置严格模式,否则所有的路由都匹配
  - 注意:嵌套路由规则的地址需要将上一级的路由地址拼上,(如 二级路由path = '/一级路由path/二级路由path'
- 再在父组件中写嵌套的子路由的匹配规则

**实现方式**

- 先需要定义个组件，用于负责匹配同一前缀的路由，将匹配到的路由指向到具体的模块

~~~jsx
// 该组件为根组件,修改入口文件的根组件地址,让页面显示当前的组件页面
import React, { Component } from 'react';
import { Route } from 'react-router-dom'
import Admin from './Admin'
class Root extends Component {
    render() {
        return (
            <div>
                我是根组件
                {/*注意:如果该组件有二级路由,那么不能设置严格模式  exact*/ }
                <Route path='/admin' component={Admin} />
            </div>
        );
    }
}

export default Root;
~~~

- 在如上的一级路由对应的组件Admin.jsx中,定义二级路由规则

~~~jsx
import React, { Component } from 'react';
import { Route } from 'react-router-dom';
// 引入子组件Adminone和Admintwo
import Adminone from './adminOne';
import Admintwo from './adminTwo';
class Admin extends Component {
    render() {
        {/*
        	可以将一级路由的path 写活,这样当一级路由的path修改时,不会影响二级路由的path
        	this.props.match.path 为一级路由的path地址
        */}
        let prefix = this.props.match.path //获取一级路由前缀 /admin
        return (
            <div>
                 我是admin 组件
                {/*定义二级路由规则:注意一定需要把一级路由地址拼上*/}
                <Route path='/admin/adminone' component={Adminone}></Route>
                <Route path={prefix+'/admintwo'} component={Admintwo}></Route>
            </div>
        );
    }
}

export default Admin;
~~~

- 创建父路由中的子路由需要的组件



## 5、重定向与404路由

vue中实现路由重定向:

const routes = [

​	{

​     path:'/',
​      redirect:'/home'

​     },

  { 
path:'/mine', 
  component:Mine,
  children:[

​     {path:'', component:Mine1}

  ]   

}

]

### 5.1、重定向路由 

React的重定向路由有以下写法：

> 在重定向的时候需要知道，从哪里来，到哪里去，因此该组件需要使用2个属性：
>
> - from：匹配需要重定向的路由
> - to：需要去往的路由

~~~react
import React, { Component } from 'react';
// 第一步:引入 Redirect 组件
import { Route, Redirect } from 'react-router-dom'
import Admin from './Admin'
class Root extends Component {
    render() {
        return (
            <div>
                <Switch>
                    我是根组件
                    <Route path='/admin' component={Admin} />
                    {/* 第二步:使用重定向路由 */}
                    <Redirect from='/'  to='/admin/adminone' exact></Redirect>
                </Switch>
            </div>
        );
    }
}

export default Root;
~~~

注意： 一定要将重定向路由规则组件写在其他路由规则之后,且重定向路由组件设置严格模式. 



### 5.2、404路由 

项目中少不了404页面的配置，在React里面配置404页面需要注意： 

- 需要用到Switch组件，让其去包裹所有路由的`Route`组件（Switch组件保证只渲染其中一个子路由）

  ~~~jsx
  import NotFound from "./Components/404";
  
  <Route>
      <NotFound></NotFound>
  </Route>
  // 或
   <Route component={NotFound}></Route>
  ~~~

> 注意：在404路由的位置，不需要给定具体的路由匹配规则，即不给`path`表示匹配`*`，即所有的路由都会匹配，因此用404路由一定要加`Switch`匹配一个路由。
>

例如：

~~~jsx
import React, { Component } from 'react';
// 第一步: 引入Switch组件
import { Route, Redirect, Switch } from 'react-router-dom'
import Admin from './Admin'
import Notfound from './NotFound';
class Root extends Component {
    render() {
        return (
            <div>
                我是根组件
              {/*使用switch组件包括所有路由,这样被包裹的路由每次只能匹配一个,同时支持手动修改地址栏*/}
                <Switch>
                    <Route path='/admin' component={Admin} />
                    {/* 使用重定向路由 */}
                    <Redirect from='/' exact to='/admin/adminone'></Redirect>
                    {/* 404路由 */}
                    <Route component={Notfound}></Route>
                </Switch>
            </div>
        );
    }
}

export default Root;
~~~



## 6、三种路由渲染方式(不是特别重要)

> v6中采用了新的属性对组件进行渲染，属性名：element

Route路由渲染组件是用于路由规则匹配成功后组件渲染容器，此组件提供了3种组件渲染方式：

- component属性（值为**变量对象或函数**）

  - ~~~jsx
    // 第一种方式 使用组件的变量名(最常用)
    <Route path="/home" component={Home}/>
    ~~~
  ~~~jsx
    import Home from '../views/routerview/Home';
    // 第二种方式: component的是为函数形式,返回对应的组件名
    // 缺点: 会造成Home组件的props属性丢失,无法使用props中的属性和方法,解决办法需要添加一个props形参, 该形参就是props属性对象,可以解构到组件上,这样就实现了props
    <Route path="/home" component={(props) => <Home {...props} />} />
  ~~~
  
- render属性（值只支持函数方式）

  - ~~~jsx
    // 如果不设置形参props和组件的{...props},会造成该组件Home 的props属性丢失,这样比如使用编程式导航跳转的时候,无法实现跳转 this.props.history.push()
    <Route path="/home" render={(props) => <Home {...props} />} />
    ~~~

- children属性（值可以是**函数或组件**）

  - ~~~jsx
    //props 属性会丢失,同上 
    //注意:children属性为函数时,该组件有个特点,无论该path是否与当前的渲染地址匹配,该渲染方式始终执行(即始终渲染该组件,前提没有Switch组件包裹),如果当前路由地址不匹配的话,其props的match属性为null
    // 后面可以使用该特性,自定义路由组件,
    <Route path="/about" children={(props) => {
    	if(props.match){
            return <About {...props} />
        }
    }} />
    ~~~
    
  - ~~~jsx
    //children属性为组件时, props属性会丢失 同上
    <Route path="/about" children={<About {...this.props} />} />
    ~~~

**注意点**

- 当children的值是一个函数时，无论当前地址和path路径匹不匹配，都将会执行children对应的函数，当children的值为一个组件时，当前地址和path不匹配时，路由组件不渲染
- children函数方式渲染，会在形参中接受到一个props对象，对象中match属性如果当前地址匹配成功返回对象，否则null



## 7、封装自定义导航组件

目前虽然在react中有导航组件Link，但是与vue相比，vue的router-link支持tag属性（自定义渲染成标签），而这里不支持。

1. 如下代码是使用<Link>组件实现的正常路由切换跳转

```jsx
import React, { Component } from 'react';
import { Link, Route } from 'react-router-dom'
class Routertest extends Component {
    render() {
        return (
            <div>
                <ul>
                    <li><Link to='/shouye' tag='p'>首页</Link></li>
                    <li><Link to='/fenlei'>分类页</Link></li>
                </ul>
                <Route path='/shouye' component={Shouye}></Route>
                <Route path='/fenlei' component={Fenlei}></Route>
            </div>
        );
    }
}
//1.定义首页组件
class Shouye extends Component {
    render() {
        return (
            <div>我是首页内容</div>
        )
    }
}
//2.定义分类组件
class Fenlei extends Component {
    render() {
        return (
            <div>我是分类页内容</div>
        )
    }
}
export default Routertest;
```

2. 使用自定义导航组件方式实现渲染;

```jsx
// 1. 父组件代码
import React, { Component } from 'react';
import { Link, Route } from 'react-router-dom';
// 引入Nav组件
import Nav from '../components/Nav'
class Routertest extends Component {
    render() {
        return (
            <div>
                <ul>
                    <li><Nav to='/shouye' tag='p'>首页</Nav></li>
                    <li><Nav to='/fenlei'>分类页</Nav></li>
                </ul>
                <Route path='/shouye' component={Shouye}></Route>
                <Route path='/fenlei' component={Fenlei}></Route>
            </div>
        );
    }
}
//1.定义首页组件
class Shouye extends Component {
    render() {
        return (
            <div>我是首页内容</div>
        )
    }
}
//2.定义分类组件
class Fenlei extends Component {
    render() {
        return (
            <div>我是分类页内容</div>
        )
    }
}
export default Routertest;
```

```jsx
// 定义 Nav组件代码
import React, { Component } from 'react';
// 需要导入
import { Route, withRouter } from 'react-router-dom'
class Nav extends Component {
    render() {
        const text = this.props.children // 获取要显示的文本内容
        const Tag = this.props.tag ? this.props.tag : 'a' // 获取要显示的标签
        const url = this.props.to // 获取要跳转的路径
        const style = { cursor: 'pointer' } // 设置样式
        return (
            <>
                <Route path={url} children={(props) => {
                    if (props.match) { // 判断当前页面路径与当前的path路径匹配,当前标签设置样式,就										是为了这个判断,要不然可以不用Route,直接使用Tag
                        style.color = 'red'
                    }
                    return <Tag onClick={() => this.go(url)} style={style}>{text}</Tag>
                }}></Route>

            </>
        );
    };
    go(url) {
        //console.log(this, props);
        // 注意:此处也可以使用children接收的props形参,在go函数的形参中接收该props,这样也可以使用
        // props.history.go(url) 实现
        this.props.history.push(url)
    }
}
// withRouter:高阶组件的强化函数,该函数可以让没有路由信息的组件获取路由信息和方法
export default withRouter(Nav);
```

知识点：6小节中的三种渲染模式



## 8、withRouter

**作用：把不是通过路由切换过来的组件中，将react-router 的 history、location、match 三个对象传入props对象上**

默认情况下，必须是经过路由匹配渲染的组件才存在this.props，才拥有路由参数，才能使用编程式导航的写法，才能执行this.props.history.push('/uri')跳转到对应路由的页面。然而不是所有组件都直接与路由相连的，当这些组件需要路由参数时，使用withRouter就可以给此组件传入路由参数，此时就可以使用this.props。

~~~jsx
// 引入withRouter
import { withRouter} from 'react-router-dom'

// 执行一下withRouter
export default withRouter(Cmp)
~~~

> 该高阶组件是路由包自带的东西，因此只需要引入+使用就可以了，不需要自己定义。



**V5与V6的变化**

- Switch被废弃，由Routes替代，并且Routes是必须的

- 渲染组件的方式被element属性替代，例如：

  - `<Route path="/dashboard" element={<Dashboard />} />`

- 重定向组件Redirect被废弃，使用以下语法替代：

  - ~~~js
    <Route path="/" element={<Navigate to="/dashboard/welcome" />} />
    ~~~

- 嵌套路由被简化

  - 父路由：

    - ~~~js
      <Route path="/dashboard/*" element={<Dashboard />} />
      ~~~

  - 子路由（子路由规则中不再需要写父前缀）：

    - ~~~js
      <Route path="welcome"  element={<Welcome/>}/>
      ~~~

      

# 十四、css-in-js技术

## 1、简介

CSS-in-JS是一种技术，而不是一个具体的库实现。简单来说CSS-in-JS就是将应用的CSS样式写在JavaScript文件里面，而不是独立为一些css，scss或less之类的文件，这样你就可以在CSS中使用一些属于JS的诸如模块声明，变量定义，函数调用和条件判断等语言特性来提供灵活的可扩展的样式定义。CSS-in-JS在React社区的热度是最高的，这是因为React本身不会管用户怎么去为组件定义样式的问题，而Vue有属于框架自己的一套定义样式的方案。

- 在js文件中写css就是css-in-js技术
- 好处：
  - 支持一些js的特性
    - 继承
    - 变量
    - 函数
  - 支持框架的特性
    - 传值特性

`styled-components` 应该是CSS-in-JS最热门的一个库，通过`styled-components`，你可以使用ES6的标签模板字符串语法，为需要styled的Component定义一系列CSS属性，当该组件的JS代码被解析执行的时候，styled-components会动态生成一个CSS选择器（比较随意的），并把对应的CSS样式通过style标签的形式插入到head标签里面。动态生成的CSS选择器会有一小段哈希值来保证**全局唯一性**来避免样式发生冲突。

- 通过ES6里面的模版字符串形式写css样式（**遵循之前css样式代码的写法**）
- 每个样式选择器都会在编译之后自动被添加上一个hash值（全局唯一）

使用`styled-components`前需要安装，安装的命令如下：

~~~shell
npm i -S styled-components
~~~

由于css后期会在模版字符串中编写，默认情况下vscode是没有css样式代码片段的（写样式的时候是没有代码提示的），为了提高css代码在模版字符串中编写的效率，此处强烈建议安装一个vscode的扩展：vscode-styled-components。

在React中写样式的方式一共有：

- **import "xxx.css"**
- **styled-components**
- **行内标签style属性**
- index.html中Link标签
- index.html的style标签



## 2、定义样式与使用

**定义:** 创建一个style.js文件.

~~~javascript
import styled from "styled-components";
// 按需导出(注意:style 后的div将会包裹对应的设置样式的内容)
export const Title = styled.div`
    font-size: 110px;
    color: pink;
    font-family: 华文行楷;
    background-color: black;
`;
~~~

**使用**

在使用的时候成员会被当作组件去使用（首字母大写）

~~~jsx
import React, { Component, Fragment } from "react";
//按需导入,就像使用常规 React 组件一样使用 Title
import { Title } from "./assets/style/style";

class App extends Component {
    render() {
        return (
            <Fragment>
                <Title>桃之夭夭，灼灼其华。</Title>
            </Fragment>
        );
    }
}

export default App;
~~~

**效果**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/0d46a4677c99b01d7caaecc08ec7349d21962da9.png?sign=d576889f41bac254c738e300e5de8c23&t=5fa04de3)



## 3、样式继承

在`styled-components`中也可以使用样式的继承，其继承思想与`react`的组件继承相似：

- 继承父的样式
- 重载父的样式

**样式继承**

~~~javascript
import styled from "styled-components";
const Button = styled.button`
    font-size: 20px;
    border: 1px solid red;
    border-radius: 3px;
`;
// 一个继承 Button 的新组件, 重载了一部分样式
// 继承会合并与父的样式，但是如果遇到样式冲突（相同），会以自己的为准
const Button2 = styled(Button)`
    color: blue;
    border-color: yellow;
`;

export { Button, Button2 };
~~~

**使用**

~~~jsx
import React, { Component, Fragment } from "react";
import { Button, Button2 } from "./assets/style/style";
class App extends Component {
    render() {
        return (
            <Fragment>
                <Button>我是第1个按钮</Button>
                <Button2>我是第2个按钮</Button2>
            </Fragment>
        );
    }
}

export default App;
~~~



## 4、属性传递

属性传递：样式值的动态传参（组件传值）

基于`css-in-js`的特性，在`styled-components`中也允许我们使用`props`（父传子），这样一来，我们可以对部分需要的样式进行传参，很方便的动态控制样式的改变。

**属性传递（JS中接收）**

~~~javascript
import styled from "styled-components";

// 参数传递 (如果没有传参就默认使用 50px值)
export const Title = styled.p`
    font-size:${(props) => { return props.fontSize || '50px'}}    
`;

// 应用
<Title fontSize={'10px'}>
    <span>测试样式传参</span>
</Title>
~~~



# 十五、网络请求

## 1.1、axios

react中通过npm来安装axios扩展

`npm i -S axios`

## 1.2、发起请求

以请求接口地址https://api.i-lynn.cn/ip为例，请求完毕后将当前我们自己的IP地址显示在视图中。

```jsx
import React, { Component } from "react";
// 引入axios
import axios from "axios";

class App extends Component {
    // 初始化状态
    state = {
        ipInfo: {},
    };

    render() {
        // 从数据中获取ip、country、area
        let { ip, country, area } = this.state.ipInfo;
        return (
            <div>
                当前的IP地址是：{ip} - {country} / {area}
            </div>
        );
    }

    // 类似于vue中的mounted
    async componentDidMount() {
        let ret = await axios.get("https://api.i-lynn.cn/ip");
        // console.log(ret.data);
        // 修改状态
        this.setState({
            ipInfo: ret.data
        });
    }
}

export default App;

```



## 1.3、react的反向代理

为了在前端解决跨域问题, 一些框架 如vue ,react  通过一些配置可以实现跨域 ()最常用的就是反向代理

**在`src目录`中，**新建`setupProxy.js`（必须是这个名字，[react](https://so.csdn.net/so/search?q=react&spm=1001.2101.3001.7020)脚手架会识别)，并通过npm安装http-proxy-middleware 代理中间件模块,保证 http-proxy-middleware  版本为2.0以上

`npm i -S http-proxy-middleware`

配置反向代理:

```js
// 第一步: 导出下载好的 http-proxy-middleware 中间件
const { createProxyMiddleware } = require('http-proxy-middleware')
// 设置跨域代理
// /api 表示当你的页面中的请求地址中包含 /api 这个字段的时候,
// target: 表示目标地址: 就使用本地服务器去请求目标服务器的域名地址
// changeOrigin 是否允许跨域
// pathRewrite : 重写地址:  /api 替换成''

module.exports = function (app) {
    app.use(
        createProxyMiddleware('/api', {//api是需要转发的请求(所有带有/api前缀的请求都会转发给5000)
            target: 'http://kumanxuan1.f3322.net:8001', //配置转发目标地址(能返回数据的服务器地址)
            changeOrigin: true, //控制服务器接收到的请求头中host字段的值
            /*
                changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:8000
                changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
                changeOrigin默认值为false，但我们一般将changeOrigin值设为true
            */
            pathRewrite: { '^/api': '' } //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
        })
    )
}

```



# 十六、Hooks（这才是重点）

## 1、简介

React中组件有函数组件与类组件，在 React Hooks 出现之前，我们可以使用函数和类组件来进行项目开发，但是如果组件中需要进行状态管理，函数组件就显得无能为力。React在**v16.8** 的版本中推出了 React Hooks 新特性，Hook是**一套工具函数的集合**,它增强了函数组件的功能，**hook不等于函数组件，所有的hook函数都是以use开头。**【以use开头的**特殊**的函数集合，就称之为Hooks】

Hook的定义：是react函数组件中使用的一些以“use”开头的特殊函数。

使用 React Hooks 相比于从前的类组件有以下几点好处：

- **代码可读性更强**，原本同一块功能的代码逻辑被拆分在了不同的生命周期函数中，容易使开发者不利于维护和迭代，通过 React Hooks 可以将功能代码聚合，方便阅读维护
- **组件树层级变浅，代码的跳跃性会变小**，在原本的代码中，我们经常使用 HOC/render/Props 等方式来复用组件的状态，增强功能等，无疑增加了组件树层数及渲染，而在 React Hooks 中，这些功能都可以通过强大的自定义的 Hooks 来实现
- **hook使用比使用类组件简单许多**（仁者见仁智者见智，但是Redux使用时 ，绝对是函数组件简单）
- hooks 更加使react面向函数开发, 也就是大家的说的函数式编程

## 2、hook的使用限制

- hook**只能**用在函数组件中或其他hook函数中使用, 类组件不能使用.
- **普通（非组件导出函数，且不以use开头的函数）**函数不能使用hook（hook不能在组件函数外去使用）
- **hook不能被有条件的调用**，因此不能放在if/for中（如果真有有条件调用的需求，请把条件写在hook函数内）

## 3、常用的hook函数

官网文档：https://reactjs.org/docs/hooks-reference.html

hook的使用步骤：

- 自定义hook（自己用自己写的）
- 导入hook成员
- 使用hook成员

### 3.1、useState

作用：保存组件的状态（用于解决函数组件无状态问题）

语法：

```js
import React, { useState } from 'react';
const [state, setstate] = useState(initialState);
```

案例：

```react
/**
 * hook：useState
 * 作用：实现函数组件中的所谓的“状态”
 * 提供方：react
 * 语法：const [访问变量名,设置数据方法名] = useState(初始默认值)
 * 参数含义：
 *      访问变量名：可以通过该变量名获取state的值
 *      设置数据方法名：只能通过调用该方法修改state的值，其只接受一个参数，参数要么是直接表示其值，要么以函数返回形式表示其值(必须有返回值)。如果前面的变量名叫a，那么此处的函数名一般叫做setA
 *      初始默认值：给state的初始值，支持字面量，也支持数组和对象等复杂数据类型
 * 注意点：
 *      a. 在修改字面量时，直接将最终的字面量的值给到修改函数即可
 *      b. 在修改对象或数组的时候，如果只是修改其中的一部分数据，记得最后返回的数据需要携带没有被修改的数据，如果不携带则丢失
 */

import React, { useState } from 'react';

const Index = () => {
    // 基础的字面量
    const [count, setCount] = useState(0);
    // 对象类型
    const [user, setUser] = useState({ uname: "张三", age: 22 })

    // 修改用户的信息的事件处理程序
    const modUser = () => {
         //1. 直接赋值 setUser({ ...user, age: 33 })
         //2. 函数返回的形式 
         setUser((m) => {
             // m为当前要修改的数据 user { uname: "张三", age: 22 }
            return {
                ...user, 
                age: user.age + 5
            }
        })
    }
    
    // 修改数组信息 注意: 再修改对象或数组时,如果只是修改其中一部分数据,记得要将剩余数据返回,否则元数据丢失
    const [usernameArr, setUsernameArr] = useState(['刘','关','张']);
    const editArr=()=>{
        usernameArr.push('赵云')
        console.log(usernameArr);
        // setUsernameArr(usernameArr)// 该种方式修改不了,useState会认为是同一个数组,不能修改
        // 第一种方式:
        setUsernameArr([...usernameArr])
        // 第二种方式:
        // setUsernameArr(()=>{
        //     return  [...usernameArr,'赵云']  
        // })
    }
    return (
        <div>
            <div>计数器的值是：{count}</div>
            {/*点击事件直接写函数表达式 一步到位*/}
            <div><button onClick={() => setCount(count + 1)}>+1</button></div>
            <hr />
            <div>
                <div>用户信息是：</div>
                <div>
                    用户名是：{user.uname}
                </div>
                <div>
                    年龄是：{user.age}
                </div>
                <button onClick={() => modUser()}>修改用户年龄</button>
            </div>
            <hr/>
            <p>{usernameArr.map((item,index)=>{
                return <span key={index}>{item}</span>
            })}</p>
            <p><button onClick={editArr}>修改数组</button></p>
        </div>
    );
}

export default Index;
```

### 3.2、useEffect

作用：模拟类组件中的生命周期的 ,执行的时间为dom更新后执行, 所以它是一个异步处理函数.

函数组件对于在一些生命周期中操作还是无能为力，所以 React提供了 useEffect 来帮助开发者处理函数组件，来帮助模拟完成一部分开发中**非常常用的生命周期方法（并不是全部的生命周期）**。常被称为：**副作用处理函数**。此函数的操作是异步的。

useEffect 相当类组件中的3个生命周期 （但含义上不完全等同）

- componentDidMount
- componentDidUpdate
- componetWillUnMount

语法：

```js
 useEffect(() => {
     effect... 副作用的操作,（类似于组件挂载完毕后、更新完毕后的操作）
     return () => {
         cleanup... 清理副作用
         （类似于组件的解除挂载的周期，可选）
     }
 },[INPUT,....])
```

案例：

```react
/**
 * hook：useEffect
 * 作用：用于操作作用及副作用代码的（用于控制函数组件生命周期的）
 * 语法：
 *      useEffect(callback[,array]) 数组array可选
 * 提供方：react
 * 细致语法：
 *      a. 在首次渲染及后续每次更新之后会执行callback的代码（模拟首次componentDidMount及componentDidUpdate）
 *          useEffect(没有返回值的callback)
 *      b. 被返回的函数是用于清理副作用的，会在执行下一次副作用前执行
 *          useEffect(带有返回值的cakkbacl)     //返回值必须是一个函数
 *      c. 有第二个参数，并且第二参数为空数组（仅仅会让副作用代码在首次挂载完毕后执行一次）
 *          useEffect(callback,[])
 *      d. 有第二个参数，且第二个参数中有具体的元素，元素个数不限，元素为useState的访问变量名。关注指定元素的更新，当被指定的元素更新后，将会再去执行副作用代码，及清理副作用的代码（如有）。
 *          useEffect(callback,[el1,el2,....])
 * 
 * 清理副作用很有意义：
 * 假设给页面绑定了点击事件，如果不清理事件，可能会导致事件越绑越多
 */
import React, { useEffect, useState } from 'react';

const Index = () => {
    const [count, setCount] = useState(0);
    const [position, setPosition] = useState({ x: 0, y: 0 })

    // 情形1: 没有返回值的callback
    // useEffect(() => {
    //     console.log(1)  //副作用代码 1.页面初始化执行,2.数据更新后执行 
    // })
    // 情形2: 带有返回值得callback函数
    // useEffect(() => {
    //     console.log(1)  //副作用代码  1.页面初始化执行;2.数据更新后执行
    //     return () => {
    //         console.log(2)  //清除副作用代码 1.下一次数据更新前执行
    //     }
    // })
    // 情形3: 第2个参数为空数组
    // useEffect(() => {
    //     console.log(1)  //副作用代码  1.页面初始化执行
    //     return () => {
    //         console.log(2)  //清除副作用代码 1.该函数不执行,return 没有意义了,相当于不写return
    //     }
    // }, [])
    // 情形4:第二个参数有数据
    // useEffect(() => {
    //     console.log(1)  //副作用代码  1.页面初始化执行,2.数据更新后执行
    //     return () => {
    //         console.log(2)  //清除副作用代码 1.下一次数据更新前执行
    //     }
    // }, [count]) // 此处第二个参数为被监听数据,可以写多个数据

    // 使用demo案例:
    const handler = (e) => {
        console.log('页面点击了');
        setPosition({ x: e.pageX, y: e.pageY })
    }
    useEffect(() => {
        // 给页面绑定点击事件
        document.addEventListener('click', handler)
        return () => {
            document.removeEventListener('click', handler) //清除副作用代码 1.下一次事件绑定前																清除,否则事件绑定对此叠加
        }
    })
    return (
        <div>
            <p>count:{count}</p>
            <p><button onClick={() => setCount(count + 1)}>+1</button></p>
            <hr></hr>
            <p>页面点击位置的坐标:x:{position.x},y:{position.y}</p>
        </div>
    );
}

export default Index;
```



实现类似类组件中this.setState 中的效果, 修改后立即获取最新的值

```jsx
// 只要count 发生改变.立即触发 useEffect,获取最新的值
useEffect(() => {
     console.log(count)	
 },[count])

 return (
        <div>
            <p>count:{count}</p>
            <p><button onClick={() => setCount(count + 1)}>+1</button></p>
        </div>
    );
```

或者使用

```react
 import { flushSync } from 'react-dom'
 // react-dom 提供的flushSync函数, 该函数需传入一个回调, 并且会同步刷新回调中的状态更新

 return (
        <div>
            <p>count:{count}</p>
            <p><button onClick={() =>{
               flushSync(() => {
 					setCount(count + 1)
                   console.log(count)	// count 为最新的值
				})         
             }}>+1</button></p>
        </div>
    );
```



### 3.3、useRef  

作用：用来生成对 DOM 对象的引用（类似于类组件中的createRef方法）

获取元素节点或者 子组件上的数据和方法

效果: 与类组件createRef相比实现相同的功能,函数组件使用useRef 相对复杂麻烦.

案例：

- 父组件Index.jsx

```react
/**
 * useRef的注意点：
 * 1. 使用肯定得导入useRef
 * 2. 通过执行该useRef函数获取ref对象
 * 3. 将ref对象挂载到需要获取对象的标签上面，例如：ref={ref}
 * 4. 后续我们可以通过ref对象获取对应的标签的对象
 * 5. 特别需要注意：默认情况下，普通的html标签是可以直接使用ref对象的（获取到的是dom对象），而函数组件标签默认不能使用ref对象！
 * 6. 如果需要解决函数组件标签使用ref就报错的问题，需要借助React.forwardRef()【让子具备转发ref的能力，子去给父ref】，该方法不是hook函数，而是hoc强化函数；还需要借助一个hook函数：useImperativeHandle来告诉父可以从子这里获取哪些数据或者方法（向外暴露成员）。
 * 7. useImperativeHandle函数的语法：useImperativeHandle(ref对象,callback)，callback必须返回一个对象，对象即为向外暴露的成员。其中ref对象为函数组件函数的第二形参
 * 
 */

import React, { useRef } from "react"
import Child from "./Child"

const Index = () => {
    // 通过useRef获取ref对象
    const ref1 = useRef()
    const ref2 = useRef()

    // 事件的处理程序
    const showRef = () => {
        console.log(ref1)
        console.log(ref2)
    }
    return (
        <div>
            {/* 默认情况下，普通的html标签是可以直接使用ref对象的 */}
            <div ref={ref1}>这是父组件</div>
            {/*如果给函数组件直接使用ref属性会报错,解决方法给该子组件使用forwardRef强化函数*/}
            <Child ref={ref2} />

            <div>
                <button onClick={showRef}>打印</button>
            </div>
        </div>
    )
}

export default Index
```

- 子组件Child.jsx

```jsx
//接下来使用 useImperativeHandle这个 hooks函数,将子组件的数据和方法暴露给父组件
// 语法: useImperativeHandle(ref,()=>{return{要暴露给父组件的数据和方法}});第二个函数必须有返回值
import React, { useState, forwardRef, useImperativeHandle } from "react"

const Child = (props, ref) => {
    const [state, setState] = useState(66666)

    const showMsg = () => {
        console.log(state)
    }
    // 向外暴露成员方法、变量,注意:第一个参数ref为该函数组件的第二个形参 
    useImperativeHandle(ref, () => ({ showMsg }))

    return (
        <div>
            <div>这是子组件</div>
        </div>
    )
}

// forwardRef:为强化函数,使该函数组件具备转发ref的能力,解决该函数组件添加ref属性报错的问题.
export default forwardRef(Child)
```

面试题：

- useRef能否用createRef替换？【可以】
- useRef与之前学习的createRef有何区别？【useRef在多次渲染页面的时候始终是同一个ref对象的引用；而createRef在每次更新后都会产生新的ref引用】

### 3.4、useContext 

- 作用: 实现跨级组件之间的传参


作用：createContext实现数据的共享，只是消费数据的方式不一样

案例：

- context对象产生的文件createContext.js

```js
import { createContext } from "react"
// createContext函数是可以传递参数的。
// 参数的意义可以理解成是待消费的数据的默认值
// 默认值一般在项目做单元(组件)测试（unit test）的时候可能会被用上
export default createContext({ a: 11, b: 22, c: 33 })
```

- 售卖方组件

```jsx
/**
 * 1. 售卖方提供需要借助context对象的Provider组件
 * 2. 通过售卖方Provider组件的value属性将需要共享的数据传递给子组件
 * 3. 消费方通过useContext函数进行消费，语法：const data = useContext(context对象)
 */
import React from "react";
import Child from "./Child";
import Obj from "./createContext";

// 获取售卖方的身份
const { Provider } = Obj;
const ProviderCmp = () => {
    const data = { a: 1111, b: 2222, c: 3333 }
    return (
        <div>
            <Provider value={data}>
                <Child />
            </Provider>
        </div>
    )
}

export default ProviderCmp
```

- 消费方的组件

```jsx
import React, { useContext } from "react"
import Obj from "./createContext";
const Child = () => {
    // 通过useContext消费数据
    const data = useContext(Obj)
    return (
        <div>
            <div>a的值是：{data.a}</div>
            <div>b的值是：{data.b}</div>
            <div>c的值是：{data.c}</div>
        </div>
    )
}

export default Child
```



### 3.5、redux相关hook

- 与类组件相比，函数组件的redux使用非常简单（只是简化了组件中的使用包括获取数据与action的派发）

```js
import { useDispatch, useSelector } from "react-redux";
```

注意点：官方自带的`useReducer`hook也可以实现针对reudx的操作，但是企业一般不直接用它。而是使用react-redux中提供的封装过的hook（**useSelector,useDispatch**）。如果企业项目有自己封装redux相关的hook的时候才会使用`useReducer`。其实react-redux提供的自定义的hook底层也是基于`useReducer`实现的。

作用：

- useSelector：帮助我们获取仓库中的数据，参数是callback，函数有一形参state（默认数据源）
- useDispatch：帮助我们派发用于修改的action

案例：通过**useSelector,useDispatch**实现对于redux中数据的读写（计数器案例）

```react
/**
 * 1. 关于redux的hooks分为两派：官方的useReducer、react-redux提供的useSelector/useDispatch。【建议用后者】
 * 2. useSelector作用：获取仓库中的数据，语法：const state = useSelector(callback)，函数接收形参，形参为仓库中的全部的数据，函数返回的为处理好的数据
 * 3. useDispatch作用：返回一个dispatch方法
 */

import React from "react"
import { useSelector, useDispatch } from "react-redux";
const Index = () => {
    // 获取数据
    const state = useSelector(state => state)
    // 产生dispatch方法
    const dispatch = useDispatch()
    // 事件处理程序
    const add = () => {
        // 派发action
        dispatch({ type: "add", payload: 1 })
    }
    return (
        <div>
            <div>计数器的值：{state.count}</div>
            <button onClick={add}>+1</button>
        </div>
    )
}

export default Index
```

### 3.6、react-router-dom相关

版本：v5.3.4

```shell
npm install react-router-dom@5
```

使用前:首先在入口文件中指定当前的路由模式,否则报错

```js
import { BrowserRouter as Router } from 'react-router-dom';
root.render(
  // 全局使用state 数据
  <Router>
    <Provider store={store}>
        <App />
    </Provider>
  </Router>
);
```

```js
import { useHistory, useParams, useLocation } from "react-router-dom";
```

作用：快速获取路由信息的

- useHistory：获取history路由信息对象
- useParams：获取路由中动态路由参数对象
- useLocation：获取路由中的location对象信息

案例：

```react
/**
 * hook：路由包提供
 *      useLocation/useHistory/useParams
 * 作用：操作路由信息(也就不用使用withRouter)
 */

import React from 'react';
import { useHistory, useLocation, useParams } from "react-router-dom"

const Index = () => {
    // 等同于类中： const his = this.props.history
    const his = useHistory();
    // 等同于类中： const loc = this.props.location
    const loc = useLocation();
    // 等同于类中： const par = this.props.macth.params
    const par = useParams(); // 获取restful风格参数
    console.log(his, loc, par);
    const goabout= ()=>{
        his.push("/about")
    }
    return (
        <div>
            <button onClick={() => his.push("/news")}>去往“/news”</button>
            <button onClick={goabout}>去往“/about”</button>
        </div>
    );
}

// 此处不需要withRouter
export default Index;
```

注意点：用了这个三个hook，组件在导出的时候就不用再withRouter。



###  3.7、useMemo (面试高频问题)

一般作为性能优化的时候使用

 `useMemo` 计算结果是 `return` 回来的值, 主要用于 缓存计算结果的值 ，应用场景如： 需要计算的状态 

相当于vue中的计算属性, 只有依赖的值发生变化,计算属性才会重新计算

```jsx
//语法结构:  
//注意: 参数1为函数,该函数必须有一个返回值,返回值就是newvalue的值
//注意: 参数2为数组,表示该useMome依赖的数据,当依赖的数据发生改变,计算属性就会重新计算,也就是重新执行参数1的箭头函数, 当依赖数组为空数组,则每次都会执行
const newvalue =  useMemo(()=>{return 该值就是计算属性的值},[依赖的值1,依赖的值2])
```

示例代码: 实现一个根据关键词进行筛选的功能

```jsx
import React, { useEffect, useState, useMemo } from 'react';
import axios from 'axios';

const Test = () => {
    const [keyword, setKeywordFn] = useState('')
    const [collegeArr, setCollegeFn] = useState([])
    const [count, setCount] = useState(0)

    useEffect(() => {
        //console.log('数据请求');
        axios.get('https://api.i-lynn.cn/college').then(res => {
            console.log('res', res);
            setCollegeFn(res.data.data.list1)
        })
    }, [])
	
    // 方式1: 当点击count++ 时, 因为函数组件的数据变化了,函数组件重新执行,则重新执行函数组件中的所有的     代码,那么getCollegelist 该值就会重新赋值.
    // const getCollegelist = collegeArr.filter((item) => {
    //     console.log('代码执行1');
    //     return item.school_name.includes(keyword)
    // })
    // console.log(getCollegelist);
	
    // 方式2: 当点击count++,getCollegelist的值不会重新赋值,因为只有依赖的keyword或collegeArr发生        变化, 该箭头函数才会重新执行,并赋值
    const getCollegelist = useMemo(() => collegeArr.filter((item) => {
        console.log('代码执行2');
        return item.school_name.includes(keyword)
    }), [keyword, collegeArr])
    // console.log(getCollegelist);

    return (
        <div>
            <p onClick={() => setCount(count + 1)}>count:{count}</p>
            <hr />
            <input value={keyword} onChange={(e) => {
                setKeywordFn(e.target.value)
            }} />
            <ul>
                {
                    getCollegelist.map((item) => {
                        return <li key={item.id}>{item.school_name}</li>
                    })
                }
            </ul>
        </div>
    );
}

export default Test;
```



### 3.8、高阶组件 memo

其实以上代码也可以不使用useMemo 钩子函数, 使用memo 纯组件写法,也可以实现对应的效果 ,也就是说只有该该组件的props 和自身state 发生变化,组件的render 会执行, 也就是才会重新渲染.

将渲染列表组件单独抽离出来: 如下

```jsx
import React, { useEffect, useState, memo } from 'react';
import axios from 'axios';


const Child = () => {
    const [keyword, setKeywordFn] = useState('')
    const [collegeArr, setCollegeFn] = useState([])
    useEffect(() => {
        //console.log('数据请求');
        axios.get('https://api.i-lynn.cn/college').then(res => {
            console.log('res', res);
            setCollegeFn(res.data.data.list1)
        })
    }, [])

    const getCollegelist = collegeArr.filter((item) => {
        console.log('代码执行1');
        return item.school_name.includes(keyword)
    })
    console.log('纯组件');
    return (
        <div>
            <input value={keyword} onChange={(e) => {
                setKeywordFn(e.target.value)
            }} />
            <ul>
                {
                    getCollegelist.map((item) => {
                        return <li key={item.id}>{item.school_name}</li>
                    })
                }
            </ul>
        </div>
    );
}

export default memo(Child);

```

将渲染列表组件引入到父组件中

```jsx
import React, { useEffect, useState, useMemo } from 'react';
import Child from './Child';

const Test = () => {
    const [count, setCount] = useState(0)
    return (
        <div>
            {/*当点击count+1 的时候, 也不会触发渲染列表组件重新渲染*/}    
            <p onClick={() => setCount(count + 1)}>count:{count}</p>
            <hr />
      		{/*引入上面定义的渲染列表组件*/}      
            <Child></Child>
        </div>
    );
}

export default Test;

```



纯组件memo 在使用时需要注意的问题:

在父组件中:

```jsx
import React, { useEffect, useState, useMemo } from 'react';
import Child from './Child';

const Test = () => {
    const [count, setCount] = useState(0);
    {/*在父组件中,给Child组件通过props传递参数,如数组girlArr, 当数组改变的时候,纯组件 Child 
     虽然使用memo强化函数变成了纯组件,但是Child组件中的getCollegelist 还是会重复执行,所以还是需要使用  	useMemo这个计算属性处理 getCollegelist 
    */}
    const [girlArr, setGirlArrFn] = useState(['范冰冰', '李冰冰'])

    return (
        <div>
            <p onClick={() => setCount(count + 1)}>count:{count}</p>
            <hr />
            <button onClick={() => {
                setGirlArrFn([...girlArr, '张冰冰'])
            }}>加人</button>
            <Child girlArr={girlArr}></Child>
        </div>
    );
}

export default Test;

```

在child 组件中

```jsx
import React, { useEffect, useState, memo, useMemo } from 'react';
import axios from 'axios';

const Child = (props) => {
    const [keyword, setKeywordFn] = useState('')
    const [collegeArr, setCollegeFn] = useState([])
    useEffect(() => {
        //console.log('数据请求');
        axios.get('https://api.i-lynn.cn/college').then(res => {
            console.log('res', res);
            setCollegeFn(res.data.data.list1)
        })
    }, [])
    
	{/*当接收的数组girlArr改变的时候,纯组件 Child 虽然使用memo强化函数变成了纯组件,但是Child组件中的     getCollegelist 还是会重复执行,所以还是需要使用 useMemo这个计算属性处理 getCollegelist,所以建议  	开发者使用useMemo,不建议使用 memo纯组件 */}
    const getCollegelist = useMemo(() => collegeArr.filter((item) => {
        console.log('代码执行1');
        return item.school_name.includes(keyword)
    }), [keyword, collegeArr])

    return (
        <div>

            <input value={keyword} onChange={(e) => {
                setKeywordFn(e.target.value)
            }} />
            <ul>
                {
                    getCollegelist.map((item) => {
                        return <li key={item.id}>{item.school_name}</li>
                    })
                }
            </ul>
            {/*遍历接收的参数girlArr*/}
            <ul>
                {props.girlArr.map(((item, index) => <li key={index}>{item}</li>))}
            </ul>
        </div>
    );
}

export default memo(Child);

```

 

###  3.9、useCallback (高频问题)

 `useCallback` 计算结果是 `函数`, 主要用于 缓存函数，应用场景如: 需要缓存的函数，因为函数式组件每次任何一个 state 的变化 整个组件 都会被重新刷新，一些函数是没有必要被重新创建的，此时就应该缓存起来，提高性能，和减少资源浪费。 

语法: 

```jsx
import React, { useState, useCallback } from 'react';

const Test2 = () => {
    const [inpvalue, setinpvalue] = useState('');
    const [userList, setUserList] = useState(['张三', '李四']);
    const [useinfo, setUserinfo] = useState({ name: "小明", age: 10 });
    
    // 情况1: 当不使用useCallback情况下, 当修改useinfo对象中的年龄的时候, 函数组件重新执行, 这样导致
    //  addUser 函数重新被创建赋值, 消耗浏览器性能
    // const addUser = () => {
    //     console.log('执行了', inpvalue);
    //     userList.push(inpvalue);
    //     // console.log(userList);
    //     setUserList([...userList])
    // }
	
    // 情况2: 当使用 useCallback 的时候, 会将当前useCallback中的参数如下的箭头函数缓存起来, 这样, 
    // 不会发生当函数组件中的数据被修改时, 函数组件代码重新执行时, 里面的addUser 函数会被重新创建, 
    // 默认如果依赖值为空,也就是 / 中的第二个参数为空数组,则会导致永远都赋值的为初始函数,函数没有被重新创建,导致添加不了用户, 可以将数组的依赖项值赋值为输入框中的变量,这样当输入框中的值改变时,才重新创建函数赋值,优化性能
    const addUser = useCallback(() => {
        userList.push(inpvalue)
        // console.log(userList);
        setUserList([...userList])
    }, [inpvalue])
    
    console.log('执行了');
    return (
        <div>
            我是test2组件
            <p onClick={() => {
                setUserinfo({
                    ...useinfo,
                    age: useinfo.age + 1
                })
            }}>{useinfo.name}--{useinfo.age}</p>
            <hr />
            <p>
                <input value={inpvalue} onChange={
                    (e) => setinpvalue(e.target.value)
                } />
                <button onClick={addUser}>添加</button>
            </p>
            <hr />
            <ul>
                {
                    userList.map((item, index) => <li key={index}>{item}</li>)
                }
            </ul>

        </div>
    );
}

export default Test2;
```



**面试题:** useMemo 和useCallback 的区别?

都是做性能优化的

 useMemo和useCallback都会在组件第一次渲染的时候执行，之后会在其依赖的变量发生改变时再次执行；并且这两个hooks都可以实现数据缓存，useMemo返回缓存的变量(重新计算的值)，useCallback缓存的是函数。 



### 3.10、useReducer

模拟实现react-redux , 实现在组件外部管理组件的状态,这样感觉当前组件就是无状态了. 因为组件的数据不在组件内定义, 而是在组件外定义了. 

简要概括:  实现部分组件共享状态(可以理解为局部redux),不是全局组件共享状态(redux)

**使用方式1**: 在一个组件外部管理组件的状态(先只处理一个组件)

![1670420390210](C:\Users\wusheng\AppData\Roaming\Typora\typora-user-images\1670420390210.png)

案例代码: 实现一个左右按钮+- 的一个功能

```jsx
import React from 'react';
import { useReducer } from 'react';
import Child1 from './Child1';
import Child2 from './Child2';
import contextObj from './context';
// useReducer hooks的使用步骤:
// 定义: 相当于一个小的redux 仓库, 几个组件可以共享使用和修改该仓库的数据

// useReducer 一般结合useContext 使用,然后这样就实现了几个组件之间的共享数据,
// 共享父组件中的数据  useReducer + useContext


// 第一步: 在函数组件外定义初始的状态
const defaultState = {
    count: 0,
    userinfo: {
        name: '范闲',
        age: 20
    }
}

// 第二步: 定义reducer 函数
const reducer = (state, actions) => {
    if (actions.type == '+') {
        return {
            ...state, //这里和redux一样, 需要保留原数据, 否则数据会丢失
            count: state.count + actions.payload
        }
    }
    if (actions.type == '-') {
        return {
            ...state,
            count: state.count - actions.payload
        }
    }
    if (actions.type == 'addage') {
        return {
            ...state,
            userinfo: {
                ...state.userinfo,
                age: state.userinfo.age + actions.payload
            }
        }
    }

    return state
}


const { Provider } = contextObj;
const Father = () => {
    //第三步 使用useReducer,创建store仓库数据和dispatch 方法
    // state: 就是仓库中的数据
    // dispatch: 就是修改state 数据的方法,用该dispatch派发任务修改state
    const [state, dispatch] = useReducer(reducer, defaultState);

    return (
        <div>
            {/* 使用useReducer仓库中的数据 */}
             <p>
                <button onClick={() => {
                    dispatch({ type: '+', payload: 1 })
                }}>+</button>

                count:{state.count}

                <button onClick={() => {
                    dispatch({ type: '-', payload: 1 })
                }}>-</button>
            </p>
        </div>
    );
}

export default Father;

```



**使用方式2:** 最常用的方式,结合useContext 实现多个组件共享一个组件状态. 有点类似于react-redux,其实对于项目中如果处理的共享状态比较少的话,则可以使用userReducer+useContext方式, 如果项目中需要管理的共享数据比较多的话,建议使用 react-redux



案例: 实现几个组件之间的数据共享及操作

```jsx
import React from 'react';
import { useReducer } from 'react'
import Child1 from './Child1';
import Child2 from './Child2';
import contextObj from './context';
// useReducer hooks的使用步骤:
// 定义: 相当于一个小的redux 仓库, 几个组件可以共享使用和修改该仓库的数据

// useReducer 一般结合useContext 使用,然后这样就实现了几个组件之间的共享数据,
// 共享父组件中的数据  useReducer + useContext

// 第一步: 在函数组件外定义初始的状态
const defaultState = {
    count: 0,
    userinfo: {
        name: '范闲',
        age: 20
    }
}

// 第二步: 定义reducer 函数
const reducer = (state, actions) => {
    if (actions.type == '+') {
        return {
            ...state,
            count: state.count + actions.payload
        }
    }
    if (actions.type == '-') {
        return {
            ...state,
            count: state.count - actions.payload
        }
    }
    if (actions.type == 'addage') {
        return {
            ...state,
            userinfo: {
                ...state.userinfo,
                age: state.userinfo.age + actions.payload
            }
        }
    }
    return state
}

const { Provider } = contextObj;
const Father = () => {
    //第三步 使用useReducer,创建store仓库数据和dispatch 方法
    // state: 就是仓库中的数据
    // dispatch: 就是修改state 数据的方法,用该dispatch派发任务修改state
    const [state, dispatch] = useReducer(reducer, defaultState);
    return (
        <div>
            {/* 使用useReducer仓库中的数据 */}
            {/* 使用Child1组件 */}
            <Provider value={{
                state,
                dispatch
            }}>
                <Child1></Child1>
                <Child2></Child2>
            </Provider>


        </div>
    );
}

export default Father;

```

````jsx
// child1组价
import React from 'react';
import contextObj from './context'
import { useContext } from 'react';

const Child1 = () => {
    const { state, dispatch } = useContext(contextObj)
    //console.log('data', data); //data就是 {state, dispatch}
    console.log('state', state);
    return (
        <div>
            <p onClick={() => {
                dispatch({ type: '+', payload: 10 })
            }}>child1-{state.count}</p>
        </div>
    );
}

export default Child1;

````

````jsx
// child2组件
import React from 'react';
import contextObj from './context';
import { useContext } from 'react'
const Child2 = () => {
    const { state, dispatch } = useContext(contextObj)
    return (
        <div>
            <p onClick={() => {
                dispatch({ type: 'addage', payload: 5 })
            }}>child2 --{state.userinfo.name}--{state.userinfo.age}</p>
        </div>
    );
}
export default Child2;


````



### 3.11 useLayoutEffect (面试可能问,实际使用很少)

useLayoutEffect 与 useEffect的区别：

- `useEffect` 是异步执行的，而`useLayoutEffect`是同步执行的。
- `useEffect` 的执行时机是浏览器完成渲染之后，而 `useLayoutEffect` ,但它会在所有的 DOM 变更之后同步调用 effect 

```jsx
import React, { useState, useLayoutEffect, useEffect } from 'react'

export default function UseLayoutEffectDemo() {
  const [ state, setState ] = useState('hello')
  // 当使用`useEffect`时, 他是异步执行, 他是页面先会直接渲染hello, 此时useEffect异步同时也会执行,但      是比页面首次渲染晚, 然后执行useeffect,页面数据更新为 world
  
  // 当使用 useLayoutEffect 他是同步执行, 也就是赋值完毕为world. 然后再执行渲染, 所有最终的结果为        world
  useEffect(() => {
    let i = 0
    while (i < 1000000000) {
      i++
    }
    setState('world')
  }, [])

  return (
    <>
      <h1>{state}</h1>
    </>
  )
}
```



### 3.12 useId

```js
const id = useId();
```

`useId`是一个钩子，用于生成唯一的ID，在服务器和客户端之间是稳定的，同时避免两端之间的不匹配。

>注意：
>
>`useId`不是用来生成列表中的键的。`Keys` 应该从你的数据中生成。

对于一个基本的例子，直接将id传递给需要它的元素。

```jsx
function Checkbox() {
  const id = useId();
  return (
    <>
      <label htmlFor={id}>Do you like React?</label>
      <input id={id} type="checkbox" name="react"/>
    </>
  );
};
```

对于同一组件中的多个ID，使用相同的ID附加一个后缀。

```jsx
function NameFields() {
  const id = useId();
  return (
    <div>
      <label htmlFor={id + '-firstName'}>First Name</label>
      <div>
        <input id={id + '-firstName'} type="text" />
      </div>
      <label htmlFor={id + '-lastName'}>Last Name</label>
      <div>
        <input id={id + '-lastName'} type="text" />
      </div>
    </div>
  );
}
```

>**注意：**
>
>`useId` 会生成一个包含 : token的字符串。这有助于确保令牌是唯一的，但在CSS选择器或API（如`querySelectorAll`）中不支持。
>
>`useId`支持一个`identifierPrefix`，以防止在多根应用程序中发生碰撞。要配置，请参阅 `hydrateRoot` 和 `ReactDOMServer `的选项。

### 3.14 useDeferredValue(面试可能问)

```
const deferredValue = useDeferredValue(value);
```

`useDeferredValue` 需要接收一个值, 返回这个值的副本, 副本的更新会在值更新渲染之后进行更新(**延迟更新**), 以此来避免一些不必要的重复渲染. 打个比方页面中有输入框, 输入框下的内容依赖于输入框的值, 但是输入框是一个高频操作, 如果输入10次, 可能用户只想看到最终的结果那么中途的实时渲染就显得不那么重要了, 页面元素少点还好, 一旦元素过多页面就会及其的卡顿, 渲染引擎堵得死死的, 用户就会骂娘了, 此时使用useDeferredValue是一个很好的选择。

注意: 如果是监听的deferredValue 副本数据, 当**高频率操作**更新 inpVal时, 会在最后一下全部渲染, 

​         如果是监听的inpVal, 而不是副本数据, 当**高频率操作**更新 inpVal时,会整个过程中出现卡顿.

**注意: 必须是高频操作才能看出效果, 否则看不出效果,** 

 如上例子好比: 就是一个同学打水 去一次打一点水.来回跑了10次. 和去水龙头那边 来回按下10次阀门发水,

 虽然最后的执行结果都是10次,但是省去了学生来回路程上的时间;

```jsx
import React from 'react';
import { useState, useDeferredValue, useEffect } from 'react';

const Father = () => {
    const [inpvalue, setinpvalue] = useState('')
    const defervalue = useDeferredValue(inpvalue)
    // console.log('defervalue', defervalue);
    const [data, setData] = useState([])
    useEffect(() => {
        // 当监听的是defervalue,则最后只在最后输出, 且页面相对不太卡,
        // 当监听的是 inpvalue,则 最后每次变化都输出, 且页面相对卡顿
        console.log('变化了'); 
        const list = [];
        for (let i = 0; i < 50000; i++) {
            list.push(i + '')
        }
        const filterArr = list.filter(item => item.includes(defervalue))
        // console.log('filterArr', filterArr);
        setData(filterArr)
    }, [inpvalue]);
    return (
        <div>
            <input type='text' onChange={(e) => {
                setinpvalue(e.target.value)
            }} />

            {/* 渲染数据 */}
            <ul>
                {
                    data.map(((item, index) => {
                        return <li key={index}>{item}</li>
                    }))
                }
            </ul>
        </div>
    );
}

export default Father;

```



### 3.15、自定义hook

案例：自定义在线状态hook，要求使用了这个hook可以自动判断当前网络连接的情况

应用场景：在线聊天类型的项目，可以用这个hook动态判断当前用户的网络连接状态

![img](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/02/f0ddcffdab69175b76024005a708968bef6da0f0.png?sign=153f9147240e75331423bdd8dc046f54&t=6017cfbd)

实现代码：

```react
/**
 * 自定义hook = 自定义函数
 * 1. 名称必须以use开头
 * 2. 细节：
 *      a. 得有返回值，返回当前用户的网络状态
 *      b. 是否在线的状态通过navigator.onLine进行判断
 *      c. 由于需要持续获取用户的状态，得用到事件监听，监听事件是什么？
 *              - online  当电脑有网触发
 *              - offline 当电脑没网触发
 *      d. 刚进入到这个函数的时候需要初始化一个变量去存储当前的网络状态
 *
 */

import { useState, useEffect } from "react";

function useOnline() {
    // 立马获取当前的状态
    const [online, setOnline] = useState(navigator.onLine)
    // .....
    const on = () => setOnline(true)
    const off = () => setOnline(false)
    useEffect(() => {
        // 执行副作用
        window.addEventListener("online", on)
        window.addEventListener("offline", off)
        return () => {
            // 清理副作用;防止频繁切换网络导致事件重复绑定
            window.removeEventListener("online", on)
            window.removeEventListener("offline", off)
        }
    })
    // 返回网络状态 
    return online
}

export default useOnline

// 在组件中使用
import useOnline from '路径';
<p>{useOnline() ? '有网' : '没网'}</p>

```

## 4、immutable 不可变数据结构

facebook 研发的一个数据结构的方法, 解决数据深拷贝带来的性能问题

 Immutable Data 就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是 **Persistent Data Structure**（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 **Structural Sharing**（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享 

作用: 就是为了提高操作性能使用

<img src="E:/react/课件/img/1.png" style="zoom: 25%;" />

<img src="E:/react/课件/img/2.png" style="zoom: 25%;" />

<img src="E:/react/课件/img/3.png" style="zoom: 25%;" />

```jsx
// 第一种情况:
let obj = {
  a: 1,
  b: {
    c: 2
  }
}


// let newObj = obj;
// console.log(newObj === obj); // true
// obj.a = 10
// console.log(newObj, obj); // 数据都发生了变化
```

```jsx
// 第二种情况
// 浅拷贝2
// let newObj = Object.assign({}, obj)
// console.log('newObj', newObj);
// console.log(newObj == obj);  // false

// newObj.a = 100;
// newObj.b.c = 99  // 说明只拷贝了一层
// console.log(obj, newObj); // 二者不一致
```

```jsx
// cloneDeep 深拷贝
import { cloneDeep } from 'lodash';
// let newObj = cloneDeep(obj)
// newObj.a = 100;
// newObj.b.c = 99  // 
// console.log(obj, newObj);  // 实现了深拷贝
// 缺点: 浪费性能

```

```jsx
// 使用immutable.js
import {Map,formJS} from 'immutable'
let imObj = Map(obj);
console.log(obj, imObj);
// 修改完数据返回一个新的对象, 没有变化的数据还是使用原内存空间的数据, 
// 只对变化的数据进行一份拷贝.所以并不是完全拷贝.只是拷贝了一部分
let newObj = imObj.set('a', 10);
console.log(obj.a); // 1
console.log(imObj.get('a'));  // 1
console.log(newObj.get('a')); // 10
```



## 5、redux 中使用immutable

可以解决的问题: 

问题1: redux 中数据每在reducer中修改,都要返回一个对象, 对象其实就是原对象的深层递归克隆, 当state 数据嵌套层级较深, 则使用immutale 可以简化 保留原数据使用的层级嵌套

问题2: 只递归变化修改的state中的数据, 而不是递归整个state, 提高性能 .



不可变数据常用的api

- formJS( ) 将js 对象转换成immutable对象(也就是Map 对象) 需要导入使用

- toJS()  将immutable 对象转成  js 对象,  该api 是immutable对象原型上的方法

- update(name,callback)  修改immutable 对象上的数据 (注意是浅层数据)

- updateIn([name1,name2,...],callback)  修改immutable 对象中的数据(注意是深层数据)

  

**redux 中使用immutable 数据结构, 核心思想:**
01: 存储时使用immutable 对象数据结构,  

02: 修改的时候使用immutable 对象的api 修改,   

03: 只有在使用的时候使用原生js 对象



定义redux 仓库

```jsx
import { createStore } from 'redux';
// 此时不使用redux 提供的combineRuducer,因为 combineRuducer只能处理js原生数据结构
// 将原来的combineReducers 改成由redux-immutable提供的combineReducers
// 需要安装 npm i redux-immutable;
import { combineReducers } from 'redux-immutable'

import { fromJS } from 'immutable';

const userState = fromJS({
    count: 100, // 第一层数据
    userinfo: { // 深层数据
        name: '张三',
        age: 20
    }
})

console.log(userState);

function userReducer(state = userState, action) {
    if (action.type == '+') {
        // return {
        //     ...state,
        //     userinfo: {
        //         ...state.userinfo,
        //         age: state.userinfo.age + action.payload
        //     }
        // }
        return state.updateIn(['userinfo', 'age'], val => val + action.payload);
    }
    return state
}

const shopState = fromJS({
    num: 10,
    goodsArr: ['手机', '笔记本', '电脑']
})
function shopReducer(state = shopState, action) {
    if (action.type == 'add') {
        //state.goodsArr.push(action.payload)
        // return {
        //     ...state,
        //     goodsArr: [...state.goodsArr]
        // }
        return state.update('goodsArr', (val) => {
            return val.push(action.payload)
        })
    }

    return state
}

const store = createStore(combineReducers({
    userReducer,
    shopReducer
}), window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)

export default store;
```

在页面中使用

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
export default function Father() {
	// 使用toJS() 将immutable 数据转成js 对象,然后就可以访问该对象的属性了
    const user = useSelector(state => state.toJS().userReducer);
    // console.log(user);
    const shop = useSelector(state => state.toJS().shopReducer)
    console.log(shop);
    const dispatch = useDispatch();

    return (
        <div>
            {/* 用户模块 */}
            <p>count:{user.count}</p>
            <p onClick={() => {
                dispatch({
                    type: '+',
                    payload: 1
                })
            }}>{user.userinfo.name}--{user.userinfo.age}</p>
            {/* 商品模块 */}
            <ul>
                {
                    shop.goodsArr.map((item, index) => <li key={index}>{item}</li>)
                }
            </ul>
            <p><button onClick={() => {
                dispatch({
                    type: 'add',
                    payload: 'ipad'
                })
            }}>添加商品</button></p>
        </div>
    )
};

```



## 6、react中的传送门  createPortal 

**作用: 实现vue中的teleport 传送门的效果**

语法:  createPortal(要渲染dom结构或组件,dom结构显示的容器);

父组件

```jsx
import React from 'react'
// 引入模态框组件
import Modal from './Modal';
import { useState } from 'react'
export default function Father() {
    const [flag, setFlag] = useState(false)
    return (
        <div>
            <p>父组件</p>
            <button onClick={() => {
                setFlag(!flag)
            }}>显示弹窗</button>
            {
                flag ? <Modal setFlag={setFlag}></Modal> : <></>
            }
        </div>
    )
}

```



Modal 模态框子组件

```jsx
import React, { Component } from 'react'
import { createPortal } from 'react-dom'
export default class Modal extends Component {
    constructor() {
        super();
        // 创建元素 该元素就是模态框要显示的容器
        this.el = document.createElement('div');
        this.el.setAttribute('id', 'container');
        document.body.appendChild(this.el);

    }
    render() {
        return (
            createPortal(<div style={{
                position: 'fixed',
                top: 0,
                left: 0,
                background: 'rgba(0,0,0,0.3)',
                width: '100%',
                height: '100%',
                display: 'flex',
                justifyContent: 'center',
                alignItems: 'center'
            }} onClick={() => {
                document.body.removeChild(this.el);
                this.props.setFlag(false)
            }}>modal</div>, this.el)
        )
    }
}

```



## 7、Redux Toolkit (Rtk)

RTK:操作redux 的一个工具包

第一步: 需要下载安装 

```shell
npm install @reduxjs/toolkit
```

第二步: 在src目录下新建 store目录, 在该目录下创建index.js 文件, 内容可以使用官网的示例代码:

redux官网地址: https://cn.redux.js.org/introduction/getting-started

```js
//01: 引入rtk 中提供的两个方法
import { createSlice, configureStore } from '@reduxjs/toolkit';
//02: 引入切片, 相当于一个相当于vuex 中的一个模块,维护自己的数据
const counterSlice = createSlice({
  name: 'counter',  // 切片的命名,相当于vuex 的命名空间
  initialState: {  // 定义该模块的初始数据
    count: 0 
  },
  reducers: { // 定义修改该数据对应的方法, 相当于vuex 中的mutations
    addCount: (state) => { //不接收参数的情形
      state.count +=1
    },
    jianCount: (state,action) => { // 接收参数的情形
      state.count -= action.payload
    }
  }
})

//03:导出同步操作数据的方法
export const { addCount, jianCount } = counterSlice.actions;

//04: 创建store 仓库
const store = configureStore({
  reducer: counterSlice.reducer
})

//05:导出仓库, 并挂载到根实例上
export default store


```



在项目入口文件中,引入 store, 并挂载到根组件

```js
// 引入store
import store from './store';
import { Provider } from 'react-redux';
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <Provider store={store}>
    <Router>
      <App />
    </Router>
  </Provider>
);
```



在类组件页面中使用 store 仓库中的数据

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { addCount, jianCount } from '../../store';

class Myclass extends Component {
    render() {
        console.log(this);
        return (
            <div>
                类组件中使用
                <p>
                    <button onClick={this.props.add}>同步+1</button>
                    count:{this.props.count}
                    <button onClick={this.props.jian}>同步-1</button>
                </p>
            </div>
        );
    }
}

function mapstatetoprops(state) {
    return state
}
function mapdispatchtoprops(dispatch) {
    return {
        add() {
            dispatch(addCount(10))
        },
        jian() {
            dispatch(jianCount(5))
        },
    }
}

export default connect(mapstatetoprops, mapdispatchtoprops)(Myclass);
```

在函数组件页面中使用store 仓库中的数据

```jsx
import React from 'react';
// 引入react-redux 中的两个钩子函数
import { useSelector, useDispatch } from 'react-redux';
// 引入store 中抛出的两个方法
import { addCount, jianCount } from '../../store/index'
const Myfun = () => {
    const { count } = useSelector((state) => state);
    const dispatch = useDispatch();
    return (
        <div>
            函数组件中使用
            <p>
                {/*调用addCount方法*/}
                <button onClick={() => {
                    dispatch(addCount(2))
                }}>+2</button>
                count:{count}
                {/*调用jianCount方法*/}
                <button onClick={() => {
                    dispatch(jianCount(2))
                }}>-2</button>
            </p>
        </div>
    );
}

export default Myfun;

```



以上是最简单的操作方式, 但是当操作异步方法时, 并需要将store仓库模块化

将如上仓库进行模块化, 不同的模块管理自己仓库的数据, 在store 目录下, 新建reducers, 在该目录中新建userReducer.js 和 goodsReducer.js

goodsReducer.js 文件

```jsx
import axios from 'axios';
// 引入 createSlice 创建切片
import { createSlice } from '@reduxjs/toolkit';
const goodsSlice = createSlice({
    name: 'goods', // 相当于vuex中的命名空间
    initialState: { // 初始数据
        count: 0,
        goodArr: []
    },
    reducers: {
        addCount: (state, action) => {
            state.count += action.payload

        },
        jianCount: (state, action) => {
            state.count -= action.payload
        },
        addgoods: (state, action) => {
            state.goodArr = action.payload
        }
    }
})

// 导出同步操作数据的方法
export const { addCount, jianCount, addgoods } = goodsSlice.actions;
// 导出异步操作数据的方法
export const asyncAddGoods = (payload) => (dispatch) => {
    // setTimeout(() => {
    //     dispatch(addCount(payload))
    // }, 1000)
    axios.get('http://kumanxuan1.f3322.net:8001/index/index').then(res => {
        console.log(11, res);
        dispatch(addgoods(res.data.data.banner))
    })
}

export default goodsSlice


```

userReducer.js 文件

```jsx

import { createSlice } from '@reduxjs/toolkit';
const userSlice = createSlice({
    name: 'user', // 相当于vuex中的命名空间
    initialState: { // 初始数据
        userinfo: {
            name: '张三',
            age: 20
        }
    },
    reducers: {
        edituser: (state, action) => {
            state.userinfo = action.payload
        },
        addAge: (state, action) => {
            state.userinfo.age += action.payload
        }
    }
})

// 导出同步操作数据的方法
export const { edituser, addAge } = userSlice.actions;
// 导出异步操作数据的方法
export const asyncedituser = (payload) => (dispatch) => {
    setTimeout(() => {
        dispatch(edituser(payload))
    }, 1000)
}

export default userSlice

```

store文件中的index.js 文件

```js

import { configureStore } from '@reduxjs/toolkit';
// 分别引入抽离出去的两个数据模块 goodsSlice 和 userSlice
import goodsSlice from './reducers/goodsReducer';
import userSlice from './reducers/userReducer';

const store = configureStore({
    // 方式2: 使用模块化拆分的写法
    reducer: {
        goods: goodsSlice.reducer,
        user: userSlice.reducer
    }
})

export default store

```

将如上的store 导入 项目的入口文件index.js

```js
// 引入store
import store from './store';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <Provider store={store}>
    <Router>
      <App />
    </Router>
  </Provider>

);

```

在类组件的页面中使用store 仓库的数据

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
// 导入 对应的操作方法
import { addCount, jianCount, asyncAddGoods } from '../../store/reducers/goodsReducer';

class Myclass extends Component {
    render() {
        // console.log(this);
        return (
            <div>
                类组件中使用
                <div>
                    <button onClick={this.props.add}>同步+10</button>
                    count:{this.props.goods.count}
                    <button onClick={this.props.jian}>同步-10</button>
                    <button onClick={this.props.addAsync}>异步+100</button>
                    <ul>
                        {this.props.goods.goodArr.map((item) => {
                            return <li key={item.id}><img src={item.image_url} /></li>
                        })}
                    </ul>
                </div>
            </div>
        );
    }
}

function mapstatetoprops(state) {
    return state
}
function mapdispatchtoprops(dispatch) {
    return {
        add() {
            dispatch(addCount(10))
        },
        jian() {
            dispatch(jianCount(10))
        },
        addAsync() {
            dispatch(asyncAddGoods())

        },
    }
}

export default connect(mapstatetoprops, mapdispatchtoprops)(Myclass);

```

在函数组件中使用 store 仓库数据

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { addAge, asyncedituser } from '../../store/reducers/userReducer';
const Myfun = () => {
    const { userinfo } = useSelector((state) => state.user);
    const dispatch = useDispatch();
    return (
        <div>
            函数组件中使用
            <p>
                <button onClick={() => {
                    dispatch(addAge(1))
                }}>用户年龄+1</button>
                <button onClick={() => {
                    dispatch(asyncedituser({ name: "测试", age: 30 }))
                }}>修改用户信息</button>
                userinfo:{userinfo.name} -- {userinfo.age}
            </p>
        </div>
    );
}

export default Myfun;

```



创建react+ts 项目方式2种: 

基于vite 搭建的脚手架项目

```
npm init vite 
然后选择 react 版本 进而选择 ts 语法
```

或者使用 create-react-app 脚手架命令 (参照create-react-app官网) 基于webpack 搭建的脚手架项目

```shell
npx create-react-app my-app --template typescript
```



## 8、redux 数据的持久化配置

参考文档: https://blog.csdn.net/hbmern/article/details/124184309

store/index.js 文件配置

````js
import { createStore } from 'redux';

import { persistStore, persistReducer } from 'redux-persist'
import storage from 'redux-persist/lib/storage'
const defaultState = {
    count: 0,
}

//在localStorge中生成key为root的值
const persistConfig = {
    key: 'root',
    storage,
    blacklist: ['loading']  //设置某个reducer数据不持久化
}
const reducer = (state = defaultState, actions) => {
    if (actions.type == '+') {
        return {
            ...state,
            count: state.count + actions.payload
        }
    }
    return state
}

const myPersistReducer = persistReducer(persistConfig, reducer)
const store = createStore(myPersistReducer);

const persistor = persistStore(store)



export {
    store,
    persistor
}
````

入口文件修改

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

// 引入normalize.css 全局样式文件
import 'normalize.css';
// 配置路由
import { HashRouter, BrowserRouter as Router } from 'react-router-dom';
import { Provider } from 'react-redux';
// import store from './store';
import { store, persistor } from './store';
import { PersistGate } from 'redux-persist/integration/react'
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <Provider store={store}>
    <PersistGate loading={null} persistor={persistor}>
      <Router>
        <App />
      </Router>

    </PersistGate>
  </Provider>

);

```

在页面中操作redux 数据

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

const Mine = () => {
    const store = useSelector(state => state);
    const dispatch = useDispatch();
    console.log(store);
    return (
        <div>
            我的页
            <p onClick={() => {
                dispatch({ type: '+', payload: 1 })
            }
            }>count:{store.count}</p>
        </div>
    );
}

export default Mine;

```



## 9、RTK 的持久化配置

参考地址: https://decong.blog.csdn.net/article/details/125601749?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-125601749-blog-126864908.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-125601749-blog-126864908.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=1

// src/store/index.ts

```ts
//01:
import { createSlice, configureStore, combineReducers } from '@reduxjs/toolkit';

//导入持久化插件中的两个方法
import { persistStore, persistReducer } from 'redux-persist'
import storage from 'redux-persist/lib/storage'
//02: 创建切片
const userSlice = createSlice({
    name: 'user',
    initialState: {
        userid: '100',
    },
    reducers: {
        update: (state, action) => {
            state.userid = action.payload
        }
    }
})

export const rootReducer = combineReducers({
    counter: userSlice.reducer
})

const persistConfig = {
    key: 'root',
    storage,
    blacklist: []
}

const myPersistReducer = persistReducer(persistConfig, rootReducer)
// 03: 导出修改state 的方法
export const { update } = userSlice.actions

// 04: 创建仓库
export const store = configureStore({
    reducer: myPersistReducer,
    middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware({
            serializableCheck: false,
        })
})

// 05: 
export const persistor = persistStore(store)
```

// 入口文件
// src/index.tsx 

```ts
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { Suspense } from 'react'
// 引入重置样式表,帮助你清除一下 默认的标签样式 margin padding ... 
import 'normalize.css';

import { BrowserRouter as Router } from 'react-router-dom'
// 导入仓库
// import store from '@/store/index'
import { persistor, store } from '@/store';
import { PersistGate } from 'redux-persist/integration/react'
import { Provider } from 'react-redux'
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <Router>
    {/* 
       suspense 当陆游与匹配的组件还没有加载出来的时候, 显示 Suspense中的fallBack 对应的组件
       suspense :表示加载的意思, 配合 lazy 使用
    
    */}
    <Suspense fallback={<div>loading.....</div>}>
      <Provider store={store}>
        <PersistGate persistor={persistor}>
          <App />
        </PersistGate>
      </Provider>
    </Suspense>
  </Router>
);

```

# 十七、react中使用ts

创建一个 react+ts 的项目 
参考地址: https://create-react-app.bootcss.com/docs/getting-started

```shell
npx create-react-app 项目名称 --template typescript
```



操作项目前 需要在vscode 中安装插件 
![1672017875744](C:\Users\wusheng\AppData\Roaming\Typora\typora-user-images\1672017875744.png)



## 01: ts在类组件中的使用

```jsx
// 定义类组件
import React, { Component } from 'react';
// 声明 state 的数据类型
interface stateType {
    username: string
}
// Component 后面接收泛型约束, <属性props约束, 自身 的state约束>
class Father extends Component<any, stateType>{
    state: stateType = {
        username: '张三'
    }
    render() {
        return (
            <div>
                <p onClick={() => {
                    this.setState({
                        username: 1  //这样就会有类型校验
                    })
                }}>{this.state.username}</p>
            </div>
        );
    }
}

export default Father;

```

案例: 

```jsx
import React, { Component, createRef } from 'react';
import Son from './Son'
interface stateType {
    username: string
}
class Father extends Component<any, stateType>{
    state = {
        username: '张三'
    }
    ref1 = createRef<HTMLInputElement>();  // 声明ref 的类型
    editFn = () => {
        this.setState({
            username: '李四'
        })
    }
    render() {
        return (
            <div>
                {/* <p onClick={() => {
                    this.setState({
                        username: '123'
                    })
                }}>{this.state.username}</p> */}
                {/* 受控组件写法 */}
                <input type="text" value={this.state.username} onChange={(e) => {
                    this.setState({
                        username: e.target.value
                    })
                }} />
                {/* 非受控组件写法 */}
                <input ref={this.ref1} type='text' defaultValue={this.state.username} />
                <button onClick={() => {
                    // 写法1: 可选链写法
                    console.log(this.ref1?.current?.value);
                    // 写法2: 采用类型断言
                    console.log((this.ref1.current as HTMLInputElement).value);
                    this.setState({
                        username: (this.ref1.current as HTMLInputElement).value
                    })

                }}>提交表单</button>
				{/* 父传子传递参数*/}
                <Son username={this.state.username} editFn={this.editFn}></Son>
            </div >
        );
    };
}

export default Father;

```

Son子组件:

```jsx
import React, { Component } from 'react';
// 定义接收的属性的类型
interface propsType {
    username: string,
    editFn: () => void
}
 // 定义state 的类型
interface stateType {
 isshow: boolean
}    
 // 指定
class Son extends Component<propsType,stateType>{
    state = {
        isshow: true
    }
    render() {
        return (
            <div>
                <p>{this.props.username}</p>
                <button onClick={() => this.props.editFn()}>点击修改</button>
            </div>
        );
    }
}

export default Son;

```



## 02: ts在函数组件中的使用

```jsx
import React, { useState, useRef } from 'react';
import Son from './Son';

const Father = () => {
    // 如果非要设置了类行,可以 useState<类型>(初始值)
    const [username, setusername] = useState<string>('123');
    const ref1 = useRef<HTMLInputElement>(null);
    const changeUsernameFn = () => {
        setusername('测试')
    }
    return (
        <div>
            {/* 受控组件 */}
            {/* <input type='text' value={username} onChange={
                (e) => {
                    // setusername 函数本身会有隐式类行校验, 所以可以不用单独设置类行
                    setusername(e.target.value)
                }
            } /> */}
            {/* 非受控组件 */}
            <input type='text' ref={ref1} defaultValue={username} />
            <button onClick={() => {
                console.log((ref1.current as HTMLInputElement).value);
                setusername((ref1.current as HTMLInputElement).value)
            }}>提交表单</button>
            <Son username={username} changeUsernameFn={changeUsernameFn}></Son>
        </div>
    );
}

export default Father;

```

子组件Son组件:

```jsx
import React from 'react';
// 定义props 的属性类型
interface propsType {
    username?: string  //可设置为可选属性
    changeUsernameFn(): void
}
const Son = (props: propsType) => {
    return (
        <div>
            <p>{props.username}</p>
            <button onClick={() => {
                props.changeUsernameFn()
            }}>点击修改username</button>
        </div>
    );
}
export default Son;

```

注意函数子组件Son组件也可以 写成如下方式:

```jsx
import React from 'react';

interface propsType {
    username: string
    changeUsernameFn(): void
}
// propsType 为props 的类型
const Son: React.FC<propsType> = (props) => {
    return <div>
        <p>{props.username}</p>
        <button onClick={() => {
            props.changeUsernameFn()
        }}>点击修改username</button>
    </div>
}
export default Son;
```



## 03: ts在路由中的使用

在入口文件index.tsx  文件中引入;  当提示类型  'react-router-dom' 的类型错误的时候, 根据提示需要安装
npm i   "@types/react-router-dom"
 有些包  如axios, 该包文件中已经有类型声明了, 不需要额外安装,所以没好友类型提示

```jsx
 import { BrowserRouter as Router } from 'react-router-dom';
```

案例: 列表页 跳转到详情页

列表页: 

```tsx
import React from 'react';
import axios from 'axios';
import { useEffect, useState, } from 'react';
import { RouteComponentProps, useHistory } from 'react-router-dom';

// 如果需要使用props中提供的
const Home = () => {
    const [list, setList] = useState<Array<any>>([]);
    const his = useHistory();
    useEffect(() => {
        axios.get('http://47.94.148.165:3001/api/pro/list?count=1').then(res => {
           // console.log(res.data.data);
            setList(res.data.data);
        })
    }, []);
    return (
        <div>
            <ul>
                {
                    list.map((item, index) => {
                        return <li key={index} style={{ width: '100px', height: 'auto' }} onClick={
                            () => {
                                // props.history.push('/detail/' + item.proid)
                                his.push('/detail/' + item.proid)
                            }
                        }><img style={{ width: '100%' }} src={item.img1} /></li>
                    })
                }
            </ul>
        </div>
    );
}

export default Home;

```

详情页:

```tsx
import { useLocation, useParams } from 'react-router-dom';
import axios from 'axios';
import { useEffect, useState } from 'react';
interface paramsObj {
    id: string,
    [key: string]: any
}

interface resObj {
    img1: string,
    [key: string]: any
}

const Deatil = () => {
    const loc = useLocation();
    // 需要指定paramsObj 类型,否则 par.id 报错
    const par = useParams<paramsObj>();
    const [detailObj, setDetailObj] = useState<resObj | {}>({})
    // console.log(loc);
    // console.log(par.id)
    useEffect(() => {
        axios.get('http://47.94.148.165:3001/api/pro/detail/' + par.id).then(res => {
            // console.log(res);
            setDetailObj(res.data.data)
        })
    }, [])

    return <div>
        详情页
        <div>
            <img src={(detailObj as resObj).img1} alt="" />
        </div>
    </div>
}

export default Deatil
```



## 04: ts 在redux 中使用

定义store下的index.ts文件

```ts
import { legacy_createStore as createStore } from 'redux';

interface stateType {
    count: number,
    [key: string]: any
}

const defaultState: stateType = {
    count: 0
}

interface actionType {
    type: string,
    payload?: any
}

const reducer = (state: stateType = defaultState, action: actionType) => {
    if (action.type == '+') {
        state.count += action.payload
        return state
    }
    return state
}
// 此处声明一下window, 否则提示报错
declare let window: any;
const store = createStore(reducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());

export default store
```

在项目的入口文件 index.ts 中如下: 

```ts
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

import { BrowserRouter as Router } from 'react-router-dom';
import store from './views/redux/store';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <Provider store={store}>
    <Router>
      <App />
    </Router>
  </Provider>
);
```

在页面中使用store仓库的数据:

```tsx
import { useSelector, useDispatch } from 'react-redux';
const Car = () => {
    interface stateType {
        count: number,
        [key: string]: any
    }
    const state = useSelector((state: stateType) => state);
    const dispatch = useDispatch()
    console.log(state)
    return <div>
        购物车页
        <p onClick={
            () => {
                dispatch({ type: '+', payload: 10 })
            }
        }>{state.count}</p>
    </div>
};

export default Car
```



## 05: react-router-dom @v6版本

react-router-dom@6 是在2021/11  月份 推出的新的路由版本, 目前官方默认安装就是 6版本

```shell
npm i react-router-dom
```

**与 react-router-dom@5版本比较**

01:  内置组件的变化:  移除Switch 新增 Routes  , Routes 相当于 Switch ,移除 Redirect替代产品为Navigate组件

02:  语法变化:  component = {Home}  变成  element={<Home/>}

03:  新增 hooks  移除useHistory, 如:  useNavigate() 主要用来实现编程式导航的 等,没有了useHistory 这个hook

04:  新增 路由表, 新增  hooks 如:useRoutes , 语法:  使用const ele = useRoutes (路由表数组)  , 有点回到了vue 的 		感觉	

v6语法: 

### 01:引入 BrowserRouter

 在项目的入口文件 和v5一样, 使用browserRouter或 HashRouter 包裹整个App组件

```ts
// index.ts
import { BrowserRouter as Router } from 'react-router-dom';
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
    <Router>
      <App />
    </Router>
);

```

### 02:  定义声明式导航

```tsx
import React from 'react';
// 这里需要注意: 
// NavLink 和 Link 两个组件都可以使用, 区别在于一个有点击样式, 一个没有点击样式;
// NavLink选中的导航会有active 类名, 可以设置高亮
import { NavLink, Link } from 'react-router-dom';
import Rules from './Index';
type Props = {};

export default function Base({ }: Props) {
    return (
        <div>
            基础页
            {/*声明式导航*/}
            <p>
                <NavLink to='/home'>首页</NavLink>
                <NavLink to='/category'>分类页</NavLink>
            </p>
            {/* 路由坑 */}
            <Rules></Rules>
        </div>
    )
}
```

### 03:  定义路由规则组件Rules

```tsx
import React from 'react';
//v6中:  Routes  相当Switch. 必须使用该组件包裹Route 否则报错
//V6中: 还是使用 Route定义路由组件的坑及和路由规则
import { Routes, Route } from 'react-router-dom';
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
const Index = () => {
    return (
        <div>
            <Routes>
                {/* 需要将component 改成 element 同时赋值也必须是标签形式的组件 */}
                <Route path='/home' element={<Home />}></Route>
                <Route path='/category' element={<Category />}></Route>
            </Routes>
        </div>
    );
}

export default Index;

```

### 04: 路由重定向

```tsx
import React from 'react';
// 路由重定向: 
// 去掉了 Redirect组件, 使用 Navigate导航组件如下: 
// Navigate组件的特点: 只要页面渲染该组件,就会根据Navigate的to属性切换页面
import { Routes, Route,Navigate } from 'react-router-dom';
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
const Index = () => {
    return (
        <div>
            <Routes>
                <Route path='/home' element={<Home />}></Route>
                <Route path='/category' element={<Category />}></Route>
                <Route path='/detail' element={<Deatil/>}></Route>
                 {/* 路由重定向 */}
                <Route path='/' element={< Navigate to='/home' replace={true} />}></Route>
            </Routes>
        </div>
    );
}

export default Index;

```

注意: Navigate 组件特性demo 

实现效果:   在detail 详情页 当num值为2时,让其自动跳转到home页 

```tsx
// 在detail 详情页 当num值为2时,让其自动跳转到home页;

import React from 'react'
// 引入 Navigate 组件
import { Navigate } from 'react-router-dom'
import { useState } from 'react'
type Props = {}


export default function Detail({ }: Props) {
    const [num, setnum] = useState(0)
    return (
        <div>
            详情页
            <p>{num}</p>
            {/*当num值为2时,让其自动跳转到home页*/}
            {num == 2 ? <Navigate to='/home'></Navigate> : <></>}
            <button onClick={() => {
                setnum(num + 1)
            }}> +1</button>
        </div>
    )
}
```



### 05: 路由path 的大小写匹配

 注意 :当 NavLink或Link的to属性和 Route组件中的path(变成大写) 可以正常匹配

 注意: 当 Route组件中的path(变成大写)  ,同时设置 caseSensitive属性,则会区分 path的大小写

```tsx
// 声明式导航
 <p>
     <NavLink to='/home'>首页</NavLink>
     {/* path='/CATEGORY' path 大写 与该处的to 属性不匹配*/}
     <NavLink to='/category'>分类页</NavLink>
 </p>
.......

// 如下是路由规则组件
const Index = () => {
    return (
        <div>
            <Routes>
                <Route path='/home' element={<Home />}></Route>
                {/* path='/CATEGORY' path 大写*/}
                <Route path='/CATEGORY' caseSensitive element={<Category />}></Route>
                <Route path='/detail' element={<Deatil />}></Route>
                {/* 路由重定向 */}
                <Route path='/' element={< Navigate to='/home' />}></Route>
                {/*404路由*/}
                <Route path='*' element={< Notfind />}></Route>
            </Routes>
        </div>
    );
}

export default Index;
```

### 06: 使用路由5模式实现嵌套路由 (Routes+Route方式)

```tsx
// router/Index.tsx
import React from 'react'
// 配置路由规则
import { Routes, Route, Navigate } from 'react-router-dom';
//01: Routes相当于Switch, 保证每次只匹配一个路由
//02: Navigate为一个路由@6包中的一个内置组件, 该组件特点:就是只要在页面中显示, 就会执行该组件的
// to 属性, 就会直接跳转到to 属性对应的路由页面
// 注意: Navigate组件会自动执行跳转
//03: router@6默认是忽略了路由的path 的大小写的,如果需要设置严格匹配, 设置caseSensitive 属性
//04:
import Home from '../views/Home';
import Category from '../views/Category'
import Car from '../views/Car'
import Mine from '../views/Mine'
import Detail from '../views/Detail'
type Props = {}

export default function Index({ }: Props) {
    return (
        <Routes>
            {/* router@5语法 */}
            {/* <Route path='/home' component={ Home}></Route> */}
            {/* router@6语法 */}
            <Route path='/home' element={<Home />}></Route>
            <Route path='/category' caseSensitive element={<Category />}></Route>
             {/* 注意: 有二级路由的一级路由的path设置/car/*就可以啦  */}
            <Route path='/car/*' element={<Car />}></Route>
            <Route path='/mine' element={<Mine />}></Route>
            <Route path='/detail' element={<Detail />}></Route>
            {/* 路由重定向 */}
            <Route path='/' element={<Navigate to='/home'></Navigate>}></Route>
            
        </Routes>
    )
}
```

在一级路由car 组件中设置二级路由坑及二级路由的规则

```tsx
import React from 'react'
// 写嵌套路由
import { Routes, Route, Navigate } from 'react-router-dom'
import Car1 from '../views/Car1'
import Car2 from '../views/Car2'
type Props = {}

export default function Car({ }: Props) {
    return (
        <div>
            <p>Car页面</p>
            {/* 嵌套的路由组件 */}
            <Routes>
                {/*设置 默认匹配二级组件 */}
                {/* <Route path='' element={<Car1></Car1>}></Route> */}
                <Route path='car1' element={<Car1></Car1>}></Route>
                <Route path='car2' element={<Car2></Car2>}></Route>
                {/* 使用路由重定向设置默认显示的二级组件*/}
                <Route path='' element={<Navigate to='car1'></Navigate>}></Route>
            </Routes>
        </div>
    )
}
```



### 06:  定义路由表 (useRoutes[路由表数组]+outlet)

第一步: 改造成路由表,修改路由规则组件如下: 
useRoutes:  用来生成路由规则表的钩子函数



```tsx
import React from 'react';
//01:  引入 useRoutes 这个钩子函数, 用来定义路由生成路由规则表
import { Routes, Route, Navigate, useRoutes } from 'react-router-dom';
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
const Index = () => {
   //02: 创建路由规则表 element
    const element = useRoutes([
        {
            path: '/home',
            element: <Home />
        },
        {
            path: '/category',
            element: <Category />
        },
        {
            path: '/detail',
            element: <Deatil />
        },
        {
            path: '/',
            element: <Navigate to='/home' />
        }
    ])
    return (
        <div>
            {/* 使用路由规则表*/}
            {element}
        </div>
    );
}

export default Index;

```

将以上代码改造, 将对应的路由规则表的数组单独导出去,在router目录下,新建 routes.tsx (相当于vue中的routes),代码如下: 

routes.tsx文件: 

```tsx
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
import { Navigate } from 'react-router-dom';
export default [
    {
        path: '/home',
        element: <Home />
    },
    {
        path: '/category',
        element: <Category />
    },
    {
        path: '/detail',
        element: <Deatil />
    },
    {
        path: '/',
        element: <Navigate to='/home' />
    }
]
```

 Index.tsx文件: 

```tsx
import React from 'react';
import { Routes, Route, useRoutes } from 'react-router-dom';
// 引入路由规则
import routes from './routes'
const Index = () => {
    // 使用路由规则
    const element = useRoutes(routes)
    return (
        <div>
            {/*使用element*/}
            {element}
        </div>
    );
}

export default Index;

```



### 07: 嵌套路由

在路由表routes.tsx 中定义二级路由规则 ,写法同vue 基本一致

```tsx
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
import { Navigate } from 'react-router-dom'
// 引入二级路由组件Cate1 和 Cate2
import Cate1 from './Cate1';
import Cate2 from './Cate2';
export default [
    {
        path: '/home',
        element: <Home />
    },
    {
        path: '/category',
        element: <Category />,
        // 定义二级路由规则
        children: [
            {
                path: '',  // 表示默认匹配的二级路由
                element: <Cate1></Cate1>
            },
            {
                path: 'cate1', // 不要加 / 
                element: <Cate1></Cate1>
            },
            {
                path: 'cate2',
                element: <Cate2></Cate2>
            }
        ]
    },
    {
        path: '/detail',
        element: <Deatil />
    },
    {
        path: '/',
        element: <Navigate to='/home' />
    }
]
```

在二级路由对应的一级路由组件中定义二级路由组件显示的坑

Catefory.tsx文件(一级路由组件)

```tsx
import { NavLink, Outlet } from "react-router-dom";


const Category = () => {
    return <div>
        分类
        <p>
            {/* 
				写法1: 二级路由的to属性可以直接写 to='cate1'
				写法2: 二级路由的to属性可以直接写 to='/category/cate2'
			*/}
            <NavLink to='cate1'>cate1</NavLink>
            <NavLink to='/category/cate2'>cate2</NavLink>
        </p>
        {/* 
           子路由组件匹配到的坑,二级路由的坑为Outlet 组件,这个和vue有区别
		*/}
        <Outlet></Outlet>
    </div >
}
export default Category;
```



### 08: 声明式路由传参

案例: 首页 跳转到详情页

Home.tsx 组件

```tsx
import { useState } from 'react'
import { Link } from 'react-router-dom'
type Props = {};

export default function Home({ }: Props) {

    const [list, setlist] = useState([
        {
            id: 1,
            name: '张三'
        },
        {
            id: 2,
            name: '李四'
        },
        {
            id: 3,
            name: '王五'
        }
    ])


    return (
        <div>
            <div>首页</div>
            <ul>
                {list.map((item) => {
                    {/*第一种方式: 查询字符串方式*/}
                    {/* return <li key={item.id}><Link to=`/detail?id=${item.id}`>{item.name}</Link></li>*/}
                    
                    {/*第二种方式: restful风格 也有叫动态路由
                         这种方式需要修改路由配值表中的path/:id
					 */}
                    {/* return <li key={item.id}><Link to=`/detail/${item.id}`>{item.name}</Link></li>*/}
                    
                    {/*第三种方式; state 隐式传参
                     这里个v5有点区别, v5隐式传参都放在一个对象中 {pathname:url,state:{key:val}}
					*/}
                   return <li key={item.id}><Link to='/detail'  state={{id:item.id}}>{item.name}</Link></li>
                })}
            </ul>
        </div>

    )
}
```



### 09:编程式导航传参

Home.tsx 

```tsx
import { useState } from 'react';
// 01: 首先需要引入  useNavigate 这个钩子函数
import { Link, useNavigate } from 'react-router-dom'
type Props = {};

export default function Home({ }: Props) {

    const [list, setlist] = useState([
        {
            id: 1,
            name: '张三'
        },
        {
            id: 2,
            name: '李四'
        },
        {
            id: 3,
            name: '王五'
        }
    ])

    // 编程式导航函数
    // 02: 使用useNavigate 该函数创建一个对象
    const navigate = useNavigate()
    
    const gotoDetail = (item: any) => {
        // 03: 实现路由的跳转
        // 语法: navigate(path,配置对象) 配置对象配置的是state形式的传参,如果查询字符串参数直接拼接  			路径path 上
         navigate('/detail', {
            state: {
                id: item.id
            }
        })
    }
    return (
        <div>
            <div>首页</div>
            <ul>
                {list.map((item) => {
                    return <li key={item.id}><Link to='/detail?id=100'>{item.name}</Link>                    {/*通过点击button 实现编程式导航*/}
                   <button onClick={() => gotoDetail(item)}>查看详情</button></li >
                })}
            </ul >
        </div >
    )
}
```

Detail.tsx 页面

```tsx
import React from 'react'
// 01: 首先需要引入  useNavigate 这个钩子函数
import { Navigate, useParams, useLocation, useNavigate } from 'react-router-dom'
import { useState } from 'react'
type Props = {}


export default function Detail({ }: Props) {
    const [num, setnum] = useState(0);
    const par = useParams();
    
    // 02: 创建 navigate对象 
    const navigate = useNavigate();
    return (
        <div>
            详情页
            <button onClick={() => {
                //03:  返回上一页效果 下一页就传1
                navigate(-1) 
            }}>返回上一页</button>

            <p>{num}</p>
            {num == 2 ? <Navigate to='/home'></Navigate> : <></>}
            <button onClick={() => {
                setnum(num + 1)
            }}> +1</button>
        </div>
    )
}
```



























