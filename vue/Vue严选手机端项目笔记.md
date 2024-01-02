项目采用 vite + vue3+ts+pinaia+axios+sass +vant  搭建的一个前端项目

## 0. 详解接口地址

项目预览的地址: http://39.97.100.253/home
接口是一个doc 文档 



## 1. 初始化项目

```sh
npm init vue@latest
```

## 2. 配置项目

```sh
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit testing? … No / Yes
✔ Add Cypress for both Unit and End-to-End testing? … No / Yes
✔ Add ESLint for code quality? … No / Yes
✔ Add Prettier for code formatting? … No / Yes

Scaffolding project in ./<your-project-name>...
Done.
```

## 3.安装依赖

```sh
> cd <your-project-name>
> npm install
```



## 4.装插件

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

#### 1. 下载清楚默认样式的css 文件

```sh
// "normalize.css": "^8.0.1",
npm install normalize.css -S
```

```sh
//在 main.js,引入
import 'normalize.css'
```

#### 2. 添加scss支持

```sh
npm install sass -D
```

#### 3. App.vue样式处理

```vue
<style lang="scss">
html,
body,
#app {
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  width: 100%;
  max-width: 768px;
  min-width: 350px; /*no*/
  margin: 0 auto;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
}
</style>
```

#### 4. vant安装

[官网地址](https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart)

```sh
# Vue 3 项目，安装最新版 Vant
npm i vant -S
```

#### 5. vant配置自动按需引入

安装插件 [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components), 它可以自动引入组件，并按需引入组件的样式。相比于基础用法，这种方式可以按需引入组件的 CSS 样式，从而减少一部分代码体积。

```sh
npm i unplugin-vue-components -D
```

基于 `vite` 的项目，在 `vite.config.js` 文件中配置插件：

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

#### 6. 使用组件

完成以上两步，就可以直接在模板中使用 Vant 组件了，`unplugin-vue-components` 会解析模板并自动注册对应的组件。

```vue
<template>
  <van-button type="primary" />
</template>
```

#### 7. 函数组件的样式

Vant 中有个别组件是以函数的形式提供的，包括 `Toast`，`Dialog`，`Notify` 和 `ImagePreview` 组件。在使用函数组件时，`unplugin-vue-components` 无法自动引入对应的样式，因此需要手动引入样式。

```ts
// main.ts
import 'vant/es/toast/style'
import 'vant/es/dialog/style';
import 'vant/es/notify/style';
import 'vant/es/image-preview/style';
```



## 5. 创建404页面

`NotFoundView.vue`

```vue
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

```ts
import { createRouter, createWebHashHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import NotFound from '../views/NotFoundView.vue'

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

```ts
declare module "*.vue" {
  import { DefineComponent } from "vue"
  const component: DefineComponent<{}, {}, any>
  export default component
}
```

## 6. 定义footer 组件

```vue
<template>
    <van-tabbar active="{{ active }}" bind:change="onChange" :route="true">
        <van-tabbar-item icon="home-o" to="/home">home</van-tabbar-item>
        <van-tabbar-item icon="shopping-cart-o" to="/cart">cart</van-tabbar-item>
        <van-tabbar-item icon="friends-o" :to="computedpath">{{ computedContent }}</van-tabbar-item>
    </van-tabbar>
</template>

<script lang="ts" setup>

import { ref, computed } from 'vue';
import { useUserStore } from '@/stores/user'
let active = ref<number>(0);
// 切换导航
const onChasnge = (index: number) => { console.log(index); active.value = index }

// 设置footer中user模块中的图标
const userStore = useUserStore();
// console.log(44, userStore);

// 根据pinia 中的用户的信息判断当前用户的登录状态
const computedpath = computed(() => {
    return userStore.userId ? '/user' : '/login'
})
// 根据pinia 中的用户的信息判断当前用户的登录状态
const computedContent = computed(() => {
    return userStore.userId ? 'user' : 'login'
})
</script>
<style scoped>

</style>
```

将用户的userId  存到 pinia中, 然后根据userid 判断footer 的图标. 

在store/index.ts 

```ts
import { createPinia } from "pinia";
// 引入持久化插件
import { createPersistedState } from "pinia-persistedstate-plugin";
// 创建pinia 
const store = createPinia();
// pinia 使用持久化插件
store.use(createPersistedState());

export default store
```

在store/user.ts

```ts
// 定义用户相关的数据的操作
import { ref, computed } from 'vue';
import { defineStore } from 'pinia';

export const useUserStore = defineStore('user', () => {
    const userId = ref('');
    // const doubleCount = computed(() => count.value * 2)
    function setUserId(payload: string) {
        userId.value = payload
    }
    return { userId, setUserId }
})

```

## 7. 封装axios.ts 配置文件

在定义axios.ts 文件中,发现报错

```
找不到名称“process”。是否需要为节点安装类型定义? 请尝试使用 `npm i --save-dev @types/node`，然后将 “node” 添加到类型字段 
```

解决方法: 先安装 npm i --save-dev @types/node ,然后在项目根目录找到 tsconfig.json 文件中,添加编译字段

```json
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

axios 的封装:

```ts
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

本地开发环境需要做请求代理

```ts
 server: {
    proxy: {
      '/api': {
        target: 'http://121.89.205.189:3000',
        changeOrigin: true,
        // rewrite: (path) => path.replace(/^\/api/, '')
      },
    }
  }
```



## 8. 图片懒加载配置

安装依赖

```
  npm install vue3-lazy -S 
```

在main.ts中引入

```ts
import lazyPlugin from 'vue3-lazy';
// 注册时可以配置额外的选项 
app.use(lazyPlugin, {
    // lazyComponent: true,
    loading: '/image/loading.jpeg'
});
// 在页面中使用
 <img v-lazy='image_url'/>
```