# 一、项目创建

**(1): 如果是进行vue2 项目开发,则需要安装vue2 对应的插件 vetur**

**(2): 如果是进行vue3 项目开发,则需要安装vue3 对应的插件 volar**

# 二、引入组件库Element-Plus

[Element-Plus 官网地址](https://element-plus.gitee.io/zh-CN/)

### 1. 安装依赖

```sh
npm install element-plus --save
```

### 2. 自动按需导入

这种方式不需要导入任何组件，可以直接使用

```sh
npm install -D unplugin-vue-components unplugin-auto-import
```

### 3.引入css样式

```js
// main.is
import 'element-plus/dist/index.css'
```

### 4. 配置文件配置

```js
// vite.config.js
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

### 5.引入Icon 图标

官网：https://element-plus.gitee.io/zh-CN/component/icon.html#%E6%B3%A8%E5%86%8C%E6%89%80%E6%9C%89%E5%9B%BE%E6%A0%87

下载依赖：

```sh
npm install @element-plus/icons-vue
```

方式一：注册所有图标

```js
// main.js
// 如果您正在使用CDN引入，请删除下面一行。
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

const app = createApp(App)
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

方式二：按需导入

您需要从 `@element-plus/icons-vue` 中导入图标。vue2需要components进行注册

```js
// 使用图标
<!-- 使用 el-icon 为 SVG 图标提供属性 -->
<template>
  <div>
    <el-icon>
      <Edit />
    </el-icon>
    <!-- 或者独立使用它，不从父级获取属性 -->
    <Edit />
  </div>
</template>
// 导入图标
import { Edit } from '@element-plus/icons-vue' // 首字母必须大写
```





# 三、配置路由Router

- vue2项目要下载第三版本的路由
- vue3项目要下载第四版本的路由

### 1. 依赖安装

```sh
npm install vue-router@4 -S
```

### 2. 创建文件目录 

src/router/index.js(ts)

```js
import { createRouter, createWebHashHistory} from "vue-router";
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      redirect: '/login'  // 重定向
    },
    {
      path: '/login',
      component: () => import('@/views/Login.vue'),// 路由懒加载
    }
  ]
})
export default router
```



### 3. 创建notfound页面

```js
<template>
    <div>
        <el-result icon="warning" title="404提示" sub-title="你找的页面走丢了---">
            <template #extra>
                <el-button type="primary" @click="$router.push('/index')">回到首页</el-button>
            </template>
        </el-result>
    </div>
</template>
```

### 4. 配置404页面的路由展示

```js
// router.js
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from '../views/Home.vue'
import NotFound from '../views/NotFound.vue'

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes: [
    // ...
    { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  ]
})
```



### 5. 配置路由前置守卫

```js
// ...
import { getToken } from '@/composbles/auth'
import { toast } from '@/composbles/util'
// 路由全局前置守卫
router.beforeEach((to,from,next)=>{
    const token = getToken()
    // 没有登录，强制跳转回登录页
    if(!token && to.path != "/login"){
        toast("请先登录","error")// 封装好的弹窗提示
        return next({ path:"/login" })
    }
    // 防止重复登录
    if(token && to.path == "/login"){
        toast("请勿重复登录","error")
        return next({ path:from.path ? from.path : "/" })
    }
    next()
})
export default router
```



### 6. 入口文件导入

```js
// main.js
// ...
import router from "./router/index"
createApp(App).use(router).use(...).mount('#app')
```

# 四、配置src路径别名

```js
//vite.config.js
// ...
import path from "path"
export default defineConfig({
  plugins: [],
  // 添加内容
  resolve:{
    alias:{
      "@":path.resolve(__dirname,"src")
    },
  }
})
```





# 五、axios数据请求

### 1. 安装axios

```sh
npm install axios -S
```

### 2. axios简单封装

##### src下创建utils/request.js

```js
// F封装axios 数据请求
// 01: 将axios 从 node_modules中导入
import axios from 'axios';
//02: 创建一个axios 的实例
// 注意: instance 就是axios
// instance 其实相当于改装过的axios 
const instance = axios.create({
    baseURL: 'http://47.94.148.165/admin', // 基础请求地址
    timeout: 5000,  // 请求的延时时间, 当前端发起请求后多少秒 请求请求自动中断
    headers: { 'Content-Type': 'application/json' } //只针对post请求其作用 设置post 请求头参数的提交格式
});
//02-1 该配置和02配置一样, 只不过如下是一个一个单独配置的
// axios.defaults.baseURL = 'https://api.example.com';
// axios.defaults.timeout = 5000;
// axios.defaults.headers.post['Content-Type'] = 'application/json';
//03: 添加请求拦截器
instance.interceptors.request.use(function (config) {
    // 在使用axios 请求数据前, 此时请求还没有发送给后端,被我拦截器拦下来了
    // 可以在该位置做一个请求的配置,最常用的就是在该位置添加 token,
    // 为什么使用token  因为http 请求时无状态的, 两个请求之间没有任何关联,所以需要 
    // 上一次请求时, 返回token, 作为下一次请求校验
    // 往header头自动添加token
    const token = getToken()
    if(token){
        config.headers["token"] = token
    }
    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});
//04: 添加响应拦截器
instance.interceptors.response.use(function (response) {
    // 当响应发给前端前, 被响应拦截器拦截,该位置一般可以用来 
    // 01: 根据后端返回的状态码 来对所有的响应做一个处理 例如 提示 
    // 02: 更新token 用来做登录的时长校验
    ElMessage({
        message: response.data.message,
        type: response.data.code == 200 ? 'success' : 'error',
        duration: 1000
    })
    return response;
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
});
//05:导出 instance
export default instance

```



#####  src下创建aip/login.js

定义登录接口

```js
// 该文件用来定义所有的接口地址
//01: 引入axios
import instance from "@/utils/request";

// 定义登录接口方式一
export function loginApi(data) {
    // data 就是参数
    return instance({
        method: 'post',
        url: '/admin/login',
        data: data,
        // headers: {'X-Requested-With': 'XMLHttpRequest'},
    })
}
// 方式二：
// export function login(username,password){
//     return instance.post("/admin/login",{
//         username,
//         password
//     })
// }

// 方式三：
// export const login = (username, password) => instance.post('/admin/login', { username, password })
```





### 3.   设置反向代理解决跨域

查看文档：https://cn.vitejs.dev/config/server-options.html

基于 vite 的项目，在 vite.config.js 文件中配置插件：

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





# 六、引入vuex

### 1. 依赖安装

```sh
npm install vuex@next --save
```

### 2. 创建文件目录

src/store/index.js

```js
// 定义store 仓库
import { createStore } from 'vuex';
const store = createStore({
    state() { // 存放的全局数据
        return {
            userinfo: {}  // 存放用户信息
        }
    },
    mutations: {// 定义所有的同步方法
        setuserinfoFn(state, data) {
            state.userinfo = data
        }
    },
    actions: {// 定义所有的异步方法
    },
    getters: {
    },
    modules: {
    }
})
export default store
```

### 3. 入口文件导入

```js
// main.js
import store from './store/index'

createApp(App).use(router).use(store).mount('#app')
```



### 4. 实现vuex数据持久化

第一步：在项目中安装插件

```js
npm install vuex-persistedstate --save
```

第二步：使用vuex-persistedstate默认存储到localStorage中

```js
// 定义store 仓库
import { createStore } from 'vuex';
import createpersistedstate from 'vuex-persistedstate'
const store = createStore({
    state() { // 存放的全局数据
        return {
            userinfo: {}  // 存放用户信息
        }
    },
    mutations: {// 定义所有的同步方法
        setuserinfoFn(state, data) {
            state.userinfo = data
        }
    },
    actions: {// 定义所有的异步方法
    },
    getters: {
    },
    modules: {

    },
    plugins: [ // 配置永久化存储插件
        createpersistedstate()// 默认存储localStorage中
    ],
})
export default store
```

第三步：也可以指定存储到sessionStorage中

```js
import createPersistedState from "vuex-persistedstate"
const store = new Vuex.Store({
   state: {},
   mutations: {},
   actions: {},
   plugins: [createPersistedState({
       storage:window.sessionStorage  // 存储sessionStorage中
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









# 七、其他

### 1、项目样式重置

1. 依赖安装(第三方的重置样式包)

```sh
npm install normalize.css -S
```

2. 入口文件引入

```js
// main.js
import 'normalize.css'
```





### 2、Windi CSS样式库的使用

官网：https://cn.windicss.org/guide/

（1）.依赖安装

```sh
npm i -D vite-plugin-windicss windicss
```

（2）.在你的 Vite 配置中添加插件：

```js
//vite.config.js
import WindiCSS from 'vite-plugin-windicss'

export default {
  plugins: [
    //添加
    WindiCSS(),
  ],
}
```

（3）.在你的 Vite 入口文件中导入 `virtual:windi.css`：

```js
//main.js
import 'virtual:windi.css'
```

（4）vscode代码提示插件： **WindiCSS IntelliSense**  





### 3、键盘监听

```js
// 监听回车事件
function onKeyUp(e){
    if(e.key == "Enter") onSubmit()//执行提交事件
}
// 添加键盘监听
onMounted(()=>{
    document.addEventListener("keyup",onKeyUp)
})
// 移除键盘监听
onBeforeUnmount(()=>{
    document.removeEventListener("keyup",onKeyUp)
})
```



### 4、全局loading进度条实现

文档：https://www.npmjs.com/package/nprogress

**（1）下载依赖**

```sh
npm i nprogress
```

**（2）引入css**

```js
// main.js
import "nprogress/nprogress.css"
```

**（3）封装方法**

```js
import nprogress from 'nprogress'
// 显示全屏loading
export function showFullLoading(){
  nprogress.start()
}
// 隐藏全屏loading
export function hideFullLoading(){
  nprogress.done()
}
```

**（4）全局前后守卫使用**

```js
import { 
    showFullLoading,
    hideFullLoading
} from "~/composables/util"
// 全局前置守卫
router.beforeEach(async (to,from,next)=>{
    // 显示loading
    showFullLoading()
	next()
})
// 全局后置守卫
router.afterEach((to, from) => hideFullLoading())
```



### 5、引入Cookie存储数据

方式一：

官网：https://vueuse.org/integrations/useCookies/#usecookies

下载依赖：

```sh
npm i @vueuse/integrations
```

```sh
npm i universal-cookie
```

定义js文件简单封装

```js
import { useCookies } from '@vueuse/integrations/useCookies'
const TokenKey = "admin-token"
const cookie=useCookies()
// 获取token
export function getToken(){
    return cookie.get(TokenKey)
}
// 设置token
export function setToken(token){
    return cookie.set(TokenKey,token)
}
// 清楚token
export function removeToken(){
    return cookie.remove(TokenKey)
}
```



方式二：

1. **安装插件：** 你可以使用 `vue-cookies` 这样的插件来方便地操作 Cookie。首先，安装该插件：

```sh
npm install vue-cookies
```

1. **在 Vue 项目中使用：** 在你的 Vue 项目中，你需要引入并配置 `vue-cookies` 插件。在你的入口文件（例如 `main.js`）中进行配置：

```js
// main.js
import Vue from 'vue';
import VueCookies from 'vue-cookies';

Vue.use(VueCookies);

// 设置全局的 Cookie 选项，例如过期时间等
Vue.$cookies.config('7d'); // 设置 Cookie 的过期时间为 7 天
```

1. **使用 Cookie 存储和获取数据：** 一旦配置完毕，你就可以在组件中使用 `this.$cookies` 来进行 Cookie 数据的存储和获取。

```js
export default {
  methods: {
    saveDataToCookie() {
      // 存储数据到 Cookie
      this.$cookies.set('key', 'value');
    },
    getDataFromCookie() {
      // 从 Cookie 中获取数据
      const value = this.$cookies.get('key');
      console.log(value);
    },
  },
};
```

请注意，Cookie 适合存储少量的数据，因为每个请求都会将 Cookie 发送到服务器，过多的 Cookie 数据可能会影响性能。对于较大的数据存储需求，你可能会考虑使用其他持久化方案，如 Local Storage 或 Vuex 插件。

此外，考虑到安全性，不应该将敏感信息存储在 Cookie 中，因为 Cookie 可以在浏览器中被查看和修改。对于敏感数据，应该采取更安全的存储方法。



### 6、全屏显示

文档：https://vueuse.org/guide/#installation    https://vueuse.org/core/usefullscreen/#usage

**（1）下载依赖**

```sh
npm i @vueuse/core
```

**（2）使用**

```js
import { useFullscreen } from '@vueuse/core'
const { isFullscreen, enter, exit, toggle } = useFullscreen()//是否全屏，进去全屏，推出全屏，屏幕切换
```





### 7、数字滚动

文档：https://www.npmjs.com/package/gsap

**（1）下载依赖**

```sh
npm i gsap
```

**（2）封装组件**

```vue
<template>
    {{ d.num.toFixed(0) }}// 取整
</template>
<script setup>
import { reactive,watch } from "vue"
import gsap from "gsap"// 引入依赖
const props = defineProps({// 接受父组件传的值
    value:{
        type:Number,
        default:0
    }
})
const d = reactive({
    num:0
})
function AnimateToValue(){
    gsap.to(d,{
        duration:0.5,// 动画时常
        num:props.value// d的num的值变成父组件传过来的值
    })
}
AnimateToValue()
watch(()=>props.value,()=>AnimateToValue())// 监听props.value的值执行AnimateToValue
</script>
```



### 8、根据屏幕进行响应变化

文档：https://vueuse.org/core/useresizeobserver/#usage

```vue
// 页面使用绑定节点
<template>
  <div ref="el">测试</div> 
</template>
<script setup>
import { useResizeObserver } from '@vueuse/core'
const el = ref(null)
useResizeObserver(el, (entries) => myChart.resize())// 根据屏幕的大小eChart图表跟着变化
</script>    
```



### 9、Animate.css 动画库

文档：https://animate.style/#migration

**（1）下载依赖**

```sh
npm install animate.css --save
```

**（2）页面使用**

```js
import 'animate.css';
```

**（3）查看代码**

根据：https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.css 文档查看动画代码

注：配合过渡动画使用

```vue
<transition name="fade">
    // 过度动画内容
</transition>
<style>
.el-aside{transition: all 0.2s;}
.fade-enter-from{opacity: 0;}// 进入前
.fade-enter-to{opacity: 1;}// 进去后
.fade-leave-from{opacity: 1;}// 离开前
.fade-leave-to{ opacity: 0;}// 离开后
.fade-enter-active,.fade-leave-active{transition: all 0.3s;}
.fade-enter-active{transition-delay: 0.3s;}// 延迟
</style>
```



### 10、滚动条css设置

```css
::-webkit-scrollbar 滚动条整体部分
::-webkit-scrollbar-thumb 滚动条里面的小方块，能向上向下移动
::-webkit-scrollbar-track 滚动条的轨道（里面装有 Thumb
::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去
::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件注意此方案有兼容性问题，一般需要隐藏滚动条时我都是用一个色块通过定位盖上去，或者将子级元素调大，父级元素使用 overflow-hidden 截掉滚动条部分。暴力且直接。
```





### 11、自动获取焦点

注册一个自定义指令有全局注册与局部注册

全局注册主要是通过`Vue.directive`方法进行注册

`Vue.directive`第一个参数是指令的名字（不需要写上`v-`前缀），第二个参数可以是对象数据，也可以是一个指令函数

```js
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()  // 页面加载完成之后自动让输入框获取到焦点的小功能
  }
})
```

局部注册通过在组件`options`选项中设置`directive`属性

```js
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus() // 页面加载完成之后自动让输入框获取到焦点的小功能
    }
  }
}
```

然后你可以在模板中任何元素上使用新的 `v-focus` property，如下：

```vue
<input v-focus />
```

自定义指令也像组件那样存在钩子函数：

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)
- `update`：所在组件的 `VNode` 更新时调用，但是可能发生在其子 `VNode` 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
- `componentUpdated`：指令所在组件的 `VNode` 及其子 `VNode` 全部更新后调用
- `unbind`：只调用一次，指令与元素解绑时调用

所有的钩子函数的参数都有以下：

- `el`：指令所绑定的元素，可以用来直接操作 `DOM`
- `binding`：一个对象，包含以下 `property`：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`
- `vnode`：`Vue` 编译生成的虚拟节点
- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用

