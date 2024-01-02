项目基础搭建流程

**项目效果线上预览地址:**  http://47.94.148.165/

**接口文档:**

项目接口地址: http://47.94.148.165:3001/admindoc

**项目的baseUrl 地址:**   http://47.94.148.165/admin 



## 一、项目创建

### 1. vue创建项目

```sh
npm i vue@latest
```

### 2. 安装依赖

```sh
cd 项目目录

npm install  // 安装项目依赖
```

### 3. 运行项目查看

```sh
npm run dev
```



## 二、创建api层

### 1. 安装axios

```sh
npm install axios -S
```

### 2. axios封装

##### 3.1 utils/request.js

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

##### 

##### 3.3 utils/login.js

定义登录接口

```js
// 该文件用来定义所有的接口地址
//01: 引入axios
import instance from "./request";

// 定义登录接口
export function loginApi(data) {
    // data 就是参数
    return instance({
        method: 'post',
        url: '/admin/login',
        data: data,
        // headers: {'X-Requested-With': 'XMLHttpRequest'},
    })
}
```



## 三、引入vue-router@4

### 1. 依赖安装

```sh
npm install vue-router@4 -S
```

### 2. 创建文件目录 src/router/index.js(ts)

```js
import { createRouter, createWebHashHistory} from "vue-router";
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      redirect: '/login'
    },
    {
      path: '/login',
      component: () => import('@/views/Login.vue'),
    },
    {
      path: '/home',
      component: () => import('@/views/Home.vue')
    }
  ]
})

export default router
export default router
```

### 3. 入口文件导入

```js
// main.js
// ...

import router from "./router/index"

createApp(App).use(router).use(...).mount('#app')
```



## 四、引入vuex

### 1. 依赖安装

```sh
npm install vuex@4 -S
```

### 2. 创建文件目录src/store/index.js

```js
// 定义store 仓库

import { createStore } from 'vuex';
import createpersistedstate from 'vuex-persistedstate';
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
        createpersistedstate()
    ]
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

安装插件

```sh
npm i vuex-persistedstate -S
```

修改`store/index.js`

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
        createpersistedstate()
    ]
})
export default store
```





## 五、 引入组件库element-plus

