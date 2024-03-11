# 一、项目插件

**(1): 如果是进行vue2 项目开发,则需要安装vue2 对应的插件 vetur**

**(2): 如果是进行vue3 项目开发,则需要安装vue3 对应的插件 volar**





# 二、引入组件库

### 1、引入Element-Plus组件库

[Element-Plus 官网地址](https://element-plus.gitee.io/zh-CN/)

(1)、安装依赖

```sh
npm install element-plus --save
```

(2)、自动按需导入

这种方式不需要导入任何组件，可以直接使用

```sh
npm install -D unplugin-vue-components unplugin-auto-import
```

(3)、引入css样式

```js
// main.is
import 'element-plus/dist/index.css'
```

(4)、配置文件配置

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

(5)、引入Icon 图标

[官网](https://element-plus.gitee.io/zh-CN/component/icon.html#%E6%B3%A8%E5%86%8C%E6%89%80%E6%9C%89%E5%9B%BE%E6%A0%87)

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

### 1、依赖安装

```sh
npm install vue-router@4 -S
```

### 2、创建文件目录 

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



### 3、创建notfound页面

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

### 4、配置404页面的路由展示

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



### 5、配置路由前置守卫

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



### 6、入口文件导入

```js
// main.js
// ...
import router from "./router/index"
createApp(App).use(router).use(...).mount('#app')
```

# 三、配置src路径别名

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





# 四、axios数据请求

### 1、安装axios

```sh
npm install axios -S
```

### 2、axios简单封装

(1).src下创建utils/request.js

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



(2).src下创建aip/login.js

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





### 3、设置反向代理解决跨域

[查看文档](https://cn.vitejs.dev/config/server-options.html)

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





# 五、引入状态管理库

### 1、引入vuex

**(1). 依赖安装**

```sh
npm install vuex@next --save
```

**(2). 创建文件目录**

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

**(3). 入口文件导入**

```js
// main.js
import store from './store/index'

createApp(App).use(router).use(store).mount('#app')
```



**(4). 实现vuex数据持久化**

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







### 2、引入pinia

**(1). 依赖安装**

```sh
npm install pinia
```

**(2). 创建文件目录**

src/store/index.js

```js
// 定义store 仓库
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

**(3). 在其他文件使用仓库信息**

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



**(4). 实现pinia 数据持久化**

第一步：在项目中安装插件

```sh
npm install pinia-persistedstate-plugin
```

第二步：在 入口文件main.ts下, 配置store 的数据的持久化

```js
import { createPinia } from "pinia";
// 导入pinia持久化插件
import { createPersistedState } from "pinia-persistedstate-plugin";
// 配置持久化
const store = createPinia();
store.use(createPersistedState());
app.use(store)
```





# 六、引入css样式库

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

[官网](https://cn.windicss.org/guide/)

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







### 3、Animate.css 动画库

[文档](https://animate.style/#migration)

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







# 七、自定义组件

### 1、数字滚动

[文档](https://www.npmjs.com/package/gsap)

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











# 八、自定义指令

### 1、自动获取焦点

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





### 2、表单防止重复提交

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



### 3、图片懒加载

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









### 4、一键 Copy的功能

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





### 5、各种popover错位的问题

解决代码修改了网页zoom导致各种popover错位的问题（适配vue2+elementUI）

```js
export default {
  install(Vue) {

    const delegates = {
      'el-select': (component, element) => {
        return [component.$refs.reference.$el]
      },
      'el-pagination': (component, element) => {
        const sizesComponent = component.$children.find((child) => child.$options._componentTag === 'sizes');
        const eles_ElSelectInSizes = delegates['el-select'](sizesComponent.$children[0], element);
        return eles_ElSelectInSizes;
      }
    }

    const getRefEles = (component, element) => {
      const tagName = component.$options._componentTag;
      const delegate = delegates[tagName];
      console.log(`TagName for fix-zoom : ${ tagName }`);
      return delegate ? delegate(component, element) : [element]; // 默认返回当前元素
    }

    Vue.directive('fixZoom', {
      inserted: function(element, binding, vnode) {
        const refEles = getRefEles(vnode.componentInstance, element);
        if (!refEles) {
          return;
        }
        refEles.forEach((refEle) => {
          refEle.getBoundingClientRectBak = refEle.getBoundingClientRect;
          refEle.getBoundingClientRect = function () {
            const rect = refEle.getBoundingClientRectBak()

            var zoom = window.getComputedStyle(document.body).zoom
            var scrollTop = document.documentElement.scrollTop;
            var scrollLeft = document.documentElement.scrollLeft;
            var offsetScrollTop = scrollTop - (scrollTop / zoom);
            var offsetScrollLeft = scrollLeft - (scrollLeft / zoom);
            
            return new DOMRect(rect.x - offsetScrollLeft, rect.y - offsetScrollTop, rect.width, rect.height);
          }
        })
      }
    })
  }
};
```

```js
import fixZoom from'@/components/fix-zoom.js'
Vue.use(fixZoom)
```



# 九、自定义函数方法

### 1、自定义验证规则

创建utils/rules.ts文件

```js
// 手机号验证
export const phoneRule = (value: string) => {
	return value && /^1[3-9][0-9]{9}$/.test(value);
};
// 固定电话
export function checkTel (tel) {
   return /^((d{3,4})|d{3,4}-|s)?d{5,14}$/.test(tel)
}
// 邮箱验证
export const emailRule = (value: string) => {
	return value && /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/.test(value);
};
// 数字验证
export const numberRule = (value: string) => {
	return value && /^[-]?\d+$/.test(value);
};
// 英文验证
export const englishRule = (value: string) => {
	return value && /^[a-zA-Z]+$/.test(value);
}
// 网址格式验证
export const websiteRule = (value: string) => {
	return value && /(http|ftp|https):\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&:/~\+#]*[\w\-\@?^=%&/~\+#])?/.test(value)
}
// 特殊字符验证
export const specialRule = (value: string) => {
	return value && /[`~!@#$%^&*()_+<>?？\\:"{},.\/;'[\]]+/.test(value)
}
```







### 2、键盘监听

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





### 3、全局loading进度条实现

[文档](https://www.npmjs.com/package/nprogress)

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





### 4、设置、获取、删除cookie

- [ ] **方式一：引入vueuse的Cookie存储数据**

[官网](https://vueuse.org/integrations/useCookies/#usecookies)

**(1)、下载依赖：**

```sh
npm i @vueuse/integrations
```

```sh
npm i universal-cookie
```

**(2)、定义js文件简单封装**

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



- [ ] **方式二：安装vue-cookies插件**

**(1)、安装插件：** 你可以使用 `vue-cookies` 这样的插件来方便地操作 Cookie。首先，安装该插件：

```sh
npm install vue-cookies
```

**(2)、在 Vue 项目中使用：** 在你的 Vue 项目中，你需要引入并配置 `vue-cookies` 插件。在你的入口文件（例如 `main.js`）中进行配置：

```js
// main.js
import Vue from 'vue';
import VueCookies from 'vue-cookies';

Vue.use(VueCookies);

// 设置全局的 Cookie 选项，例如过期时间等
Vue.$cookies.config('7d'); // 设置 Cookie 的过期时间为 7 天
```

**(3)、使用 Cookie 存储和获取数据：** 一旦配置完毕，你就可以在组件中使用 `this.$cookies` 来进行 Cookie 数据的存储和获取。

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



- [ ] **方式三：document**

**(1)、设置cookie,设置max-age 属性指定cookie 的有效期（秒）**

```js
/**
 * 设置 cookie。
 *
 * @param {string} name - cookie 名称
 * @param {string} value - cookie 值
 * @param {number} [expiretime] - cookie 过期时间（以秒为单位）
 * @returns {void}
 */
function setCookie(name, value, expiretime) {
    // 编码 cookie 值，并设置默认的 path
    let cookie = `${name}=${encodeURIComponent(value)}; path=/`;
    // 如果提供了过期时间，添加 max-age 属性
    if (typeof expiretime === 'number') {
        cookie += `; max-age=${expiretime}`;
    }
    // 将 cookie 设置到 document.cookie
    document.cookie = cookie;
}
```

举个栗子 → 🙌🌰：

```js
setCookie('id',1,1)
document.cookie //"id=1"
```

------

**(2)、读取cookie,将设置的cookie值拿到单个key 对应的值**

```js
/**
 * 从 cookie 中读取指定名称的值。
 *
 * @param {string} name - 要读取的 cookie 名称
 * @returns {string|null} 指定名称的 cookie 值，如果未找到则返回 null
 */
function getCookie(name) {
    let cookie = document.cookie;
    let arrCookie = cookie.split('; ');
    // 遍历 cookie 数组，查找指定名称的 cookie 值
    for (let i = 0; i < arrCookie.length; i++) {
        let arr = arrCookie[i].split('=');
        if (arr[0] === name) {
            return arr[1]; // 返回指定名称的 cookie 值
        }
    }
    // 如果未找到指定名称的 cookie，返回 null
    return null;
}
```

举个栗子 → 🙌🌰

```js
getCookie('id') // 1
```

------

**(3)、删除对应设置的cookie**

max-age为0时，删除cookie

```js
/**
 * 删除指定名称的 cookie。
 *
 * @param {string} name - 要删除的 cookie 名称
 * @returns {void}
 */
function deleteCookie(name) {
    let currentCookie = getCookie(name);
    // 如果找到指定名称的 cookie，设置其 max-age 为 0 以删除
    if (currentCookie) {
        document.cookie = `${name}=${currentCookie}; max-age=0; path=/`;
    }
}
```

举个栗子 → 🙌🌰

```js
deleteCookie('id')
document.cookie // ''
```









### 5、防抖节流函数的应用

**(1)、防抖函数的应用**

在一定的时间内，多次执行同一个函数，只会触发一次

```js
/**
 * 创建一个 debounce（防抖）函数，用于延迟执行目标函数。
 *
 * @param {Function} fn - 目标函数
 * @param {number} delay - 延迟时间（以毫秒为单位）
 * @returns {Function} 包装后的防抖函数
 */
function debounce(fn, delay) {
    let timer = null;
    /**
     * 包装后的防抖函数，用于延迟执行目标函数。
     *
     * @returns {void}
     */
    return function () {
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(fn, delay);
    };
}
```

------

**(2)、节流函数的应用**

在一定时间内，多次执行同一个函数，只有第一次执行才会触发。

```js
/**
 * 创建一个 throttle（节流）函数，用于限制目标函数的调用频率。
 *
 * @param {Function} fn - 目标函数
 * @param {number} delay - 两次调用之间的最小时间间隔（以毫秒为单位）
 * @returns {Function} 包装后的节流函数
 */
function throttle(fn, delay) {
    let flag = true;
    /**
     * 包装后的节流函数，用于限制目标函数的调用频率。
     *
     * @returns {void}
     */
    return function () {
        if (!flag) {
            return false;
        }
        flag = false;
        setTimeout(() => {
            fn();
            flag = true;
        }, delay);
    };
}
```

举个栗子 → 🙌🌰
场景：以一个输入框为例，监听鼠标弹起事件，在1s时间内， 输出时间戳，多次输入，只会执行一次。

```js
let ele = document.getElementsByTagName('input')[0];
ele.addEventListener('keyup',throttle(()=>{
    console.log(Date.now());
},1000));
```





### 6、Url参数与对象相互转换并取值

**(1)、将Url参数转换成对象没有参数时返回空对象**

```js
/**
 * 从当前页面 URL 中获取查询参数，并返回一个包含参数键值对的对象。
 *
 * @returns {Object} 包含查询参数的对象
 */
function getQueryObject() {
    // 获取 URL 中的查询参数部分，兼容 hash 模式
    let search = window.location.search.substr(1) || window.location.hash.split('?')[1],
        obj = {};
    // 如果没有查询参数，返回空对象
    if (!search) return obj;
    // 将查询参数字符串分割为数组
    let paramsArr = search.split('&');
    // 遍历数组，将参数添加到对象中
    for (let i of paramsArr) {
        let arr = i.split('=');
        obj[arr[0]] = arr[1]; // 设置对象的键值对
    }
    // 返回包含查询参数的对象
    return obj;
}
```

举个栗子 → 🙌🌰：www.baidu.com?id=1&type=2

```js
getQueryObject() // {id: "1", type: "2"}
```

------



**(2)、将对象转换成Url需要的参数 tag标记是否带问号(?)**

```js
/**
 * 将对象格式化为 URL 查询参数字符串。
 *
 * @param {Object} obj - 包含键值对的对象
 * @param {boolean} [tag=true] - 是否在返回的字符串中包含问号 (?)
 * @returns {string} 格式化后的 URL 查询参数字符串
 */
function formatObjToParamStr(obj, tag = true) {
    let data = [],
        dStr = '';
    for (let key in obj) {
        data.push(`${key}=${obj[key]}`);
    }
    dStr = tag ? '?' + data.join('&') : data.join('&');
    return dStr;
}
```

举个栗子 → 🙌🌰：

```js
formatObjToParamStr({id:1,type:2}) // "?id=1&type=2"
formatObjToParamStr({id:1,type:2},false) // "id=1&type=2"
```

------



**(3)、通过参数名获取url中的参数值****

```js
/**
 * 从指定 URL 中获取指定名称的查询参数的值。
 *
 * @param {string} name - 要获取的查询参数的名称
 * @param {string} [url=window.location.href] - 要解析的 URL 字符串（默认为当前页面 URL）
 * @returns {string} 查询参数的值，如果没有找到则返回空字符串
 */
function getUrlParam(name, url) {
    // 如果未提供 url 参数，则使用当前页面的 URL
    url = url || window.location.href;
    // 从 URL 中解析查询参数部分
    let search = url.includes('?') ? url.split('?')[1] : url.split('#')[1];
    // 如果没有查询参数，返回空字符串
    if (!search) return '';
    // 将查询参数字符串分割为数组
    let paramsArr = search.split('&');

    // 遍历数组，查找指定名称的查询参数
    for (let i of paramsArr) {
        let arr = i.split('=');
        if (arr[0] === name) {
            return arr[1]; // 返回查询参数的值
        }
    }
    // 如果没有找到指定名称的查询参数，返回空字符串
    return '';
}
```

举个栗子 → 🙌🌰：www.baidu.com?id=1&type=2

```js
getUrlParam('id','www.baidu.com?id=1&type=2') // 1
```



### 7、检查数据类型

**(1)、检查数据类型是否是数组**

```js
export function isArray (val) {
   return Object.prototype.toString.call(val) === '[object Array]';
}
```

举个栗子 → 🙌🌰

```js
isArray([]) // true
isArray({}) // false
```

------



**(2)、检查数据类型是否是对象**

```js
export function isObject(val) {
   return Object.prototype.toString.call(val) === '[object Object]';
}
```

举个栗子 → 🙌🌰

```js
isObject([]) // false
isObject({}) // true
```

------



**(3)、检查数据类型是否是数值**

```js
function isNumber(val) {
   return Object.prototype.toString.call(val) === '[object Number]';
}
```

举个栗子 → 🙌🌰

```js
isNumber(12) // true
isNumber({}) // false
```





### 8、检测对象是否含有某个属性

```js
/**
 * 检查对象是否包含指定属性。
 *
 * @param {Object} obj - 要检查的对象
 * @param {string} key - 要检查的属性名
 * @returns {boolean} 如果对象包含指定属性，返回 true；否则返回 false
 */
function checkObjHasAtrr(obj, key) {
    return Object.prototype.hasOwnProperty.call(obj, key);
}
```

举个栗子 → 🙌🌰

```js
checkObjHasAtrr({id: 1, type: 2}, 'id') // true
```





### 9、检测数组最大最小值

**(1)、数组最大值**

```js
function max (arr) {
   if (!isArray(arr) && arr.length) return;
   return Math.max.apply(null,arr);
}
```

举个栗子 → 🙌🌰

```js
max([1,2,3,4,5,6])  // 6
```

------

**(2)、数组最大值**

```js
export function min(arr) {
   if (!isArray(arr) && arr.length) return;
   return Math.min.apply(null, arr);
}
```

举个栗子 → 🙌🌰

```js
min([1,2,3,4,5,6])  // 1
```





### 10、生成随机范围的随机数

说明： Math.floor：下取整
			Math.random：生成0~1 的随机数

```js
/**
 * 生成指定范围内的随机整数（包括边界值）。
 *
 * @param {number} min - 随机数的最小值（包括）
 * @param {number} max - 随机数的最大值（包括）
 * @returns {number} 生成的随机整数
 */
export function getRandom(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

举个栗子 → 🙌🌰

```js
getRandom(1,2) // 1 随机生成1,2
```





### 11、去除字符串空

**去除首尾空格**

```js
export function trim(str) {
    return str.replace(/(^\s*)|(\s*$)/g, '');
}
```

**去除字符串所有空格**

```js
export function trimAll(str) {
    return str.replace(/(\s+)/g, ''); 
}
```

举个栗子 → 🙌🌰

```js
trim(' web api ') // 'web api'
trimAll(' web api ') // 'webapi'
```





### 12、删除数组中的某个元素

```js
export function removeArr(arr, val) {
    let index = arr.indexOf(val);
    if (index > -1) arr.splice(index, 1);
    return arr;
}
```

举个栗子 → 🙌🌰

```js
removeArr([1,2,3,4,5,6,7,8],4) //  [1, 2, 3, 5, 6, 7, 8]
```





### 13、数组去重

```js
export function uniqueArr(arr) {
    return Array.from(new Set(arr));
}
```

举个栗子 → 🙌🌰

```js
uniqueArr([1, 2, 1, 3]) //[1, 2, 3]
```





### 14、获取验证码倒计时

```js
function getCode(time) {
    let setInter = null,
        codeText = '';
    setInter = setInterval(() => {
        if (time < 1) {
            clearInterval(setInter);
            codeText = '获取验证码';
        } else {
            codeText = `已发送${ time }s`;
            time--;
        }       
    }, 1000);	
}
```

举个栗子 → 🙌🌰

```js
getCode(5)
```







### 15、将手机号码4-7位转换成 *

```js
function replaceMobile(mobile) {
    return Number.prototype.toString.call(mobile).replace(/1(\d{2})\d{4}(\d{4})/g,'1$1****$2');
}
```

举个栗子 → 🙌🌰

```js
replaceMobile(18000009999) //"180****9999"
```





### 16、值是否存在于数组

用于页面判断页面active状态

```js
/**
 * 检查数组中是否存在指定元素
 * @param {Array} list - 要检查的数组
 * @param {*} key - 要检查的元素
 * @returns {boolean} - 如果数组中存在指定元素，返回 true；否则返回 false
 */
export function isExistInArray(list, key) {
  return list.indexOf(key) > -1;
}

```



### 17、截取数组

```js
/**
 * 获取数组的前几个元素
 * @param {Array} list - 要切片的数组
 * @param {number} number - 要获取的元素个数
 * @returns {Array} - 返回包含前几个元素的新数组
 */
export function sliceArray(list, number) {
  return list.slice(0, number);
}
```





### 18、处理浮点数

```js
/**
 * 对数字进行四舍五入
 * @param {number} num - 要进行四舍五入的数字
 * @param {number} decimalPlaces - 要保留的小数位数，默认为 2
 * @returns {number} - 返回四舍五入后的数字
 */
export function roundNumber(num, decimalPlaces = 2) {
  const factor = Math.pow(10, decimalPlaces);
  return Math.round(num * factor) / factor;
}
```



### 19、大额数字转换

```js
export const numberFormat = (val: number | string) : string | number => {
	const num = (val as any) * 1;
	if (num > 10000) {
		let sizesValue = '';
		if (num > 10000 && num < 99999999) {
			sizesValue = '万';
		} else if (num > 100000000) {
			sizesValue = '亿';
		}
		const i = Math.floor(Math.log(num) / Math.log(10000));
		return `${(num / Math.pow(10000, i)).toFixed(1)}${sizesValue}`;
	}
	return turnThousandth(`${val}`);
};
```





### 20、download二进制文件

```js
export const downloadBlob = (res: any, name?: string) => {
	const blob = new Blob([res], {
		type: 'text/plain;charset=utf-8',
	});
	const downloadElement = document.createElement('a'); //创建一个a 虚拟标签
	const href = window.URL.createObjectURL(blob); // 创建下载的链接
	downloadElement.href = href;
	downloadElement.download = decodeURI(name?.replace(/%/g, '%25') ?? ''); // 下载后文件名
	document.body.appendChild(downloadElement);
	downloadElement.click(); // 点击下载
	document.body.removeChild(downloadElement); // 下载完成移除元素
	window.URL.revokeObjectURL(href);
};
```



### 21、点击复制文本

```js
import useClipboard from 'vue-clipboard3';
const { toClipboard } = useClipboard();
// 点击复制文本
const copyText = (text: string) => {
    return new Promise((resolve, reject) => {
        try {
            //复制
            toClipboard(text);
            //下面可以设置复制成功的提示框等操作
            ElMessage.success('复制成功');
            resolve(text);
        } catch (e) {
            //复制失败
            ElMessage.error(t('复制失败'));
            reject(e);
        }
    });
};
```





### 22、统计函数

```js
/**
* 对数字进行四舍五入
* @param {Array} array - 要进行统计的数组
* @param {Function} generateKey - 回调传入key
*/
function countBy(array, generateKey) {
    const result = {};
    // 遍历数组
    for (const u of array) {
        // 回调获取key
        const key = generateKey(u);
        if (result[key]) {
            result[key]++;
    	} else {
    		result[key] = 1;
    	}
    }
    return result;
}
```

```js
const arr = [
            { name: '小明', sex: '男', age: 22 },
            { name: '小明', sex: '女', age: 12 },
            { name: '小明', sex: '男', age: 19 },
            { name: '小明11', sex: '女', age: 18 }]

        console.log(countBy(arr, (u) => u.sex));//{男: 2, 女: 2}
        console.log(countBy(arr, u => u.name.length))// {2: 3, 4: 1}
        console.log(countBy(arr, u => u.age > 18 ? '成年' : '未成年'));// {成年: 2, 未成年: 2}
```



# 十、其他相关

### 1、根据屏幕进行响应变化

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





### 2、开启GZip压缩

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





### 3、全屏显示

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



### 4、图片懒加载配置

安装依赖

```vue
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





### 5、滚动条css设置

```css
::-webkit-scrollbar 滚动条整体部分
::-webkit-scrollbar-thumb 滚动条里面的小方块，能向上向下移动
::-webkit-scrollbar-track 滚动条的轨道（里面装有 Thumb
::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去
::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件注意此方案有兼容性问题，一般需要隐藏滚动条时我都是用一个色块通过定位盖上去，或者将子级元素调大，父级元素使用 overflow-hidden 截掉滚动条部分。暴力且直接。
```