> 除了 `el` 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 `dataset` 来进行





### 12、表单防止重复提交

表单防止重复提交这种情况设置一个`v-throttle`自定义指令来实现

```js
// 1.设置v-throttle自定义指令
Vue.directive('throttle', {
  bind: (el, binding) => {
    let throttleTime = binding.value; // 节流时间
    if (!throttleTime) { // 用户若不设置节流时间，则默认2s
      throttleTime = 2000;
    }
    let cbFun;
    el.addEventListener('click', event => {
      if (!cbFun) { // 第一次执行
        cbFun = setTimeout(() => {
          cbFun = null;
        }, throttleTime);
      } else {
        event && event.stopImmediatePropagation();
      }
    }, true);
  },
});
// 2.为button标签设置v-throttle自定义指令
<button @click="sayHello" v-throttle>提交</button>
```





### 13、图片懒加载

在`component`文件夹中新建`LazyLoad`文件夹，在文件夹里新建`index.js`。

设置一个`v-lazy`自定义指令完成图片懒加载

```js
const LazyLoad = {
    // install方法
    install(Vue,options){
    	  // 代替图片的loading图
        let defaultSrc = options.default;
        Vue.directive('lazy',{
            bind(el,binding){
                LazyLoad.init(el,binding.value,defaultSrc);
            },
            inserted(el){
                // 兼容处理
                if('IntersectionObserver' in window){
                    LazyLoad.observe(el);
                }else{
                    LazyLoad.listenerScroll(el);
                }
                
            },
        })
    },
    // 初始化
    init(el,val,def){
        // data-src 储存真实src
        el.setAttribute('data-src',val);
        // 设置src为loading图
        el.setAttribute('src',def);
    },
    // 利用IntersectionObserver监听el
    observe(el){
        let io = new IntersectionObserver(entries => {
            let realSrc = el.dataset.src;
            if(entries[0].isIntersecting){
                if(realSrc){
                    el.src = realSrc;
                    el.removeAttribute('data-src');
                }
            }
        });
        io.observe(el);
    },
    // 监听scroll事件
    listenerScroll(el){
        let handler = LazyLoad.throttle(LazyLoad.load,300);
        LazyLoad.load(el);
        window.addEventListener('scroll',() => {
            handler(el);
        });
    },
    // 加载真实图片
    load(el){
        let windowHeight = document.documentElement.clientHeight
        let elTop = el.getBoundingClientRect().top;
        let elBtm = el.getBoundingClientRect().bottom;
        let realSrc = el.dataset.src;
        if(elTop - windowHeight<0&&elBtm > 0){
            if(realSrc){
                el.src = realSrc;
                el.removeAttribute('data-src');
            }
        }
    },
    // 节流
    throttle(fn,delay){
        let timer; 
        let prevTime;
        return function(...args){
            let currTime = Date.now();
            let context = this;
            if(!prevTime) prevTime = currTime;
            clearTimeout(timer);
            
            if(currTime - prevTime > delay){
                prevTime = currTime;
                fn.apply(context,args);
                clearTimeout(timer);
                return;
            }

            timer = setTimeout(function(){
                prevTime = Date.now();
                timer = null;
                fn.apply(context,args);
            },delay);
        }
    }

}
export default LazyLoad;
```

