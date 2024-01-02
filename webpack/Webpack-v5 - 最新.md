

# 一、Webpack

## 1、介绍

### 1.1、webpack是什么

官网：https://webpack.js.org

中文网：https://www.webpackjs.com

**概念:**

![1677829275933](E:\webpack\img\1677829275933.png)



webpack是**一种前端资源构建（打包）工具（npm run build）**，一个静态模块打包器。在webpack看来，前端的所有资源文件（js/json/css/image/less/sass/vue,jsx,tsx.....）都会作为模块处理。它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源。webpack可以解决当前web开发中所面临的困境, 目前绝大多数企业中的前端项目，都是基于webpack进行打包构建的。

> 将浏览器不认识的代码转化成浏览器认识的操作, 并可以将代码进行一定程度的压缩，称之为打包，对应的工具就是打包工具。

![webpack](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/3ed87fed6859579595666cc45aabe79a5b41bce1.png?sign=d9bb626d5bf10643dc0625447a0b6312&t=5f436a97)



### 1.2、webpack的核心概念

核心概念即webpack的**五个核心配置项**。

-  **entry入口**

本项目应该使用哪个模块来作为构建其内部依赖图的开始（指定打包入口文件）。**打包入口文件默认为`src/index.js`**

​			react 脚手架默认的入口文件就是`src/index.js`, 
​			vue2 脚手架默认的入口文件就是 `src/main.js`

- **output输出**

在哪里输出它所创建的 bundles，以及如何命名这些文件，**打包输出文件默认值为`dist/main.js`**

​			 react 脚手架默认的输出文件为build目录
​             vue 脚手架默认的输出目录为 dist目录

- **loader加载器**

loader让webpack能够去处理那些非js文件,如 jpeg, png,css less,vue,jsx等等（webpack默认只能处理js文件和json 文件,并且是es5的语法,es6及以上不行）

- **plugins插件**

插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

​		如: 文件的压缩, 还有页面从public目录帮你赋值到打包后的dist或build 目录.

- **mode 模式**

通过选择 development（打包出来的代码是没有经过压缩的） 或 production（默认值，代码经过压缩的） 之中的一个，来设置 mode 参数，你可以启用相应模式下的 webpack 内置的优化



## 2、webpack的基本使用

### 2.1、基本的安装与配置

webpack是运行在node环境中的， Webpack 5 运行于 Node.js v10.13.0+ 的版本:

![1677832885993](E:\webpack\img\1677832885993.png)





 Webpack 有大量的配置项，可能会让你不知所措，请利用 [webpack-cli 的 init 命令](https://webpack.docschina.org/api/cli/#init)，它可以根据你的项目需求快速生成 webpack 配置文件，它会在创建配置文件之前询问你几个问题。 

```shell
npx webpack init
```

```shell
// 询问步骤
 Would you like to install '@webpack-cli/generators' package? (That will run 'npm install -D @webpack-cli/generators') (Y/n) y  yes
? Which of the following JS solutions do you want to use? ES6
? Do you want to use webpack-dev-server? Yes
? Do you want to simplify the creation of HTML files for your bundle? Yes
? Do you want to add PWA support? Yes
? Which of the following CSS solutions do you want to use? LESS
? Will you be using CSS styles along with LESS in your project? Yes
? Will you be using PostCSS in your project? Yes
? Do you want to extract CSS for every file? Only for Production
? Do you like to install prettier to format generated configuration? Yes
? Pick a package manager: npm


```

安装好后可以通过先前提及过的`npx`命令来检查webpack的版本以确定是否安装成功：

~~~shell
#查看webpack 版本 和 webpack-cli
npx webpack --version 
# npx可以帮助我们快速执行一些模块内部的命令 
~~~

至此 你已经创建了一个webpack 的基本的配置的项目了. 

```shell
//使用如下命令运行你的项目
npm run serve 
```



### 2.2、运行webpack项目

在项目根目录执行npm run serve 命令, 执行该脚本, 启动本地的一个 webpack-dev-server 开发服务器, 项目在该开发服务器上运行.

```shell
npm run serve
```



### 2.3、webpack的配置文件

#### 2.3.1、基本配置

我们在第一节的概述中提及了webpack的五个核心概念，这五个核心概念都属于webpack的配置，因此，如果需要更好的运用webpack，我们需要掌握其配置文件的相关知识点。

引入第三方包 jq包

```js
// 控制li奇数绿色, 偶数为红色
$('li:odd').css('background', 'red');
$('li:even').css('background', 'yellow');
```

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/a5e76f3aed14e3d9a3ccb5b40501e3f5fb7b8f48.png?sign=dbe83f863acf3e3d6d494fc4d574c2c9&t=5f8d4e65)