[element-plugs 官网地址](https://element-plus.gitee.io/zh-CN/)

### 1. 依赖安装

```sh
npm install element-plus --save
```

### 2. 自动按需导入

这种方式不需要导入任何组件，可以直接使用

```sh
npm install -D unplugin-vue-components unplugin-auto-import
```

### 3. 配置文件配置

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



## 六、项目样式重置

### 1. 依赖安装(第三方的重置样式包)

```sh
npm install normalize.css -S
```

### 2. 入口文件引入

```js
// main.js
import 'normalize.css'
```



## 七: vscode 工具插件的安装

#### 1: 如果是进行vue2 项目开发,则需要安装vue2 对应的插件 vetur

#### 2: 如果是进行vue3 项目开发,则需要安装vue3 对应的插件 volar



## 八: 项目页面开发

### 01: 登录页布局, 创建login.vue 文件;

注意:使用scss 语法需要手动安装sass   npm i sass --save;方可使用scss 预编译语法

```vue
// src/views/Login.vue

<template>
  <div class="login">
    <div class="loginbox">
      <h3>嗨购后台系统-2220</h3>
      <el-form
        ref="ruleFormRef"
        :model="ruleForm"
        status-icon
        :rules="rules"
        label-width="120px"
        class="demo-ruleForm"
      >
        <el-form-item prop="adminname">
          <el-input v-model="ruleForm.adminname" type="adminname" autocomplete="off" />
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="ruleForm.password" type="password" autocomplete="off" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm(ruleFormRef)">登录</el-button>
          <el-button @click="resetForm(ruleFormRef)">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
// 导入 登录接口
import { loginApi } from '@/utils/login'
// console.log('loginApi', loginApi)

// 使用映射方法将store仓库上的数据和方法映射到当前组件的实例上
import { mapMutations } from 'vuex'
export default {
  name: '',
  data() {
    return {
      ruleForm: {
        // 表单的用户名和密码
        adminname: 'admin',
        password: '123456'
      },
      ruleFormRef: null, // 表单组件实例对象
      rules: {
        //验证规则
        adminname: [{ validator: this.validateAdminname, trigger: 'blur' }],
        password: [{ validator: this.validatePwd, trigger: 'blur' }]
      }
    }
  },
  methods: {
    ...mapMutations(['setuserinfoFn']),
    // 登录事件
    submitForm(formEl) {
      // console.log('ruleFormRef', formEl)
      // 使用 formEl组件实例对象validate方法实现一个表单验证功能
      if (!formEl) return
      formEl.validate((valid) => {
        if (valid) {
          // 验证通过
          //01: 调用登录接口, 实现登录功能
          // console.log(this.ruleForm)
          loginApi(this.ruleForm).then((res) => {
            // console.log(res)
            if (res.data.code == 200) {
              //02: 已经将登录提示添加在响应拦截器中统一处理
              //03: 将后端返回的用户信息存到本地可以vuex 中
              this.setuserinfoFn(res.data.data)
              //04: 跳转页面到首页
              this.$router.push('/home')
            }
          })
        } else {
          // 验证不通过
          console.log('验证不通过')
          return false
        }
      })
    },
    // 重置事件
    resetForm(formEl) {
      if (!formEl) return
      formEl.resetFields()
    },
    //验证账户的函数
    validateAdminname(rule, value, callback) {
      if (/^\w{2,6}$/.test(value)) {
        // 验证通过
        callback()
      } else {
        callback(new Error('账号有误,请重新输入!!!'))
      }
    },
    //验证密码的函数
    validatePwd(rule, value, callback) {
      if (/^\d{6}$/.test(value)) {
        // 验证通过
        callback()
      } else {
        callback(new Error('密码有误,请重新输入!!!'))
      }
    }
  },
  mounted() {
    // 给表单组件实例对象赋值为表单组件实例对象
    this.ruleFormRef = this.$refs.ruleFormRef
    //console.log(this.$refs.ruleFormRef)
  }
}
</script>
<style lang='scss' scoped >
.login {
  width: 100%;
  height: 100%;
  background-image: url('@/assets/imgs/bg.jpeg');
  background-size: 100% 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  .loginbox {
    width: 400px;
    padding: 20px 0 20px 0;
    box-sizing: border-box;
    background: white;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    .el-form {
      width: 80%;
      margin: 0 auto;
    }
    /* 
       样式穿透,用来修改某一些ui 组件默认自带的样式,提高权重
     */
    ::v-deep .el-form-item__content {
      margin: 0 !important;
    }
  }
}
</style>
```

在router中的index.js 文件中添加路由守卫

```js
// 设置路由全局守卫, 对登录状态做校验
router.beforeEach((to, from) => {
  // 01: 获取登录存的本地信息
  let userinfo = JSON.parse(localStorage.getItem('vuex'));
  console.log('userinfo', userinfo);
  if (to.path == '/login') {
    // 当有本次登录后的存储信息 token存在,说明已登录, 然后跳转到首页
    if (userinfo) {
      // 说明登录过了
      // 提示已登录, 
      ElMessage({
        message: '已登录,无需重复登录',
        type: 'warning',
        duration: 1000,
      })
      //然后重定向到首页
      return { path: '/home' }
    }
    // 如果有userinfo 不用写return, 因为不写默认表示放行,允许正常跳转
  } else {
    // 当本地有token信息, 说明已经登录, 就可以正常页面,否则提示请先登录, 跳转到登录页
    if (!userinfo) {
      ElMessage({
        message: '请先登录',
        type: 'warning',
        duration: 1000,
      })
      //然后重定向到登录页
      return { path: '/login' }
    }
  }

})
```



### 02: 首页布局及功能

在views目录下,创建HomeView.vue,作为首页

```vue
<template>
    <el-container>
        <el-aside :width="asideWidth">
            <!-- 设置导航图标 -->
            <div class="logo">
                <el-image style="width: 32px; height: 32px" :src="logoImg" />
                <p v-show="!isCollapse">千锋管理后台</p>
            </div>
            <Aside :isCollapse="isCollapse" />
        </el-aside>
        <el-container>
            <el-header>
                <!-- header 头部左边 -->
                <el-icon @click="isCollapse=!isCollapse" :size="22">
                    <!-- 使用动态组件实现切换-->
                    <component :is="headLeft"></component>
                </el-icon>
                <!-- header头部右边头像信息 -->
                <div class="avatar-box">
                    <el-space :size="20">
                        <span>欢迎您:{{userinfo.adminname}}</span>
                        
                        <el-dropdown trigger="click">
                            <span class="el-dropdown-link">
                                <el-avatar shape="square" :size="30" :src="squareUrl" />
                            </span>
                            <template #dropdown>
                                <el-dropdown-menu>
                                    <el-dropdown-item :icon="Plus" @click="$router.push('/set')">设置
                                    </el-dropdown-item>
                                    <el-dropdown-item :icon="CirclePlusFilled" @click="loginOut">
                                        退出
                                    </el-dropdown-item>
                                </el-dropdown-menu>
                            </template>
                        </el-dropdown>
                    </el-space>
                </div>
            </el-header>
            <el-main>Main</el-main>
        </el-container>
    </el-container>
</template>

<script>
// 引入图标组件
import { Fold, Expand } from '@element-plus/icons-vue';
// 引入侧边栏组件
import Aside from '@/components/AsideCom.vue';
// 引入图片路径
import logoImg from '@/assets/img/logo.jpeg';
// 引入映射函数
import { mapState, mapMutations } from 'vuex'

export default {
    name: '',
    data() {
        return {
            isCollapse: true, // 设置是否折叠 表示默认折叠
            logoImg: logoImg, // 引入的logo
            squareUrl: 'https://cube.elemecdn.com/9/c2/f0ee8a3c7c9638a54940382568c9dpng.png'
        };
    },
    components: {
        Fold,
        Expand,
        Aside
    },
    computed: {
        asideWidth() {// 左边菜单栏的宽度
            return !this.isCollapse ? '200px' : '64px'
        },
        headLeft() { // 折叠左侧菜单栏使用的折叠图标
            return !this.isCollapse ? Fold : Expand
        },
        ...mapState(['userinfo'])
    },
    methods: {
        ...mapMutations(['updateUserinfo']),
        loginOut() { // 退出登录
            //01: 清除本地的缓存
            localStorage.removeItem('token')
            //02: 清空store 仓库中的数据
            this.updateUserinfo({})
             //03:跳转到登录页
            this.$router.push('/login')
        }
    }
}

</script>

<style lang="scss" scoped>
.el-container {
    height: 100%;
    background-color: #dfe6e9;

    .el-aside {
        background-color: #2d3436;
        transition: 0.3s linear;
        border-right: none;

        .logo {
            display: flex;
            font-size: 16px;
            height: 56px;
            padding-left: 20px;
            box-sizing: border-box;
            align-items: center;
            width: 100%;

            .el-image {
                margin-right: 10px;
            }

            p {
                transition: 0.3s;
                color: white;
            }
        }
    }

    .el-sub-menu__title {
        color: white !important;
    }

    .el-header {
        background-color: #7f8fa6;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .avatar-box {
        display: flex;
        align-items: center;
    }
}
</style>
```



### 03: 创建Aside组件

在src 下的components 文件夹中创建AsideCom 组件;

```vue
<template>
    <el-menu :default-active="currentPath" :router="true" background-color="#2d3436" text-color="white"
        class="el-menu-vertical-demo" :collapse="isCollapse">
        <el-menu-item index="/home">
            首页
        </el-menu-item>
        <el-sub-menu :index="`/home/${item.path}`" v-for="item in userinfo.checkedkeys">
            <template #title>
                <el-icon>
                    <location />
                </el-icon>
                <span>{{ item.label }}</span>
            </template>
            <el-menu-item :index="`/home/${item.path}/${child.path}`" v-for="child in item.children">
                {{ child.label }}</el-menu-item>
        </el-sub-menu>
    </el-menu>
</template>
<script>
// 引入映射函数
import { mapState } from 'vuex';
// 引入图标组件
import {
    Document,
    Menu as IconMenu,
    Location,
    Setting,
} from '@element-plus/icons-vue';

export default {
    name: '',
    data() {
        return {
        };
    },
    // 在非setup语法糖中引入的图标组件还需要注册,setup语法糖引入就可以使用图标组件
    components: {
        Document,
        IconMenu,
        Location,
        Setting,
    },
    props: {
        isCollapse: { // 接收父传子属性  是否折叠左侧菜单栏
            type: Boolean,
            required: true
        }
    },
    computed: {
        ...mapState(['userinfo', 'currentPath'])
    }
}
</script>
<style lang="scss" scoped>
.el-menu {
    width: 100%;
    border-right: none;
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 200px;
    min-height: 400px;
}
</style>
```



### 04:  创建管理员列表页面组件

在 views/manage/ 创建 manageListView.vue 组件.用于展示用户列表及页面相关的功能

01.配置对应的路由表

```js
export const routes = [
    { path: '/', redirect: '/home' },
    {
        path: '/home',
        component: Home,
        label: '首页',
        name: () => import('@/views/manage/manageIndex.vue'),
        children: [
            {
                path: 'manage',
                name: "manage",
                label: '账户管理',
                component: () => import('@/views/manage/manageIndex.vue'),
                children: [
                    {
                        path: 'managelist',
                        name: "managelist",
                        label: '管理员列表',
                        component: () => import('@/views/manage/manageListView.vue')
                    }
                ]
            }
        ]
    },
    { path: '/login', component: loginView, name: 'login' },

];
```

02: 如下为 manageListView.vue 组件

```vue
<template>
    <div>
        <!-- 头部 -->
        <div class="header">
            <el-button type="primary" @click="addadminFn">
                添加管理员<el-icon>
                    <Plus />
                </el-icon>
            </el-button>
        </div>
        <!-- 表格数据 -->
        <el-table :data="computedTableData" style="width: 100%" :cell-style="{height:'60px'}">
            <el-table-column type="index" label="序号" width="80" align="center" />
            <el-table-column prop="adminname" label="姓名" align="center" />
            <el-table-column prop="role" label="权限" align="center">
                <!-- <el-tag>Tag 1</el-tag> -->
                <template #default="scope">
                    <!-- {{scope}} -->
                    <el-tag :type="scope.row.role === 1 ? '' : 'success'" disable-transitions>{{
                    scope.row.role==1?"管理员":"普通用户"
                    }}
                    </el-tag>
                </template>
            </el-table-column>
            <el-table-column label="操作" align="center">
                <template #default="scope">
                    <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除
                    </el-button>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination background layout="total,prev, pager, next" v-model:current-page="currentPage"
            :total="tableData.length" />

        <!-- 抽屉组件 -->
        <el-drawer v-model="isshowdrawer" title="添加管理员" direction="rtl">
            <template #default>
                <!-- 主体部分 -->
                <el-form :label-position="'right'" label-width="80px" :model="formLabelAlign" style="max-width: 460px">
                    <el-form-item label="账号">
                        <el-input v-model="formLabelAlign.adminname" />
                    </el-form-item>
                    <el-form-item label="密码">
                        <el-input v-model="formLabelAlign.password" />
                    </el-form-item>
                    <el-form-item label="角色">
                        <el-select v-model="formLabelAlign.role" placeholder="请选择角色">
                            <el-option label="管理员" value="1" />
                            <el-option label="超级管理员" value="2" />
                        </el-select>
                    </el-form-item>
                    <el-form-item label="权限">
                        <!-- 树形菜单 -->
                        <el-tree ref="treeRef" :data="treeData" show-checkbox node-key="path" default-expand-all
                            :default-checked-keys="defaultCheckedKeys" :props="defaultProps" />
                    </el-form-item>
                </el-form>
            </template>
            <template #footer>
                <div style="flex: auto">
                    <el-button @click="cancelClick">取消</el-button>
                    <el-button type="primary" v-show="show_btn_type==1?true:false" @click="add_sure_Click">添加
                    </el-button>
                    <el-button type="primary" v-show="show_btn_type==2?true:false" @click="edit_sure_Click">编辑
                    </el-button>
                </div>
            </template>
        </el-drawer>
    </div>
</template>
<script>
// 引入接口调用函数,
import { getManageList, deleteManager, addManager, editManager } from '@/service/http.js';
// 引入字体图标组件
import { Plus } from '@element-plus/icons-vue';

// 导入树形路由表中的树形菜单
import { routes } from '@/router/index.js';
export default {
    name: '',
    data() {
        return {
            tableData: [], // 管理者列表总数据
            currentPage: 1, // 当前页 默认1
            isshowdrawer: false, // 是否显示抽屉组件
            formLabelAlign: { // 表单数据对象
                adminname: '',  // 账号
                password: '', //密码
                role: '', // 角色
                checkedKeys: '' // 选中的权限菜单项
            },
            treeData: routes[1].children,  //树形菜单结构数据,使用路由表中配置的routes
            defaultCheckedKeys: [], //树形菜单中默认勾选的节点的 key 的数组
            defaultProps: {
                children: 'children', //指定子树为节点对象的某个属性值
                label: 'label',// 指定属性菜单节点标签为节点对象的某个属性值
            },
            show_btn_type: 1 // 抽屉中的确认按钮是哪一个按钮, 1为添加确认,2为编辑确认
        };
    },
    components: {
        Plus
    },
    computed: {
        // 计算属性: 每当tableData 发生数据变化时,重新计算当前页的数据
        computedTableData() {
            return this.tableData.slice((this.currentPage - 1) * 10, this.currentPage * 10)
        }
    },
    created() {
        // 初始化请求数据列表
        this.getmanagelist()
    },
    methods: {
        // 获取管理员列表接口
        getmanagelist() {
            getManageList().then(res => {
                // console.log(res);
                if (res.code == 200) {
                    this.tableData = res.data
                }
            })
        },
        // 点击修改
        handleEdit(index, row) {
            // 显示抽屉
            this.isshowdrawer = true;
            // 显示编辑确认按钮
            this.show_btn_type = 2;
            // console.log(row);
            // 数据回填
            this.formLabelAlign.adminname = row.adminname
            this.formLabelAlign.password = row.password
            this.formLabelAlign.role = row.role;
            // 设置默认选址树形菜单,需要看node-key 指定的属性是数据中的什么属性,然后将该属性
            // 组成数组赋值给defaultCheckedKeys 就可以啦 结构为[path1,path2,...]
            const checkedKeys = [];
            // console.log(row.checkedKeys);
            row.checkedKeys.forEach(item => {
                item.children.forEach(item1 => {
                    checkedKeys.push(item1.path)
                })
            })
            // 设置回填选中的树形菜单
            nextTick(()=>{
                   this.$refs.treeRef.setCheckedKeys(checkedpathArr)
            })
        },
        //修改确认
        edit_sure_Click() {
            // 获取树形菜单的选中的值
            this.formatCheckedKeys();
            editManager(this.formLabelAlign).then(res => {
                console.log(2, res);
                if (res.code == 200) {
                    // 初始化请求数据列表
                    this.getmanagelist()
                    // 显示抽屉
                    this.isshowdrawer = false;
                }
            })
        },
        // 点击删除
        handleDelete(index, row) {
            // console.log(index, row);
            // 调用删除管理员接口
            deleteManager({ adminid: row.adminid }).then(res => {
                console.log(res);
                if (res.code == 200) {
                    // 删除成功后,重新调用
                    this.getmanagelist()
                }
            })
        },
        // 添加管理员取消
        cancelClick() {
            this.isshowdrawer = false
        },
        //点击添加管理员
        addadminFn() {
            // 显示抽屉
            this.isshowdrawer = true;
            // 显示添加确认按钮
            this.show_btn_type = 1;
            // 清空表单
            this.formLabelAlign.adminname = '';
            this.formLabelAlign.password = '';
            this.formLabelAlign.role = '';
            // 注意清空树形菜单选项只能使用
            // console.log(this.$refs);
            // 当属性菜单还没有渲染出来的时候,使用this.$refs为undefiend,或者放在nextTick 函数中执				行,这样保证有组件实例
            if (this.$refs.treeRef) {
                this.$refs.treeRef.setCheckedKeys([])
            }
        },
        // 获取树形菜单的选中的值
        formatCheckedKeys() {
            //根据ref获取树结构中选中的所有的子菜单(不包括父菜单)
            let list = this.$refs.treeRef.getCheckedNodes(true);
            // console.log(list);
            //当点击添加管理员的时候,希望得到的数据结构是如下: {选中得大类,children:[{选中的小类},{选中的小类}]}
            // [{ label: 账号管理, path: 'manage', children: [{ label: 管理员列表, path: 'managelist' }] }]
            // 定义一个数组,作为最终的选中的数据,结构为[{ label: 账号管理, path: 'manage', children: [{ label: 管理员列表, path: 'managelist' }] }]
            let result = [];
            // 定义中间处理数组
            let templist = [];

            list.forEach(item => {
                //01: 先找到包含的父类  arr.find(item=> return 条件) // 返回符合条件的第一项的值
                const parent = this.treeData.find(item1 => {
                    return item1.children.some((item2) => item2.label == item.label)
                })
                // console.log(100, parent);
                //02: 修改父类中的子类children 因为不一定是所有的子类都选中了
                // 如下确保父类只添加到result 数组中一次
                if (!templist.includes(parent.label)) {
                    templist.push(parent.label);
                    result.push({
                        label: parent.label,
                        path: parent.path,
                        children: [item]
                    })
                } else {
                    // 这一步: 表示同一个大类中不止一个子类被选中 如 账户这个大类下除了子类1管理员列表还有子类2被选中
                    result.find(item1 => item1.label == parent.label).children.push(item)
                }
            })
            // 此处的result为最终的树形菜单选中的结果
            // console.log(result);
            // 调用添加管理员接口
            this.formLabelAlign.checkedKeys = result
        },
        // 添加管理员确定
        add_sure_Click() {
            this.isshowdrawer = false; // 隐藏抽屉
            // 获取树形菜单的选中的值
            this.formatCheckedKeys()
            // 调用添加管理员接口
            addManager(this.formLabelAlign).then(res => {
                // console.log(1, res);
                if (res.code == 200) {
                    // 重新调用管理员列表接口
                    this.getmanagelist()
                    // 隐藏 抽屉
                    this.isshowdrawer = false
                }
            })
        }
    }
}
</script>
<style lang="scss" scoped>
.el-pagination {
    justify-content: flex-end;
    margin-top: 20px;
}
.header {
    padding: 20px 0
}
</style>
```

### 05:  渲染左侧菜单栏导航

在src /components 中修改AsideCom 组件,渲染vuex中userinfo 中存的数据checkedKeys数组.该数组为左侧栏菜单数据.

```vue
<template>
    <el-menu default-active="1-1" :router="true" background-color="#2d3436" text-color="white"
        class="el-menu-vertical-demo" :collapse="isCollapse">
        <el-menu-item index="/home">
            首页
        </el-menu-item>
        <!-- <el-sub-menu index="/home/manage">
            <template #title>
                <el-icon>
                    <location />
                </el-icon>
                <span>账户管理</span>
            </template>
            <el-menu-item index="/home/manage/managelist">管理员列表</el-menu-item>
            <el-sub-menu index="1-4">
                <template #title><span>item four</span></template>
                <el-menu-item index="1-4-1">item one</el-menu-item>
            </el-sub-menu>
        </el-sub-menu> -->
        <el-sub-menu :index="`/home/${item.path}`" v-for="item in userinfo.checkedkeys">
            <template #title>
                <el-icon>
                    <location />
                </el-icon>
                <span>{{item.label}}</span>
            </template>
            <el-menu-item :index="`/home/${item.path}/${child.path}`" v-for="child in item.children">
                {{child.label}}</el-menu-item>
        </el-sub-menu>
    </el-menu>
</template>

<script>
// 引入映射函数
import { mapState } from 'vuex';
import {
    Document,
    Menu as IconMenu,
    Location,
    Setting,
} from '@element-plus/icons-vue'
export default {
    name: '',
    data() {
        return {

        };
    },
    // 在非setup语法糖中引入的图标组件还需要注册,setup语法糖引入就可以使用图标组件
    components: {
        Document,
        IconMenu,
        Location,
        Setting,
    },
    props: {
        isCollapse: { // 接收父传子属性  是否折叠左侧菜单栏
            type: Boolean,
            required: true
        }
    },
    computed: {
        ...mapState(['userinfo'])
    }
}

</script>
<style lang="scss" scoped>
.el-menu {
    width: 100%;
    border-right: none;
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 200px;
    min-height: 400px;
}
</style>
```

### 06: 面包屑导航组件

01:需要在 src/router/index.js 设置 路由中的meta 属性

```js
   children: [
            {
                path: 'manage',
                name: "manage",
                label: '账户管理',
                component: () => import('@/views/manage/manageIndex.vue'),
                meta: {
                    name: "账户管理"
                },
                children: [
                    {
                        path: 'managelist',
                        name: "managelist",
                        label: '管理员列表',
                        meta: {
                            name: "管理员列表"
                        },
                        component: () => import('@/views/manage/manageListView.vue')
                    }
                ]
            },
            .......
```

02: 在src/components 文件夹下创建 BreadCrumbCom.vue 组件,然后引入到首页. 如下为面包屑导航组件

```vue
<template>
    <el-breadcrumb separator="/">
        <el-breadcrumb-item v-for="item in breadcrumbArr" :to="item.path">{{item.meta.name}}</el-breadcrumb-item>
    </el-breadcrumb>
</template>
<script>

export default {
    name: '',
    data() {
        return {
            breadcrumbArr: [] // 定义面包屑导航数组
        };
    },
    watch: {
        $route: { // 监听路由变化
            handler(val) {
                // console.log(val)  当前路由的数据
                this.breadcrumbArr = val.matched
            },
            immediate: true  // 立马监听
        }
    }
}
</script>
<style scoped>

</style>
```



### 07: 设置折叠菜单

在homeView 组件中,设置 

```vue
<template>
    <el-container>
        <el-aside :width="asideWidth">
            <!-- 设置导航图标 -->
            <div class="logo">
                <el-image style="width: 32px; height: 32px" :src="logoImg" />
                <p v-show="!isCollapse">千锋管理后台</p>
            </div>
            <Aside :isCollapse="isCollapse" />
        </el-aside>
        <el-container>
            <el-header>
                <!-- header 头部左边 -->
                <el-icon @click="isCollapse = !isCollapse" :size="22">
                    <!-- 使用动态组件实现切换-->
                    <component :is="headLeft"></component>
                </el-icon>
                <!-- header头部右边头像信息 -->
                ....
            </el-header>
            <el-main>
                <BreadCrumbCom />
                <!-- 右侧要显示的数据 -->
                <router-view class="main_content"></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
// 引入图标组件
import { Fold, Expand, Plus, CirclePlusFilled } from '@element-plus/icons-vue';
// 引入侧边栏组件
import Aside from '@/components/AsideCom.vue';
// 引入图片路径
import logoImg from '@/assets/img/logo.jpeg';
export default {
    name: '',
    data() {
        return {
            isCollapse: false, // 设置是否折叠 表示默认折叠
            logoImg: logoImg, // 引入的logo
            squareUrl: 'https://cube.elemecdn.com/9/c2/f0ee8a3c7c9638a54940382568c9dpng.png',
        };
    },
    components: {
        Fold,
        Expand,
        Aside,
        BreadCrumbCom  // 面包屑导航组件
    },
    computed: {
        asideWidth() {// 左边菜单栏的宽度
            return !this.isCollapse ? '200px' : '64px'
        },
        headLeft() { // 折叠左侧菜单栏使用的折叠图标
            return !this.isCollapse ? Fold : Expand
        },
    },
}
</script>

<style lang="scss" scoped>
.el-container {
    height: 100%;
    background-color: #dfe6e9;

    .el-aside {
        background-color: #2d3436;
        transition: 0.3s linear;
        border-right: none;

        .logo {
            display: flex;
            font-size: 16px;
            height: 56px;
            padding-left: 20px;
            box-sizing: border-box;
            align-items: center;
            width: 100%;

            .el-image {
                margin-right: 10px;
            }

            p {
                transition: 0.3s;
                color: white;
            }
        }
    }

    .el-sub-menu__title {
        color: white !important;
    }

    .el-header {
        background-color: #7f8fa6;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .avatar-box {
        display: flex;
        align-items: center;
    }
}
</style>
```

在 aside 组件中设置

```vue
<template>
    <el-menu :default-active="currentPath" :router="true" background-color="#2d3436" text-color="white"
        class="el-menu-vertical-demo" :collapse="isCollapse">
        <el-menu-item index="/home">
            首页
        </el-menu-item>
        <el-sub-menu :index="`/home/${item.path}`" v-for="item in userinfo.checkedkeys">
            <template #title>
                <el-icon>
                    <location />
                </el-icon>
                <span>{{ item.label }}</span>
            </template>
            <el-menu-item :index="`/home/${item.path}/${child.path}`" v-for="child in item.children">
                {{ child.label }}</el-menu-item>
        </el-sub-menu>
    </el-menu>
</template>

<script>

// 引入图标组件
import {
    Document,
    Menu as IconMenu,
    Location,
    Setting,
} from '@element-plus/icons-vue';

export default {
    name: '',
    data() {
        return {

        };
    },
    // 在非setup语法糖中引入的图标组件还需要注册,setup语法糖引入就可以使用图标组件
    components: {
        Document,
        IconMenu,
        Location,
        Setting,
    },
    props: {
        isCollapse: { // 接收父传子属性  是否折叠左侧菜单栏
            type: Boolean,
            required: true
        }
    },
}

</script>
<style lang="scss" scoped>
.el-menu {
    width: 100%;
    border-right: none;
}
.el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 200px;
    min-height: 400px;
}
</style>
```





### 07: 保证账号在下一次登录时,还是选中上一次打开的菜单项

思路: 将退出账号前的路由保存,下次登录成功后直接跳转到上一次保存的路由地址

01: 在vuex 中设置一个路由参数currentPath 保存到state中

```js
const store = createStore({
    state() {
        return {
            count: 0,
            userinfo: {},// 登录后的用户信息
            currentPath: '/' // 当前的路由,路由每次切换都会更新
        }
    },
    mutations: {
        updateUserinfo(state, payload) { // 更新用户信息
            state.userinfo = payload
        },
        updateCurrentPath(state, payload) { // 更新当前的路由信息
            state.currentPath = payload
        }
    },
    plugins: [
        createPersistedState({
            // 设置要持久化的数据
            reducer: state => {
                return {
                    userinfo: state.userinfo
                }
            }
        })
    ]
})

```

02: 第二步修改 src/components/BreadCrumbCom.vue 组件

```js
  watch: {
        $route: { // 监听路由变化
            handler(val) {
                // console.log(val)  当前路由的数据
                // 给面包屑导航数组赋值
                this.breadcrumbArr = val.matched;
                // 将当前路由的信息存到vuex中, 
                // 注意需要判断当进入登录页时不保存,避免下次变成了从登录页进入登录页的死循环
                val.path != '/login' && this.updateCurrentPath(val.path)
            },
            immediate: true  // 立马监听
        }
    }
```

03:  第三步 修改src/views/loginView.vue组件,登录成功后,直接跳转到vuex持久化保存的路由地址

```js
   computed: {
        ...mapState(['currentPath'])
    }
    
   //01: 登录成功后,跳转到首页
   ....
   this.$router.push(this.currentPath)
```



04: 修改src/components/AsideCom.vue

```vue
 <el-menu :default-active="currentPath" :router="true" background-color="#2d3436" text-color="white" class="el-menu-vertical-demo" :collapse="isCollapse">
     .....
    </el-menu>
 
 <script>
 . .......
   computed: {
        ...mapState(['userinfo', 'currentPath'])
    },
  </script>
```



### 08:  轮播图管理页面

01: 在src/views/swipermanage/manageIndex.vue    创建轮播图管理页

```vue
<template>
    <router-view></router-view>
</template>
<script>
export default {
    name: 'swipermanage',
    data() {
        return {

        };
    },
}
</script>
<style scoped>

</style>
```

02: 在src/views/swiper/manage/manageListView.vue  创建轮播图列表页

```vue
<template>
    <div>
        <!-- 头部 -->
        <div class="header">
            <el-button type="primary" @click="$router.push({name:'addswiper'})">
                添加轮播图<el-icon>
                    <Plus />
                </el-icon>
            </el-button>
        </div>
        <!-- 表格数据 -->
        <el-table :data="computedTableData" style="width: 100%;margin-top: 20px;" :cell-style="{height:'60px'}">
            <el-table-column type="index" label="序号" width="80" align="center" />
            <el-table-column prop="img" label="图片" align="center">
                <template #default="scope">
                    <el-image :src="scope.row.img"></el-image>
                </template>

            </el-table-column>
            <el-table-column prop="link" label="链接" align="center"></el-table-column>
            <el-table-column prop="alt" label="提示" align="center"></el-table-column>
            <el-table-column label="操作" align="center">
                <template #default="scope">
                    <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除
                    </el-button>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination background layout="total,prev, pager, next" v-model:current-page="currentPage"
            :total="tableData.length" />
    </div>
</template>
<script>
// 引入接口调用函数,
import { getSwiperList, deleteoneSwiper } from '@/service/swiperapi.js';
// 引入字体图标组件
import { Plus } from '@element-plus/icons-vue';
export default {
    name: '',
    data() {
        return {
            tableData: [], // 轮播图列表总数据
            currentPage: 1, // 当前页 默认1
        };
    },
    components: {
        Plus
    },
    computed: {
        // 计算属性: 每当tableData 发生数据变化时,重新计算当前页的数据
        computedTableData() {
            return this.tableData.slice((this.currentPage - 1) * 10, this.currentPage * 10)
        }
    },
    created() {
        // 初始化请求数据列表
        this.getswiperlist()
    },
    methods: {
        // 获取轮播图列表接口
        getswiperlist() {
            getSwiperList().then(res => {
                // console.log(res);
                if (res.code == 200) {
                    this.tableData = res.data
                }
            })
        },
        // 点击删除
        handleDelete(index, row) {
            // console.log(index, row);
            // 调用删除某一条轮播图数据接口
            deleteoneSwiper({ bannerid: row.bannerid }).then(res => {
                // console.log(res);
                if (res.code == 200) {
                    // 删除成功后,重新调用
                    this.getswiperlist()
                }
            })
        }
    }
}

</script>
<style lang="scss" scoped>
.el-pagination {
    justify-content: flex-end;
    margin-top: 20px;
}
.header {
    padding: 20px 0
}
</style>
```



### 09. 添加轮播图

在src/views/swipermanage/ 目录下创建一个 addswiperView.vue 页面, 

```vue
<template>
    <el-form ref="ruleFormRef" :model="ruleForm" status-icon :rules="rules" label-width="60px" class="demo-ruleForm">
        <el-form-item label="link" prop="link">
            <el-input v-model="ruleForm.link" type="text" autocomplete="off" placeholder="请输入link" />
        </el-form-item>
        <el-form-item label="alt" prop="alt">
            <el-input v-model="ruleForm.alt" type="text" autocomplete="off" placeholder="请输入图片名称" />
        </el-form-item>
        <el-form-item label="图片">
            <el-upload class="avatar-uploader" :http-request="httprequestFn" :show-file-list="false">
                <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                <el-icon v-else class="avatar-uploader-icon">
                    <Plus />
                </el-icon>
            </el-upload>
        </el-form-item>
        <el-form-item label="base64" prop="img">
            <el-input v-model="ruleForm.img" placeholder="图片自动转成base64" />
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="submitForm(ruleFormRef)">提交</el-button>
            <el-button @click="resetForm(ruleFormRef)">重置</el-button>
        </el-form-item>
    </el-form>
</template>

<script>
import { ElMessage } from 'element-plus';
import { Plus } from '@element-plus/icons-vue';
import { addSwiper } from '@/service/swiperapi.js';
export default {
    name: '',
    data() {
        return {
            ruleFormRef: '', //该变量为 submitForm事件函数的形参
            imageUrl: '', // 预览的图片地址
            ruleForm: { // 提交的表单数据,参数名与接口文档上的参数保持一致
                link: '',
                alt: '',
                img: '',
            },
            rules: { // 表单的校验规则
                img: [{ required: true, trigger: 'blur', message: '请输入内容' }],
                alt: [{ required: true, trigger: 'blur', message: '请输入内容' }],
                link: [{ required: true, trigger: 'blur', message: '请输入内容' }],
            }
        };
    },
    components: { // 注册组件
        Plus
    },
    mounted() {
        // 给  ruleFormRef 这个表单组件的实例赋值
        this.ruleFormRef = this.$refs.ruleFormRef
    },
    methods: {
        submitForm(formEl) { // 表单提交事件
            if (!formEl) return
            formEl.validate((valid) => {
                if (valid) {
                    // console.log(this.ruleForm)
                    addSwiper(this.ruleForm).then(res => {
                        console.log(res);
                        if (res.code == 200) {
                            // 跳转到轮播图列表页面
                            this.$router.push('/home/swipermanage/swiperlist')
                        }
                    })
                } else {
                    console.log('error submit!')
                    return false
                }
            })
            console.log(this.ruleForm);
        },
        //element-ui-plus 上的api方法, 覆盖默认的 Xhr 行为，允许自行实现上传文件的请求
        httprequestFn(data) {
            console.log(data); // data对象中的file就是要上传的文件信息

            // 01: 对上传的文件的类型做判断,只能上传图片格式的数据
            let fileTypeFlag = ['image/jpeg', 'image/jpg', 'image/png'].includes(data.file.type)
            if (!fileTypeFlag) {
                // 提示文件格式不正确
                ElMessage.error('上传的文件的图片格式不正确!');
                return false

            } else if (data.file.size / 1024 / 1024 > 2) {
                // 否则提示文件格式不正确
                ElMessage.error('文件体积不允许超过2MB!')
                return false
            }

            // 02: 根据data中的file.生成一个图片的url,用于展示预览
            this.imageUrl = URL.createObjectURL(data.file);

            // 03: 将获取的图片地址转成base64格式
            this.getbase64Format(data.file).then(res => {
                // console.log(res);
                this.ruleForm.img = res
            })
        },
        getbase64Format(file) {
            // 转成的base64的 过程是一个异步过程,所以咱们可以使用promise 封装一下
            return new Promise((resolve, reject) => {
                let reader = new FileReader();
                let fileResult = '';
                reader.readAsDataURL(file);
                // 监听读取文件 为 base64
                reader.onload = () => {
                    fileResult = reader.result
                };
                // 读取错误的时候触发
                reader.onerror = (error) => {
                    reject(error)
                }
                // 读取结束
                reader.onloadend = () => {
                    resolve(fileResult)
                }
            })
        }
    }


}

</script>
<style lang="scss" scoped>
.el-form {
    margin-top: 30px;
    width: 300px;
}

.avatar-uploader .avatar {
    width: 178px;
    height: 178px;
    display: block;
}

.avatar-uploader .el-upload {
    border: 2px dashed white;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: var(--el-transition-duration-fast);
}

.avatar-uploader .el-upload:hover {
    border-color: white;
}

.el-icon.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    text-align: center;
    border: 1px dashed white;
}
</style>
```



添加路由配置  addswiper路由配置项

```js
 {
     path: 'swipermanage',
         name: "swipermanage",
             label: '轮播图管理',
                 meta: {
                     name: "轮播图管理"
                 },
                     component: () => import('@/views/swipermanage/manageIndex.vue'),
                         children: [
                             {
                                 path: 'swiperlist',
                                 name: "swiperlist",
                                 label: '轮播图列表',
                                 meta: {
                                     name: "轮播图列表"
                                 },
                                 component: () => import('@/views/swipermanage/manageListView.vue')
                             },
                             {
                                 path: 'addswiper',
                                 name: "addswiper",
                                 label: '添加轮播图',
                                 meta: {
                                     name: "添加轮播图"
                                 },
                                 component: () => import('@/views/swipermanage/addswiperView.vue')
                             }
                         ]
 },
```



**注意:** 配置对象的添加轮播图路由, 但是希望该路由不在权限中显示,需要如下处理:

在 src/views/manage/manageList.vue 页面中,进行路由配置的修改,将data 中的treeData 数据属性修改成计算属性

```js
// 原数据结构
data(){
	return  {
	   treeData: routes[1].children
	}
 }

// 导入 loadsh  提供了更加简便操作js 的方法,实现对原数据的深拷贝,此处拷贝路由数组
import { cloneDeep } from 'lodash';
// 修改为计算属性
computed:{
	 // 当路由每次变化时,也就是添加新的路由时,都会重新生成新的抽屉中的权限菜单
    treeData() {
        let cloneTreeData = cloneDeep(routes[1].children);
        console.log(cloneTreeData);
        cloneTreeData[1].children.pop(cloneTreeData[1].children[1])
        return cloneTreeData
    }
}

// 页面修改
 <el-form-item label="权限">
 <!-- 树形菜单 -->
     <el-tree ref="treeRef" :data="treeData" show-checkbox node-key="path" default-expand-all
:default-checked-keys="defaultCheckedKeys" :props="defaultProps" />
 </el-form-item>
```



### 10. 产品列表

在src/views/productmanage 目录下,创建 productListView.vue 组件

```vue
<template>
    <el-table :data="tableData" :row-style="{height:'80px'}"
        :cell-style="{textAlign:'center', height: '60px',overflow:'hidden' }" max-height="960" style="width: 100%">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column prop="img" label="图片">
            <template #default="scope">
               <!-- preview-src-list 为图片预览属性  preview-teleported -->
                <el-image style="height:100px" :preview-src-list="[
                  scope.row.img1,
                  scope.row.img2,
                  scope.row.img3,
                  scope.row.img4,
                ]" :src="scope.row.img1" preview-teleported> </el-image>
            </template>
        </el-table-column>
        <el-table-column prop="proname" label="名称"></el-table-column>
        <el-table-column prop="brand" label="品牌"></el-table-column>
        <el-table-column prop="category" label="分类" :filters="filters" :filter-method="filterHandler"></el-table-column>
        <el-table-column prop="originprice" label="原价" sortable></el-table-column>
        <el-table-column prop="discount" label="折扣" sortable></el-table-column>
        <el-table-column prop="sales" label="销量" sortable></el-table-column>
        <el-table-column label="是否售卖">
            <template #default="scope">
                <!-- 当 v-model的值与active-value相同,则 active对应的样式起作用,否则反之-->
                <el-switch v-model="scope.row.issale" :active-value="1" :inactive-value="0" class="ml-2"
                    style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
                    @change="updateProductStatus('issale',scope.row.issale,scope.row.proid)" />
            </template>
        </el-table-column>
        <el-table-column label="是否秒杀">
            <template #default="scope">
                <el-switch v-model="scope.row.isseckill" :active-value="1" :inactive-value="0" class="ml-2"
                    style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
                    @change="updateProductStatus('isseckill',scope.row.isseckill,scope.row.proid)" />
            </template>
        </el-table-column>
        <el-table-column prop="isrecommend" label="是否推荐">
            <template #default="scope">
                <el-switch v-model="scope.row.isrecommend" :active-value="1" :inactive-value="0" class="ml-2"
                    style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
                    @change="updateProductStatus('isrecommend',scope.row.isrecommend,scope.row.proid)" />
            </template>
        </el-table-column>
        <el-table-column label="操作">
            <template #default="scope">
                <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
        </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination background layout="prev, pager, next" @current-change="changePageFn(page)"
        v-model:current-page="currentPage" :total="total" />
</template>

<script>
 // 引入对应的接口api
import { getProList, updateProduct } from '@/service/productapi.js'
export default {
    name: '',
    data() {
        return {
            tableData: [], // 表格数据
            currentPage: 1,  // 当前页数
            total: 0, // 总的数据条数
        };
    },
    computed: {
        // 设置分类的筛选filters 为计算属性
        filters() {
            let arr = this.tableData.map(item => {
                return item.category
            })
            // 将数组arr 去重,利用 new set(数组) 方法 返回一个类数组,
            // 然后利用 Array.from转成数组
            // console.log(1, new set(arr));
            let newArr = Array.from(new Set(arr))
            //console.log(2, newArr);
            return newArr.map(item => {
                return { text: item, value: item }
            })
        }
    },
    mounted() {
        // 请求产品列表数据
        this.getproductlist()
    },
    methods: {
        // 获取商品列表
        getproductlist() {
            getProList({
                count: this.currentPage,
                limitNum: 10
            }).then(res => {
                console.log(res);
                if (res.code == 200) {
                    this.tableData = res.data
                    this.total = res.total
                }
            })
        },
        //更新某一条的商品的状态
        updateProductStatus(type, flag, proid) {
            // console.log(type, flag, proid);
            updateProduct({
                type,
                flag,
                proid
            }).then(res => {
                // console.log(res);
                if (res.code == 200) {
                    this.getproductlist()
                }
            })
        },
        changePageFn(page) {
            // console.log(this.currentPage);
            // 请求产品列表数据
            this.getproductlist()
        },
        // 表头的分类的筛选功能
        filterHandler(value, row, column) {
            // console.log(value, row, column);
            const property = column['property']
            return row[property] === value
        }
    }
}

</script>
<style lang="scss" scoped>
.el-table {
    margin-top: 20px;

    /* 样式穿透 */
    ::v-deep .cell {
        height: 90px;
        text-overflow: ellipsis;
    }
}

.el-pagination {
    justify-content: flex-end;
    margin-top: 20px;
}
</style>
```

### 11. 秒杀列表

参照 产品列表排版和功能

### 12. 推荐列表

参照 产品列表排版和功能

### 13.筛选列表

参照 产品列表排版和功能

### 14. 数据可视化

在src/views/echarts 文件下创建 EchartsIndexView.vue, echartsOtherView.vue,  echartShowdata.vue文件,

echartShowdata.vue 文件内容如下:

```vue
<template>
    <div class="echarts-container">
        <div id="echarts-main"></div>
    </div>
</template>

<script>
// 引入echarts 
import * as echarts from 'echarts';
// 引入echarts图表接口
import { getcandlestickData } from '@/service/echartapi.js'
export default {
    name: 'echartShowdata',
    data() {
        this.myChart = null;
        return {

        };
    },
    methods: {
        drawEcharts(datalist) {
            // 绘制图表
            this.myChart.setOption({
                // 用于显示echarts图表的title
                title: {
                    text: 'K线图',
                },
                // 鼠标滑动展示信息
                tooltip: {
                    trigger: 'axis',
                },
                // 横轴展示的数据，格式是一个数据
                xAxis: {
                    data: datalist.x
                },
                yAxis: {},
                // 工具箱的设置
                toolbox: {
                    feature: {
                        // 图形缩放功能
                        dataZoom: {
                            yAxisIndex: 'none',
                        },
                        // 数据窗口的功能
                        dataView: { readOnly: false },
                        // 图形类型切换的功能
                        magicType: {
                            type: ['line', 'bar'],
                        },
                        // 重置功能
                        restore: {},
                        // 图表保存为图片功能
                        saveAsImage: {},
                    },
                },
                series: [
                    {
                        type: 'candlestick',
                        data: datalist.val
                    }
                ]
            })
        },
        getData() {
            getcandlestickData().then((res) => {
                this.drawEcharts(res.data)
            })
        },

    },
    mounted() {
        // 基于准备好的dom，初始化echarts实例
        this.myChart = echarts.init(document.getElementById('echarts-main'))
        this.getData()
    }
}

</script>
<style lang="scss" scoped>
.echarts-container {
    width: 100%;
    height: 100%;
    position: relative;
    #echarts-main {
        width: 600px;
        height: 600px;
        background-color: #fff;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
    }
}
</style>
```

echartsOtherView.vue 文件内容如下:

```vue
<template>
    <div class="echarts-container">
        <div id="echarts-main"></div>
    </div>
</template>
  
<script>
import * as echarts from 'echarts'
import { getSimpleData } from '@/service/echartapi'
export default {
    data() {
        this.myChart = null
        return {}
    },
    methods: {
        drawEcharts(datalist) {
            // 绘制图表
            this.myChart.setOption({
                // 用于显示echarts图表的title
                title: {
                    text: 'ECharts 入门示例',
                },
                // 鼠标滑动展示信息
                tooltip: {
                    trigger: 'axis',
                },
                // 横轴展示的数据，格式是一个数据
                xAxis: {
                    data: datalist.map((item) => item.x),
                },
                // 一般不用设置，会根据数据自动设置
                yAxis: {},
                // 工具箱的设置
                toolbox: {
                    feature: {
                        // 图形缩放功能
                        dataZoom: {
                            yAxisIndex: 'none',
                        },
                        // 数据窗口的功能
                        dataView: { readOnly: false },
                        // 图形类型切换的功能
                        magicType: {
                            type: ['line', 'bar'],
                        },
                        // 重置功能
                        restore: {},
                        // 图表保存为图片功能
                        saveAsImage: {},
                    },
                },
                // 控制某个系列数据的展示
                legend: {
                    data: ['饼图', '销量', '价格'],
                },
                series: [
                    {
                        name: '饼图',
                        type: 'pie',
                        radius: [50, 250], // 数组的第一项是内半径，第二项是外半径。
                        center: ['50%', '50%'], // 饼图的中心（圆心）坐标，数组的第一项是横坐标，第二项是纵坐标
                        roseType: 'area', // 是否展示成南丁格尔图，通过半径区分数据大小。
                        // 用于指定饼图扇形区块的内外圆角半径，支持设置固定数值或者相对于扇形区块的半径的百分比值
                        itemStyle: {
                            borderRadius: 8,
                        },
                        data: datalist.map((item, index) => {
                            return { value: item.val, name: 'rose' + index }
                        }),
                    },
                    {
                        name: '销量', // 一系列数据的名字
                        type: 'line', // 数据展示的类型： 柱状图、折线图、饼图
                        data: datalist.map((item) => item.val), // 要展示的数据集合（系列）
                    },
                    {
                        name: '价格',
                        type: 'bar',
                        data: [5, 20, 36, 10, 10, 20, 36],
                    },
                ],
            })
        },
        getData() {
            getSimpleData().then((res) => {
                this.drawEcharts(res.data)
            })
        },
    },
    mounted() {
        // 基于准备好的dom，初始化echarts实例
        this.myChart = echarts.init(document.getElementById('echarts-main'))
        this.getData()
    },
}
</script>
  
<style lang="scss" scoped>
.echarts-container {
    width: 100%;
    height: 100%;
    position: relative;
    #echarts-main {
        width: 600px;
        height: 600px;
        background-color: #fff;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
    }
}
</style>
```



### 15. 富文本编辑器

官网地址: http://tinymce.ax-z.cn/

#### 1. 下载富文本编辑器代码包

地址: http://tinymce.ax-z.cn/download-all.php

#### 2. 下载并配置汉化包(可选)

地址 : http://tinymce.ax-z.cn/general/localize-your-language.php

#### 4. index.html 中引入

```html
 <script src="./src/assets/tinymce/tinymce.min.js"></script>
```

#### 5. 单文件中使用

```vue
<template>
  <textarea id="mytextarea">Hello, World!</textarea>
  <button @click="submitFn">提交</button>
</template>

<script>
export default {
  name: '',
  data() {
    return {}
  },
  mounted() {
    tinymce.init({
      selector: '#mytextarea',
      branding: false,
      language: 'zh_CN',
      statusbar: false  //隐藏编辑器下方状态栏
    })
  },
  methods: {
    // 点击提交
    submitFn() {
      console.log(tinymce.activeEditor.getContent())
      // 调用接口
    }
  }
}
</script>
<style scoped>
#mytextarea {
  width: 100%;
  height: 500px;
}
/*解决菜单栏层级低不显示的问题*/
.tox-tinymce-aux {
  z-index: 709826033 !important;
}
.tox-tinymce-inline {
  z-index: 709826032 !important;
}
</style>
```



### 16:地图的使用





### 17.数据导出

数据导出:  https://www.npmjs.com/package/js-export-excel

```vue
<template>
  <div>
    <!-- 用户列表 -->

    <el-button type="primary" @click="exportFn">数据导出</el-button>
    <el-table :data="productlist" style="width: 100%" :row-style="{ height: '50px' }">
      <el-table-column label="序号" align="center">
        <template #default="scope">
          <div style="display: flex; align-items: center; justify-content: center">
            <span style="margin-left: 10px">{{ (pageNum - 1) * limitNum + scope.$index + 1 }}</span>
          </div>
        </template>
      </el-table-column>
      <el-table-column label="图片" align="center">
        <template #default="scope">
          <div style="display: flex; align-items: center; justify-content: center">
            <el-image :src="scope.row.img1"></el-image>
          </div>
        </template>
      </el-table-column>
      <el-table-column label="名称" align="center">
        <template #default="scope">
          <div style="display: flex; align-items: center; justify-content: center">
            {{ scope.row.proname }}
          </div>
        </template>
      </el-table-column>
      <el-table-column label="品牌" prop="brand" align="center"> </el-table-column>
      <el-table-column label="分类" align="center" prop="category"> </el-table-column>
      <el-table-column label="原价" align="center" sortable prop="originprice"> </el-table-column>
      <el-table-column label="折扣" align="center" sortable prop="discount"> </el-table-column>
      <el-table-column label="销量" align="center" sortable prop="sales"> </el-table-column>
      <el-table-column label="是否售卖" align="center">
        <template #default="scope">
          <!-- 当 v-model的值与active-value相同,则 active对应的样式起作用,否则反之-->
          <el-switch
            v-model="scope.row.issale"
            style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
            :active-value="1"
            :inactive-value="0"
            @change="updateProductStatus('issale', scope.row.issale, scope.row.proid)"
          />
        </template>
      </el-table-column>
      <el-table-column label="是否秒杀" align="center">
        <template #default="scope">
          <el-switch
            v-model="scope.row.isseckill"
            style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
            :active-value="1"
            :inactive-value="0"
            @change="updateProductStatus('isseckill', scope.row.isseckill, scope.row.proid)"
          />
        </template>
      </el-table-column>
      <el-table-column label="是否推荐" align="center">
        <template #default="scope">
          <el-switch
            v-model="scope.row.isrecommend"
            style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
            :active-value="1"
            :inactive-value="0"
            @change="updateProductStatus('isrecommend', scope.row.isrecommend, scope.row.proid)"
          />
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center">
        <template #default="scope">
          <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页组件 -->
    <el-pagination
      v-model:page-size="limitNum"
      background
      layout="prev, pager, next"
      :total="total"
      v-model:current-page="pageNum"
      :default-page-size="limitNum"
    />
  </div>
</template>
<script>
import { getproductlist } from '@/utils/product'
 //导入数据导出需要使用的模块(已下载好的第三方)
import ExportJsonExcel from 'js-export-excel'
export default {
  name: '',
  data() {
    return {
      productlist: [],
      pageNum: 1,
      limitNum: 10,
      total: 0 // 总的数据条数
    }
  },

  watch: {
    pageNum(newvalue) {
      console.log('newvalue', newvalue)
      this.getdatalist(newvalue)
    }
  },
  created() {
    this.getdatalist(this.pageNum)
  },
  methods: {
    getdatalist(pageNum) {
      getproductlist({ count: pageNum, limitNum: this.limitNum }).then((res) => {
        this.productlist = res.data.data
        this.total = res.data.total
      })
    },
    exportFn() {
      // 数据导出
      var option = {
        fileName: '产品列表',
        datas: [
          {
            sheetData: this.productlist, //要导出的数据
            sheetName: '产品列表1', // 导出的表格页签名
            sheetFilter: ['proname', 'img1', 'category'], // 需要导出的数据中哪些的字段
            sheetHeader: ['产品名称', '图片', '分类'], // 表头的值
            columnWidths: [20, 20] // // 设置列宽
          }
        ]
      }
      var toExcel = new ExportJsonExcel(option) //new
      toExcel.saveExcel() //保存
    }
  }
}
</script>
<style lang='scss' scoped>
::v-deep .cell {
  height: 60px;
  white-space: nowrap;
  overflow: hidden;
  line-clamp: 2;
  text-overflow: ellipsis;
  display: -webkit-box;
}
::v-deep .el-table thead {
  height: 50px !important;
  .cell {
    height: 40px;
  }
}
</style>
```



数据导入: 

参考地址: https://www.npmjs.com/package/xlsx