在`main.js`里添加

```js
import LazyLoad from './components/LazyLoad';
Vue.use(LazyLoad,{
    default:'https://tva1.sinaimg.cn/large/007S8ZIlgy1gfyof9vr4mj3044032dfl.jpg'
});

```

在组件中使用

```js
<img v-lazy="https://tva1.sinaimg.cn/large/007S8ZIlgy1gfynwi1sejj30ij0nrdx0.jpg" />
```









### 14、一键 Copy的功能

1.首先建一个 js 文件（v-copy.js）。定义一个对象。（ 指令实际就是一个对象 ）

```js
import { Message } from 'ant-design-vue';

const vCopy = { //
  /*
    bind 钩子函数，第一次绑定时调用，可以在这里做初始化设置
    el: 作用的 dom 对象
    value: 传给指令的值，也就是我们要 copy 的值
  */
  bind(el, { value }) {
    el.$value = value; // 用一个全局属性来存传进来的值，因为这个值在别的钩子函数里还会用到
    el.handler = () => {
      if (!el.$value) {
      // 值为空的时候，给出提示，我这里的提示是用的 ant-design-vue 的提示，你们随意
        Message.warning('无复制内容');
        return;
      }
      // 动态创建 textarea 标签
      const textarea = document.createElement('textarea');
      // 将该 textarea 设为 readonly 防止 iOS 下自动唤起键盘，同时将 textarea 移出可视区域
      textarea.readOnly = 'readonly';
      textarea.style.position = 'absolute';
      textarea.style.left = '-9999px';
      // 将要 copy 的值赋给 textarea 标签的 value 属性
      textarea.value = el.$value;
      // 将 textarea 插入到 body 中
      document.body.appendChild(textarea);
      // 选中值并复制
      textarea.select();
      // textarea.setSelectionRange(0, textarea.value.length);
      const result = document.execCommand('Copy');
      if (result) {
        Message.success('复制成功');
      }
      document.body.removeChild(textarea);
    };
    // 绑定点击事件，就是所谓的一键 copy 啦
    el.addEventListener('click', el.handler);
  },
  // 当传进来的值更新的时候触发
  componentUpdated(el, { value }) {
    el.$value = value;
  },
  // 指令与元素解绑的时候，移除事件绑定
  unbind(el) {
    el.removeEventListener('click', el.handler);
  },
};

export default vCopy;
```