#### 2.3.2、配置代理

```shell
 devServer: {
    port: 8090,
    open: true,
    host: "localhost",
    proxy: {
      // 一旦devServer服务器接受到 /api开头的请求，就会把请求转发到另一个服务器
      "/api": {
        target: "http://kumanxuan1.f3322.net:8001",
        // 发送请求时，请求路径重写: 将/api 去除
        pathRewrite: {
          "^/api": "",
        },
      },
    },
```

```js
//  进行数据请求
axios.get('/api/index/index').then(res => {
    console.log('res', res);
})
```





## 3、加载器

### 3.1、加载器概述

在实际开发中，webpack只能打包处理以`.js`为后缀的模块（并且是其中一部分比较简单的JavaScript代码），其他非`.js`后缀的模块webpack默认处理不了，而需要调用loader加载器才能正常打包，否则会报错！

loader加载器可以协助webpack打包处理特定的文件模块了，例如：

- less-loader可以打包处理`.less`相关的文件
- sass-loader可以打包处理`.scss`相关的文件
- ...

![loader调用过程](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/08/5177ca4ed032a9d921e0a89019cae57688b548af.jpeg?sign=c490e9acb5a3903e934656c30956c3ac&t=5f4385a2)

### 3.2、处理样式

#### 3.2.1、处理css文件

在src/css/assets 目录中, 新建reset.css 文件.  

```css
body {
    font-size: 30px;
}
```

将css 文件引入到入口文件，然后运行 npm run serve 

```js
// 在入口文件中，引入对应的css 文件
require('../css/reset.css')

```

正如前面所说，webpack默认不能打包css文件，所以要想打包css文件，则需要安装css加载器，该加载器的安装命令为：

~~~shell
npm i -D style-loader css-loader
# css-loader：处理css后缀的文件的
# style-loader：将处理好的css文件样式以style标签的方式注入到页面的header标签内
~~~

安装好需要的加载器后需要对webpack进行配置，告诉webpack当遇到css后缀的文件应该交由哪个加载器去处理。在webpack打包命令**对应**的`module`的`rules`数组中添加css-loader规则：

~~~javascript
module: {
    rules: [{ test: /\.css$/, use: ["style-loader", "css-loader"] }],
},
~~~

在写加载器`use`的时候，需要注意：

- use数组中指定的加载器顺序是**固定**的，==顺序不能随意调换==

**以上配置在webpack5 脚手架中已经初始化配置了. 不需要单独配置了**



#### 3..2.2、处理less文件

在src/assets/目录中新建common.less 文件,

```less
body {
    ul {
        list-style: none;

        li {
            font-weight: bold;
        }
    }
}
```

将该文件引入到入口index.js 中

```js
// 在入口文件中引入less
import './assets/common.less'
```

要想通过webpack打包less文件，同样需要安装对应的加载器：

~~~shell
npm i -D less-loader less
~~~

安装好后也需要在对应的webpack配置文件中配置针对less文件打包的规则：

~~~javascript
module: {
    rules: [
        {test: /\.less$/,use: ["style-loader","css-loader","less-loader"]}
    ]
}
~~~

也是一样，需要注意顺序问题。

**以上配置在webpack5 脚手架中已经初始化配置了. 不需要单独配置了**



#### 3.2.3、抽取css（优化）

通过前面的学习，细心的同学会发现，打包好的html文件在浏览器运行时，“审查元素”时 能够看到所有的样式代码都以“style”标签写入了html，这些样式都是由JavaScript动态生成的，每次都这样动态生成：

