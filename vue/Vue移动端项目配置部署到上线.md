例：项目采用 vite + vue3+ts+pinia+axios+sass +vant  搭建的一个前端项目

项目预览的地址: http://39.97.100.253/home

## 一. 初始化项目并安装依赖

```sh
npm init vue@latest
```

```sh
> cd <your-project-name>
> npm install
```

## 二.根据页面配置一级路由

```sh
import { createRouter, createWebHistory } from 'vue-router'
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      redirect: '/home',//路由重定向
    },
    {
      path: '/home',
      name: 'home',
      component: () => import('@/views/Home.vue')
    },
    {
      path: '/cart',
      name: 'cart',
      component: () => import('@/views/Cart.vue')
    },
    {
      path: '/category',
      name: 'category',
      component: () => import('@/views/Category.vue')
    },
    {
      path: '/mine',
      name: 'mine',
      component: () => import('@/views/Mine.vue')
    }
  ]
})
export default router
```

配置完一级路由后，选择路由出口，实现路由跳转

```sh
<!-- 一级路由组件出口 -->
  <RouterView></RouterView>
```



## 三.移动端配置装插件

### 1.1.vw布局

（1）vw布局第一种

安装如下插件,可以将我们设置的css单位px自动转成vw 单位,实现手机端/pc 页面的适配, 在项目的根目录创建 .postcssrc.js 文件,然后将如下代码写入该文件中.

```shell
npm i postcss-px-to-viewport -D
```

配置如上下载的插件, 试下页面中px 单位自定转换成vw 单位

```js
module.exports = () => {
    // 兼容vant
    return {
        plugins: {
            // 自动转换vw的postcss插件
            // 'postcss-px-to-viewport': {
            //   unitToConvert: "px",
            //   viewportWidth: 375, // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
            //   unitPrecision: 6, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
            //   viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
            //   fontViewportUnit: 'vw', // 字体转换后使用的单位
            //   propList: ['*', '!min-width', '!font*', '!border*'], // 要进行转换的属性列表，*表示匹配所有，！表示不转换
            //   selectorBlackList: ['.ignore', '.hairlines'], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
            //   minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
            //   mediaQuery: false, // 允许在媒体查询中转换`px`
            //   exclude: [/^(?!.*node_modules\/vant)/], // 第三方依赖不转换
            //   landscape: false, // 是否自动加入 @media (orientation: landscape)，其中的属性值是通过横屏宽度来转换的
            //   landscapeUnit: 'vw', // 横屏单位
            //   landscapeWidth: 1334 // 横屏宽度
            // },
            'postcss-px-to-viewport': {
                unitToConvert: "px",
                viewportWidth: 375, // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
                unitPrecision: 6, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
                viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
                fontViewportUnit: 'vw', // 字体转换后使用的单位
                propList: ['*', '!min-width', '!font*', '!border*'], // 要进行转换的属性列表，*表示匹配所有，！表示不转换
                selectorBlackList: ['.ignore', '.hairlines'], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
                minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
                mediaQuery: false, // 允许在媒体查询中转换`px`
                exclude: [/node_modules\/vant/i], // 第三方依赖不转换
                landscape: false, // 是否自动加入 @media (orientation: landscape)，其中的属性值是通过横屏宽度来转换的
                landscapeUnit: 'vw', // 横屏单位
                landscapeWidth: 1334 // 横屏宽度
            }
        }
    }
}
```

（2）vw布局第二种

安装如下插件,可以将我们设置的css单位px自动转成vw 单位,实现手机端/pc 页面的适配, 在项目的根目录创建 postcss.config.js 文件,然后将如下代码写入该文件中.

```js
npm i postcss-px-to-viewport -D
```

```js
module.exports = {
  plugins: {
    'postcss-px-to-viewport': {
      viewportWidth: 375,
    },
  },
};
```

1.2.rem布局

安装如下插件,可以将我们设置的css单位px自动转成vw 单位,实现手机端/pc 页面的适配, 在项目的根目录创建 postcss.config.js 文件,然后将如下代码写入该文件中.

如果需要使用 `rem` 单位进行适配，推荐使用以下两个工具：