2.到这里，一键 Copy 的功能就实现了，最后也可以将自定义指令注册到全局：再新建一个 js （ directives.js ）文件来注册所有的全局指令。

```js
import copy from './v-copy';
// 自定义指令
const directives = {
  copy,
};
// 这种写法可以批量注册指令
export default {
  install(Vue) {
    Object.keys(directives).forEach((key) => {
      Vue.directive(key, directives[key]);
    });
  },
};
```

3.最后，在 main.js 中这样引入：

```js
import Vue from 'vue';
import Directives from './directives';
Vue.use(Directives);
```







### 15、开启GZip压缩

拆完包之后，我们再用`gzip`做一下压缩 安装`compression-webpack-plugin`

```sh
cnmp i compression-webpack-plugin -D
```

在`vue.congig.js`中引入并修改`webpack`配置

```js
const CompressionPlugin = require('compression-webpack-plugin')

configureWebpack: (config) => {
        if (process.env.NODE_ENV === 'production') {
            // 为生产环境修改配置...
            config.mode = 'production'
            return {
                plugins: [new CompressionPlugin({
                    test: /\.js$|\.html$|\.css/, //匹配文件名
                    threshold: 10240, //对超过10k的数据进行压缩
                    deleteOriginalAssets: false //是否删除原文件
                })]
            }
        }
```

在服务器我们也要做相应的配置 如果发送请求的浏览器支持`gzip`，就发送给它`gzip`格式的文件 我的服务器是用`express`框架搭建的 只要安装一下`compression`就能使用

```
const compression = require('compression')
app.use(compression())  // 在其他中间件使用之前调用
```