- 消耗性能
  - 在html中本身是没有style代码的，而是通过js后续生成的，再塞入到html中

因此，此处可以对该部分进行改善操作。我们可以让webpack在打包时直接将这些样式抽成一个样式文件。步骤如下：

- 安装插件

  - npm i -D mini-css-extract-plugin

- 导入插件

  - const MiniCssExtractPlugin = require('mini-css-extract-plugin')

- 配置插件

  - ~~~javascript
    // 插件配置
    plugins: [
        new MiniCssExtractPlugin({
            filename: "css/[name]_[hash:6].css",  // 防止生成的css 文件名一致 所以hash 表示取随机码
        })
    ],
    ~~~

- 使用插件

  - MiniCssExtractPlugin.loader（使用它去**替换**之前的“style-loader”）, 这样就不会再生成内部样式而是单独的css文件,如下替换掉了style-loader
  
    ```js
     module: {
            rules: [
                { test: /\.css$/, use: [MiniCssExtractPlugin.loader, "css-loader"] },
                { test: /\.less$/, use: [MiniCssExtractPlugin.loader, "css-loader", 			"less-loader"] }
            ],
        },
    ```

- 配置package.json 文件, 指定 环境变量为 开发环境 ;

  ![企业微信截图_1678022198757](E:\webpack\img\企业微信截图_1678022198757.png)


**以上配置在webpack-cli5脚手架 中已经做了初始配置, 不需要单独配置了.**

- 运行 npm run build 命令就可以看见 打包后的css 文件

```js
//修改打包后的css文件输出目录
module.exports = () => {
    if (isProduction) {
        config.mode = 'production';

        config.plugins.push(new MiniCssExtractPlugin({
            filename: "css/[name]_[hash:6].css"  // 修改输出的文件目录
        }));


    } else {
        config.mode = 'development';
    }
    return config;
};
```



### 3.3、处理图片

> 在webpack5中，针对图片处理已经不再使用加载器了，可以使用webpack内置的Asset Module处理。

如果在样式表css/视图html中使用了图片（远程图片不受影响，本地图片受加载器的影响），则也需要配置对应的加载器才能进行正确的打包操作。加载器的安装命令如下：

~~~shell
npm i -D html-loader
~~~

安装好后也需要在webpack配置文件中进行对应的规则配置：

~~~javascript
module: {
    rules: [
      {
        test: /\.(png|jpeg|jpg|gif)$/i,
        type: 'asset/resource',
        generator: {
          filename: 'img/[name][ext]'  // 指定输出的图片文件的路径
        }

      },
      {
        test: /\.html$/,
        // 处理html中的img,负责引入img
        use: [
          { loader: "html-loader" },
        ]
      }
    ]
}
~~~

在/src/assets /image 目录下引入本地资源图片,  在index.html 设置img 标签图片和背景图

```html
// index.html 页面

<!-- img 标签图片 -->
 <img src='./src/assets/image/11.jpeg' />
 <!-- 背景图 -->
 <div class="bgimgbox"></div>
```



### 3.4、压缩处理图片

此外, 如上 还可以压缩处理图片,  减少图片体积, 使图片加载更快,  优化项目性能

需要安装如下loader

```shell
cnpm i file-loader image-webpack-loader -D
```

**注意** :  在这里url-loader 和 image-webpack-loader 不能一起使用，否则会导致图片出不来 , 这里可以使用file-loader(其实url-loader 处理图片本质也是使用得file-loader)

注意:   安装image-webpack-loader后, 如果运行报错, 可以先卸载掉该包避免残留, 然后使用cnpm i image-webpack-loader 可能npm镜像源的包中文件有缺失

具体配置如下