- [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 是一款 PostCSS 插件，用于将 px 单位转化为 rem 单位

- [lib-flexible](https://github.com/amfe/lib-flexible) 用于设置 rem 基准值

  ```js
  npm i postcss-pxtorem
  ```

  ```js
  npm i lib-flexible
  ```

  ```js
  module.exports = {
    plugins: {
      'postcss-pxtorem': {
        rootValue: 37.5,
        propList: ['*'],
      },
    },
  };
  ```

  





### 2. 下载清楚默认样式的css 文件

```sh
// "normalize.css": "^8.0.1",
npm install normalize.css -S
```

```sh
//在 main.js,引入
import 'normalize.css'
```







### 3. 添加scss支持

```sh
npm install sass -D
```







### 4. vant安装

[官网地址](https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart)

```sh
# Vue 3 项目，安装最新版 Vant
npm i vant -S
```

### 5. vant配置自动按需引入

安装插件 [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components), 它可以自动引入组件，并按需引入组件的样式。相比于基础用法，这种方式可以按需引入组件的 CSS 样式，从而减少一部分代码体积。

```sh
npm i unplugin-vue-components -D
```

**基于 vite 的项目，在 vite.config.js 文件中配置插件：**

```ts
// ...
// 添加
import Components from 'unplugin-vue-components/vite';
import { VantResolver } from 'unplugin-vue-components/resolvers';

export default {
  plugins: [
    // ...
     vue(),
    // 添加
    Components({
      resolvers: [VantResolver()],
    }),
  ],
};

```

**如果是基于 `vue-cli` 的项目，在 `vue.config.js` 文件中配置插件：**

```js
// 添加
const { VantResolver } = require('unplugin-vue-components/resolvers');
const ComponentsPlugin = require('unplugin-vue-components/webpack');
// 添加
module.exports = {
  configureWebpack: {
    plugins: [
      ComponentsPlugin({
        resolvers: [VantResolver()],
      }),
    ],
  },
};
```



### 6. 函数组件的样式

Vant 中有个别组件是以函数的形式提供的，包括 `Toast`，`Dialog`，`Notify` 和 `ImagePreview` 组件。在使用函数组件时，`unplugin-vue-components` 无法自动引入对应的样式，因此需要手动引入样式。

```ts
// main.ts
import 'vant/es/toast/style'
import 'vant/es/dialog/style';
import 'vant/es/notify/style';
import 'vant/es/image-preview/style';
```





### 7. iOS 触发组件点击效果适配

这是因为 iOS Safari 默认不会触发 `:active` 伪类，解决方法是在 `body` 标签上添加一个空的 `ontouchstart` 属性：

```sh
<body ontouchstart="">
  ...
</body>
```





### 8.桌面端适配

Vant 是一个面向移动端的组件库，因此默认只适配了移动端设备，这意味着组件只监听了移动端的 `touch` 事件，没有监听桌面端的 `mouse` 事件。

如果你需要在桌面端使用 Vant，可以引入我们提供的 [@vant/touch-emulator](https://github.com/vant-ui/vant/tree/main/packages/vant-touch-emulator)，这个库会在桌面端自动将 `mouse` 事件转换成对应的 `touch` 事件，使得组件能够在桌面端使用。

```sh
# 安装模块
npm i @vant/touch-emulator -S
```

```sh
// 引入模块后自动生效
import '@vant/touch-emulator';
```



### 9.底部安全区适配

iPhone X 等机型底部存在底部指示条，指示条的操作区域与页面底部存在重合，容易导致用户误操作，因此我们需要针对这些机型进行安全区适配。Vant 中部分组件提供了 `safe-area-inset-top` 或 `safe-area-inset-bottom` 属性，设置该属性后，即可在对应的机型上开启适配，如下示例：

```js
<!-- 在 head 标签中添加 meta 标签，并设置 viewport-fit=cover 值 -->
<meta
  name="viewport"
  content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, viewport-fit=cover"
/>

<!-- 开启顶部安全区适配 -->
<van-nav-bar safe-area-inset-top />

<!-- 开启底部安全区适配 -->
<van-number-keyboard safe-area-inset-bottom />
```







## 四.创建404页面

创建notfound页面

```js
<template>
  <div class="notfound-container">
    <div class="header">
      <van-nav-bar left-arrow left-text="返回" @click-left="$router.go(-1)" />
    </div>
    <div class="content">
      <van-empty image="error" description="404 暂无该页面~" />
    </div>
  </div>
</template>
```

配置404页面的路由展示

```js
import { createRouter, createWebHashHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import NotFound from '../views/NotFound.vue'

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes: [
    // ...
    // 参考vue-router4.0官网
    { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  ]
})
```

解决引入vue组件ts报错, `env.d.ts`中加入如下代码

```js
declare module "*.vue" {
  import { DefineComponent } from "vue"
  const component: DefineComponent<{}, {}, any>
  export default component
}
```









## 五.axios请输数据的简单封装

**下载axios依赖包，在src上创建utils/request.ts封装一个axios请求**

```sh
npm i axios
```

```js
// 引入axios
import axios from "axios";
// 01:创建axios实例
const instance = axios.create({
    baseURL: 'http://kumanxuan1.f3322.net:8001/',
    timeout: 5000,
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' }// 默认表单编码格式
});
// 02:添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});

// 03:添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
}, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
});
// 04: 导出instance实例
export default instance
```



## 六.定义接口，统一管理

**在src文件下创建api文件，定义各种接口分类管理**

api/home.ts统一管理首页接口

```js
// 引入接口
import instance from "@/utils/request";

// 定义首页请求接口
export function gethomedata(data?: null) {// 不需要带参数
    return instance({
        url: '/index/index',
        method: 'get',
        params:data
    })
}

//在home.vue页面调用
// 调用首页接口
    gethomedata().then((res: any) => {
        // console.log(res);
        const { banner } = res.data.data // res.data.data是所有接口数据，结构banner
        bannerArr.value = banner
    })
```

其他分类接口  api/###.ts

```js
// 导入接口
import instance from '@/utils/request';
// 获取详情页数据
export function getgoodsdetail(data: { id: string }) {
    return instance({
        url: '/goods/detail',
        method: 'get',// get请求
        params: data
    })
}
//定义加入购物车接口
export function addcarapi(data: {
    goodsId: string,
    productId: string,
    number: number
}) {
    return instance({
        url: '/cart/add',
        method: 'post',// post请求
        data: data,
        headers: {
            'Content-Type': 'application/json'
        }
    })
}
// 定义携带参数的类型
interface topicType{
    page:number,
    size:number
}
// 定义专题接口
export function gettopicdata(data:topicType){
    return instance({
        url: '/topic/list',
        method: 'get',
        params:data
    })
}
// 方式二：
export const addCartHadnler = (userid: string, proid: string, num: number) => http.post('/cart/add', { userid, proid, num })

// 方式三
// export function login(username,password){
//     return instance.post("/admin/login",{
//         username,
//         password
//     })
// }
```





## 七.TypeScript 与组合式 API

**当使用 `<script setup>` 时，`defineProps()` 宏函数支持从它的参数中推导类型：**

```js
<script setup lang="ts">
const props = defineProps({
  foo: { type: String, required: true },
  bar: Number
})

props.foo // string
props.bar // number | undefined
</script>
```

**通过泛型参数来定义 props 的类型通常更直接：**

```js
<script setup lang="ts">
const props = defineProps<{
  foo: string
  bar?: number
}>()
</script>
```



**在src目录下创建types文件夹，types/hometype.ts定义类型**

```js
// 轮播图类型
export interface BannarType {
    id: number,
    image_url: string
}
```

components/Swiper.vue组件里 

```js
// 导入类型
import type { BannarType } from "@/types/hometype"
const props = defineProps<{
    bannerArr: Array<BannarType>
}>()
```

首页home.vue页面

```js
// home父组件
<template>
    <div class="home">
        <!-- 使用轮播图组件 -->
        <Swiper :bannerArr="bannerArr"></Swiper>//父传子数据
    </div>
</template>
<script setup lang="ts">
// 导入轮播图组件
import Swiper from '@/components/Swiper.vue'
// 导入类型
import type { BannarType} from "@/types/hometype"
// 定义轮播图数组
const bannerArr = ref<Array<BannarType>>([])
// 调用首页接口
gethomedata().then((res: any) => {
    console.log(res);
    const { banner} = res.data.data //  接口是首页所有数据，需要解构
    bannerArr.value = banner        //  数据赋值
})
</script>
<style lang="scss" scoped>
.home {
    width: 375px;
}
</style>
```





## 八.定义Footer组件

```js
<template>
    <van-tabbar :route="true" v-model="active" active-color="#008BD5" placeholder>
        <van-tabbar-item active="{{ active }}" to="/home" name="home" icon="home-o">首页</van-tabbar-item>
        <van-tabbar-item to="/topic" name="topic" icon="gift-card-o">专题</van-tabbar-item>
        <van-tabbar-item to="/category" name="category" icon="bar-chart-o">分类</van-tabbar-item>
        <van-tabbar-item to="/cart" name="cart" icon="cart-o">购物车</van-tabbar-item>
        <van-tabbar-item to="/mine" name="mine" icon="friends">我的</van-tabbar-item>
    </van-tabbar>
</template>
<script lang="ts" setup>
import { ref } from 'vue';
let active = ref<number>(0);
</script>
<style scoped>
</style>
```

在App.vue使用

```js
<script setup lang="ts">
// 引入footer组件
import Footer from './components/Footer.vue';
</script>
<template>
  <!-- 一级路由组件出口 -->
  <RouterView></RouterView>
  <!-- 使用footer组件自动定位 -->
  <Footer></Footer>
</template>
<style scoped></style>
```



## 九.pinia仓库实现持久化

 Pinia 是 Vue 的专属状态管理库，它允许你跨组件或页面共享状态。 和之前vuex 的功能是一样的, 可以理解为pinia 为 下一代的vuex工具

 **Pinia API 与 Vuex(<=4) 区别:**

- *mutation* 已被弃用。它们经常被认为是**极其冗余的** 
- 不再有嵌套结构的**模块** ,也就是说没有了modules,也就没有了 子模块中的命名空间

```sh
npm install pinia
```

在 *Setup Store* 中：

- `ref()` 就是 `state` 属性

- `computed()` 就是 `getters`

- `function()` 就是 `actions`

  

  src目录下创建store仓库

  ```js
  import { ref } from 'vue'
  import { defineStore } from 'pinia'
  export const useUserStore = defineStore('user', () => {
    // 定义数据状态state
    const token = ref<string>('')// 定义token
    interface userinfoType {  // 定义userinfo类型
      avatar: string
      gender: number
      id: number
      nickname: string
      username: string
    }
    const userinfo = ref<userinfoType | {}>({})  // 定义userinfo
  
    // 定义actions
    function settoken(data: string) {// 定义修改token
      token.value = data
    }
    function setuserinfo(data: any) {// 定义修改
      userinfo.value = data
    }
    return { setuserinfo, settoken, userinfo, token }// 导出数据state和方法actions
  })
  ```

  在其他文件使用仓库信息

  ```js
  // 导入user仓库的额方法
  import { useUserStore } from '@/stores/user'
  // 结构出ref方法包裹userStore不会丢失数据
  import { storeToRefs } from 'pinia';
  // 创建仓库
  const userStore = useUserStore()
  // console.log(userStore);
  // 解构state响应式数据
  const { token, userinfo } = storeToRefs(userStore)
  // 解构actions方法
  const { settoken, setuserinfo } = userStore
  ```

  



**pinia 数据持久化:**

安装  pinia-persistedstate-plugin

```js
npm install pinia-persistedstate-plugin
```

在 入口文件main.ts下, 配置store 的数据的持久化

```js
import { createPinia } from "pinia";
// 导入pinia持久化插件
import { createPersistedState } from "pinia-persistedstate-plugin";
// 配置持久化
const store = createPinia();
store.use(createPersistedState());
app.use(store)
```



## 十.使用meta路由原信息

在路由页面定义路由原信息

```js
import { createRouter, createWebHistory } from 'vue-router'
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      redirect: '/home',//路由重定向
    },
    {
      path: '/home',
      name: 'home',
      component: () => import('@/views/Home.vue'),
      meta: { isshowtabbar: true }
    }，
    {
      path: '/channel',
      name: 'channel',
      component: () => import('@/views/Channel.vue'),
      meta: { isshowtabbar: false }
    }
  ]
})
export default router

```

**在app.vue页面定义Footer组件**

```js
<Footer v-show="$route.meta.isshowtabbar"></Footer>
```





## 十一.图片懒加载配置

安装依赖

```js
npm install vue-lazyload 
```

在main.ts中引入

```js
import lazyPlugin from 'vue-lazyload';
// 注册时可以配置额外的选项 
app.use(lazyPlugin, {
    // 配置项，您可以根据需要进行配置
    // lazyComponent: true,
    loading: '占位符图片的URL',
    error: '加载错误时显示的图片URL',
});
// 在页面中使用
 <img v-lazy='实际图片的URL'/>
```







## 十二.设置反向代理解决跨域

查看文档：https://cn.vitejs.dev/config/server-options.html

**基于 vite 的项目，在 vite.config.js 文件中配置插件：**

本地开发环境需要做请求代理

```js
server: {// server配置是配置的开发服务器，只对开发服务器起作用，上线不起作用
    proxy: {
      '/api': {// 当请求地址中包含'api'字段时，请求的地址代理的target这个地址上
        target: 'http://kumanxuan1.f3322.net:8001/',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    }
  }
```

**在utils/request.ts文件下判断开发环境**

```js
// 判断是开发环境还是生产环境
let baseURL=''
// process.env.NODE_ENV 获取当前的项目的环境变量(全局变量)
// 当前运行的是 npm run dev命令，那么process.env.NODE_ENV为'development'
if(process.env.NODE_ENV == 'development'){
    baseURL = '/api'
}else{// 当前运行的是 npm run build命令，上线前进行的项目打包，那么process.env.NODE_ENV为'production'
    baseURL= 'http://kumanxuan1.f3322.net:8001/'
}
```

在定义utils/request.ts文件中,发现报错

```
找不到名称“process”。是否需要为节点安装类型定义? 请尝试使用 `npm i --save-dev @types/node`，然后将 “node” 添加到类型字段 
```

解决方法: 先安装 npm i --save-dev @types/node ,然后在项目根目录找到 tsconfig.json 文件中,添加编译字段

```js
npm i --save-dev @types/node
```

```js
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "./src/*"
      ]
    },
    "types": [
      "node"
    ],
  },
```

axios 的封装（例）:

```js
import axios from 'axios';
import { useRouter } from 'vue-router';
// 引入 Notify 样式组件
import { Notify } from 'vant';
switch (process.env.NODE_ENV) {
    case 'production':
        axios.defaults.baseURL = 'http://121.89.205.189:3000/api';
        break;
    case 'development':
        axios.defaults.baseURL = '/api'
        break;
}
// 配置超时时间
axios.defaults.timeout = 5000;
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    if (localStorage.getItem('token')) {
        if (config && config.headers) {
            config.headers.token = localStorage.getItem('token')
        }
    }
    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});
// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    console.log(response);

    // 对响应数据做点什么
    if (response.data && response?.data?.code == 200) {
        // 如果后端接口返回有token 数据,就将token 数据保存到保存到本地,作为一个token 有效期的的一个验证
        localStorage.setItem('token', response.data?.data?.token)
    } else {
        // 如果后端返回的状态码是10119,表示token 失效,返回到登录页
        if (response.data?.code == '10119') {
            localStorage.removeItem('token');
            const router = useRouter();
            router.push('/login')
        }
    }
    //  将后端的message 信息作为一个提示
   Notify({ type: response.data?.code == 200 ? 'success' : 'danger', message: response.data?.message });
    return response.data;
}, function (error) {
    // 对响应错误做点什么
    //此处服务器没有响应,可能是断网了或者服务器宕机了.
    return Promise.reject(error);
});
export default axios
```