```js
 module: {
        rules: [
       		// .....省略上面代码
            {
                test: /\.(jpg|jpeg|png|gif)$/,
                type: 'asset/resource',
                generator: {
                  filename: 'img/[name][ext]'  // 指定输出的图片文件的路径
                },
                use: [
                {
                    loader: 'image-webpack-loader',
                    options: {
                        mozjpeg: { // 配置压缩 jpeg图片
                            progressive: true,
                            quality: 65
                        },
                        optipng: { //配合pngquant一起使用,共同压缩png图片
                            enabled: false,
                        },
                        pngquant: { //压缩png图片
                            quality: '65-90',
                            speed: 4
                        },
                        gifsicle: { // 压缩 gif
                            interlaced: false,
                        },
                        webp: { // 压缩webp
                            quality: 75
                        }
                    }
                }
                ]
            },
            {
                test: /\.html$/,
                // 处理html中的img(负责引入img,从而能被url-loader进行处理)
                use: [
                    { loader: "html-loader" },
                ]
            }
        ],
    },
```



### 3.5、处理js文件高级语法

webpack在不需要引入任何loader就可以对js进行打包处理，但是它不会对于js兼容性进行任何的处理，而我们编写的项目是需要在不同的浏览器中运行的，此时就需要对于js的兼容性在打包过程中进行对应的处理。我们可以使用babel来完成对应的js兼容处理。

如下 在入口文件中添加js 高级语法, 则编译会报错.  

```js
// 添加es6高级语法
class Person {
    static sex = '女'; // 当添加静态属性时,编译报错不支持
    constructor(name) {
        this.name = name
    }
    say() {
        console.log(this.name)
    }
}
let p = new Person('小芳')
console.log(p);
console.log(Person.sex);
```

**解决办法:** 

```
  {
        test: /\.(js|jsx)$/i,
        loader: "babel-loader",
   },
```

**以上配置在webpack-cli5脚手架 中已经做了初始配置, 不需要单独配置了.**



## 4、补充

### 4.1、entry多入口

前面我所使用webpack打包方式是针对单一入口的项目，那么如果项目中有多个入口需要处理，我们只需要将webpack的入口配置配置成多入口的形式即可。

例如，我们有`index.js`和`login.js`两个入口，则需要将entry入口配置写成以下形式：

~~~javascript
entry: {
    main: "./src/index.js",
    login: "./src/login.js",
    // ....
},
~~~

打包后自动在dist 目录下的html 文件中 引入对应的js文件

```html
<script src="../js/main.js"></script>
<script src="../js/login.js"></script></body>
```



### 4.2、路径别名与默认后缀（简化）

> 在vue工程化的时候，@=/src，vue脚手架中默认配置, react 默认没有配置, 需要在setupProxy.js中配置

在先前学习Vue的时候提及并使用过`@`，当时说`@`表示`src`目录，这样一来，在我们自己写的代码中作文件导入的时候路径写起来比较轻松，但是当前的项目中如果使用`@`导入在打包时就会出现类似如下的错误：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/e4a3ceb328a3125e9036d3673132ca3db05967af.png?sign=ba05f1ab98d80657d5f387fb091e9bae&t=5f9158f8)

这就说明，当前项目并不支持我们在导入路径中使用`@`符号，如果想要支持之前的这种简化写法，需要我们配置解析规则（vue项目中是工具已经帮我们配置好了，所以当时可以直接使用）。

**配置方式如下：**修改webpack的配置文件，在配置选项中添加`resolve`选项，增加别名配置：

~~~javascript
resolve: {
    // 配置解析模块路径别名：优点简写路径，缺点路径没有提示
    alias: {
        // 定义一个@，可在import引入时使用
        "@": path.join(__dirname, "./src"),
    },
    // 设置可以忽略不写的后缀
    extensions: [".js",".vue",".jsx"]  // 在webpack5 后缀不需要处理设置了. webpack5之前需要改配置
},
~~~

这时，再写模块导入路径的时候即可简化路径写法。



### 4.3、忽略打包（优化项目性能）

webpack中的`externals`选项提供了不从bundle捆绑中使用依赖的方式。

在开发项目时，有些外部模块通过`CDN链接`使用`script`标签引入到页面中可能要比通过打包使用更加方便，例如jQuery库。此时就可以使用`externals`忽略打包的方式去指定哪些库不需要webpack进行打包。

请注意，不被webpack打包的外部依赖，后期依旧可以通过以下方式在项目中使用：

- `script`标签链入远程/本地js文件（推荐）

`externals`选项指定实现的方式比较简单，**只需要在webpack配置文件中添加externals选项指定需要忽略的包信息即可**。

例如，需要忽略对jQuery的打包，则可以写成：

~~~javascript
externals: {
    jquery: 'jQuery',
    // .....
}
~~~

可以在打包就的dist 文件中查看js文件打包后的体积和之前没有做忽略的对比. 这样就比较出来了.当然这样会导致打包后的文件不能使用jquery,解决方法可以在页面中引入jquery



### 4.4、打包处理vue文件

与前面的css、less、scss等文件一样，webpack默认是不能处理vue后缀的文件的，如果需要让其支持打包处理vue文件，也需要安装和配置对应的loader加载器。

可以先在项目中安装vue框架，随后写一个vue文件供测试使用：

~~~shell
npm i vue 
~~~

所提供的demo测试vue文件代码`/src/App.vue`：

~~~vue
<template>
    <div class="app">{{ msg }}</div>
  </template>
  
  <script>
  export default {
    name: 'App',
    data () {
      return {
        msg: 'Hello world'
      }
    }
  }
  </script>
  
  <style  scoped>
  
  </style>

~~~

再去创建vue的访问入口文件`/src/main.js`：

~~~javascript
import { createApp } from 'vue'
import App from './App'

const app = createApp(App)

app.mount('#app')

~~~

不要忘记在html文件中写上渲染的容器：

~~~html
<div id="app">
    
</div>
~~~

在打包入口中指定vue文件的入口：

~~~javascript
 entry: "./src/main.js",
~~~

如果在打包时出错，则需要安装对应的loader并且配置，安装指定如下：

~~~shell
npm i vue-loader @vue/compiler-sfc -D
~~~

安装好对应的加载器后需要进一步配置打包规则，首先需要在webpack的配置文件中引入vue-loader的插件：

**Error: [VueLoaderPlugin Error] No matching use for vue-loader is found.** 将 rules 中的loader 规则放到首位就可以啦!!

~~~javascript
const { VueLoaderPlugin } = require('vue-loader');

module.exports = {
    plugins: [
        new VueLoaderPlugin(),
    ],
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: "vue-loader",
            },
        ]
    }
}
~~~

还可以添加 vue-router

```shell
npm i vue-router
```

在 '/src/router/index.js'文件中编辑路由代码

```js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '@/views/Home'
import Category from '@/views/Category'
const routes = [
    {
        path: '/',
        redirect: '/home'
    },
    {
        path: '/home',
        name: 'home',
        component: () => import('@/views/Home.vue')
    },
    {
        path: '/category',
        name: 'category',
        component: () => import('@/views/Category.vue')
    }
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router
```

在 '/src/views' 目录下创建Home 页面组件和Category 页面组件. 

```vue
// Home.vue
<template>
  <div>
    首页
  </div>
</template>

<script>
export default {
    data() {
        return {
            
        }
    }
}
</script>
<style scoped>

</style>
```

修改App.vue中

```vue
<template>
    <div class="app">{{ msg }}</div>
     <!-- 添加路由 和路由出口 -->
    <router-link to="/home">首页</router-link>
    <router-link to="/category">分类</router-link>
    <router-view></router-view>
</template>
  
<script>
export default {
    name: 'App',
    data() {
        return {
            msg: 'Hello world'
        }
    }
}
</script>
  
<style scoped></style>

```

面试题:
loader 与plugin 的区别

 **Loader**直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件和json文件，如果想将其他文件也打包的话 比如( 针对css，图片,.vue等格式的文件没法打包 ), 就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。 

使用:  **Loader**在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options） 

**常用的Loader:**

- 样式：style-loader、css-loader、less-loader、sass-loader等

- 文件：file-loader 、url-loader,vue-loader ... 等

  

 **plugin** 主要做打包优化和压缩，到重新定义环境变量，比loader的功能更加强大。webpack提供了很多开箱即用的插件：

参考地址: https://blog.51cto.com/u_15283585/5156299

**常用的plugin:**

 01: html-webpack-plugin ;  作用:完成了html文件的拷贝，打包，还给html中自动增加了引入打包后的js文件的代码（），还能指明把js文件引入到html文件的底部等等。 

