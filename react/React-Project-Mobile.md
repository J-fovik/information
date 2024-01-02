# 移动端项目开发 webapp 

项目的接口文档地址:  http://47.94.148.165:3001/apidoc/
项目的预览地址:   http://39.97.100.253:70/
后端项目地址: http://47.94.148.165/

百度网盘视频地址:  链接：https://pan.baidu.com/s/1hmeNBxV7sFdFVKZDhlAdgg?pwd=u4fj  提取码：u4fj 

项目使用的技术栈:  react18+react-router-dom@5 + redux +react-redux +antd-design-mobile +axios



## 1.创建react项目

```shell
npx create-react-app 项目名称
```

## 2.安装项目依赖

```shell
npm i -S axios redux react-redux  styled-components react-router-dom@5 sass

```

## 3.安装@craco/craco 

实现对react脚手架 内置的webpack 进行默认的修改.

注意: 该包@craco/craco 可以显示对react脚手架内置的webpack 做修改,需要借助一个配置文件craco.config.js, 在该文件中做配置. 

参考地址: https://www.npmjs.com/package/@craco/craco

 ```
npm i -D @craco/craco
 ```

 在项目的根目录创建一个  craco.config.js 文件,  

 ```ts
 const path = require('path');
 module.exports = {
     webpack: {
         alias: {
             "@": path.resolve(__dirname, 'src')
         }
     }
 }
 ```

4.  修改`package.json`中的脚本命令为如下：

 如上修改完webpack配置,需要修改package.json中的一些脚本命令配置,这样如上修改的配置才能生效. 将react-scripts 替换成 如下

 > 参考来源：https://www.npmjs.com/package/@craco/craco

 ![1671980571549](C:\Users\wusheng\AppData\Roaming\Typora\typora-user-images\1671980571549.png)



修改完如上配置后, 重启项目, 这样craco.config.js文件就被加载了, 就可以使用@ 在项目中表示srck目录啦



## 4.项目中 安装sass  

create-react-app 脚手架支持sass , 所以只需要安装 sass 直接使用就可以了.

参考地址:https://create-react-app.bootcss.com/docs/adding-a-sass-stylesheet

```
$ npm install sass
# 或者
$ yarn add sass
```

 现在，你可以将 `src/App.css` 重命名为 `src/App.scss`，并更新 `src/App.js` 以引入 `src/App.scss`。 如果以扩展名 `.scss` 或 `.sass` 引入，则此文件和其他文件都将会自动编译。 

​     

## 5.引入全局css 文件

```
npm i normalize.css
```

在 入口文件 index.ts 中引入

```
//引入重置样式表文件
import 'normalize.css'
```

## 6.实现移动端适配 

安装 postcss-px-to-viewport-8-plugin 第三方插件,可以将css样式中的px单位自动转成vw 单位, 不需要开发者手动写vw单位了

```
npm i postcss-px-to-viewport-8-plugin -D   // 将用户写的样式中的px单位自动转换为vw vh单位
```

在根目录下的 craco.config.js 中配置如下代码:

```ts
const path = require('path');
module.exports = {
    webpack: {
        alias: {
            "@": path.resolve(__dirname, 'src') // 配置路径别名
        }
    },
    style: {
        postcss: {
            mode: "extends",
            loaderOptions: {
                postcssOptions: {
                    ident: "postcss",
                    plugins: [
                        [
                            "postcss-px-to-viewport-8-plugin",
                            {
                                viewportWidth: 375, // 设计稿的视口宽度
                            },
                        ],

                        // pxtorem({
                        //     rootValue: 37.5,
                        //     propWhiteList: [],
                        //     minPixelValue: 2,
                        //     exclude: /node_modules/i
                        // }),
                    ],
                },
            },
        },
    }
}
```



## 7.最后安装 antd-mobile 组件库

```shell
npm install --save antd-mobile
```

在页面组件中使用

```tsx
import { Button, Space } from 'antd-mobile'
function App() {
  return (
    <div className="App">
      <Button color='primary'>Primary</Button>
    </div>
  );
}

export default App;


```



## 8.配置项目路由

在src/router/Index.jsx;

```jsx
import React from 'react';
import Home from '@/views/Home';
import Mine from '@/views/Mine';
import Login from '@/views/Login';
import Cart from '@/views/Cart';

import { Route, Switch, Redirect, } from 'react-router-dom'
const Index = () => {
    return (
        <Switch>
            <Route path='/home' component={Home}></Route>
            <Route path='/cart' component={Cart}></Route>
            <Route path='/mine' component={Mine}></Route>
            <Route path='/login' component={Login}></Route>
            <Redirect from='/' to='/home' exact></Redirect>
        </Switch>
    );
}

export default Index;

```

将Index.jsx 文件引入到根组件App.jsx中;

```jsx
import './App.scss';
// 引入路由规则组件
import Index from '@/router/Index';

function App() {
  return (
    <div className="App">
      {/* 使用路由规则和坑 */}
      <Index></Index>
    </div>
  );
}

export default App;

```



## 9.封装axios  

```ts
// 封装axios
import axios from 'axios';
// 创建axios 实例
let baseUrl = '';
switch (process.env.NODE_ENV) {
    case 'production':
        baseUrl = 'http://47.94.148.165:3001/';
        break;
    case 'development':
        baseUrl = '/api'
        break;
}

const instance = axios.create({
    baseURL: baseUrl,
    timeout: 5000,
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' }
});

// 添加请求拦截器

instance.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    // 如果本地有jwt(token) 我就将token添加倒排请求头中
    if (localStorage.getItem('token')) {
        //判断headers存在的情况
        config.headers && (config.headers.token = localStorage.getItem('token'))

    }

    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});


// 添加响应拦截器
instance.interceptors.response.use(function (response) {
    // console.log('响应拦截器', response);
    // if (response.data.context && response.data.context.jwt) {
    //     // 将该jwt(token) 存到本地
    //     // localStorage.setItem('jwt', response.data.context.jwt)
    // }
    // 对响应数据做点什么
    return response;
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
});

export default instance


```

## 9.配置前端的react 反向代理 

当后端给的服务器地址没有设置允许跨域, 那么前端再请求后端的服务器地址的时候, 会出现跨域的情况. 

(解决后端接口没有做cros 允许跨域设置)

配置跨域代理

```
npm i -D http-proxy-middleware 
// 开发环境安装的依赖,上线打包的时候,不会打包这些文件的
# http-proxy-middleware：代理中间件，在vue中默认写好代理配置即可，在react中需要先安装三方的包，才能写代理配置。
```

注意: 需要在src 目录新建setupProxy.js 文件, 内容如下

```js

// 引入代理中间件
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function (app) {
    app.use(
        createProxyMiddleware('/api', {
            target: 'http://47.94.148.165:3001',
            changeOrigin: true, // needed for virtual hosted sites
            ws: true, // proxy websockets
            pathRewrite: {
                '^/api': ''
            }
        })
    )
};
```

​       

## 10.定义接口请求文件 

```ts
// 定义项目的接口请求
import instance from "@/https/http";
//首页的列表数据请求
export function getlistdata(data) {
    return instance({
        url: '/api/pro/list',
        method: 'get',
        params: data

    })
}
```



## 11.首页布局

#### 	1.头部搜索组件

```jsx
//src/components/Mysearch.jsx 
import React from 'react';
import { SearchBar } from 'antd-mobile'
const Mysearch = () => {
    return (
        <div style={{
            display: 'flex',
            alignItems: 'center',
            background: 'red',
            padding: "10px 0"

        }}>
            <span style={{
                padding: '0 5px',
                fontSize: '16px',
                color: 'white'
            }}>地址</span>
            <SearchBar
                placeholder='请输入内容'
                style={{
                    '--border-radius': '100px',
                    '--background': '#ffffff',
                    '--height': '40px',
                    '--padding-left': '12px',
                    flex: 1
                }}
            />
            <span style={{
                padding: '0 5px',
                fontSize: '16px',
                color: 'white'
            }}>我的</span>
        </div>
    );
}

export default Mysearch;

```

#### 2.轮播图组件

```jsx
import React from 'react';
// 定义轮播图组件
import { Swiper } from 'antd-mobile'
const Myswiper = (props, ref) => {
    console.log('props', props);
    const { swiperArr } = props
    return (
        <Swiper autoplay autoplayInterval={1000} loop>
            {
                swiperArr.map((item, index) => <Swiper.Item key={index} >
                    <img style={{ width: '100%' }} src={item} />
                </Swiper.Item>)
            }
        </Swiper>
    );
}

export default Myswiper;

```

#### 3.导航菜单组件 

```jsx
import React from 'react';
import { Grid } from 'antd-mobile';
const navList = [
    { navid: 1, title: '嗨购超市', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/125678/35/5947/4868/5efbf28cEbf04a25a/e2bcc411170524f0.png' },
    { navid: 2, title: '数码电器', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/178015/31/13828/6862/60ec0c04Ee2fd63ac/ccf74d805a059a44.png' },
    { navid: 3, title: '嗨购服饰', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/41867/2/15966/7116/60ec0e0dE9f50d596/758babcb4f911bf4.png' },
    { navid: 4, title: '嗨购生鲜', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/177902/16/13776/5658/60ec0e71E801087f2/a0d5a68bf1461e6d.png' },
    { navid: 5, title: '嗨购到家', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/196472/7/12807/7127/60ec0ea3Efe11835b/37c65625d94cae75.png' },
    { navid: 6, title: '充值缴费', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/185733/21/13527/6648/60ec0f31E0fea3e0a/d86d463521140bb6.png' },
    { navid: 7, title: '9.9元拼', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/36069/14/16068/6465/60ec0f67E155f9488/595ff3e606a53f02.png' },
    { navid: 8, title: '领券', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/186080/16/13681/8175/60ec0fcdE032af6cf/c5acd2f8454c40e1.png' },
    { navid: 9, title: '领金贴', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/196711/35/12751/6996/60ec1000E21b5bab4/38077313cb9eac4b.png' },
    { navid: 10, title: 'plus会员', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/37709/6/15279/6118/60ec1046E4b5592c6/a7d6b66354efb141.png' }
];
// 定义首页的导航列表组件
const Mynavlist = () => {
    return (
        <>
            <Grid columns={5} gap={8}>
                {
                    navList.map(item => {
                        return <Grid.Item key={item.navid}>
                            <div style={{
                                display: 'flex',
                                justifyContent: 'center',
                                alignItems: 'center',
                                flexDirection: 'column',
                                paddingTop: '20px'
                            }}>
                                <img style={{
                                    width: '28px'
                                }} src={item.imgurl} />
                                <p style={{
                                    paddingTop: '5px'
                                }}>{item.title}</p>
                            </div>
                        </Grid.Item>
                    })
                }
            </Grid>
        </>
    );
}

export default Mynavlist;
```

#### 4.定义秒杀列表组件

```
// src/components/Mysearch.jsx
import React from 'react';
import { Card, Image } from 'antd-mobile'
const Seckill = (props) => {
    const { seckillArr } = props;
    return (
        <div>
            <Card title='京东秒杀' />
            <ul style={{
                display: 'flex',
                // alignItems: 'center'
            }}>
                {
                    seckillArr.map(item => <li key={item.proid} style={
                        {
                            listStyle: 'none',
                            padding: '0 5px'
                        }
                    }>
                        <Image src={item.img1} height={'100px'} />
                        <p style={{
                            color: 'red'
                        }}>${item.originprice}</p>
                    </li>)
                }
            </ul>
        </div>
    );
}

export default Seckill;

```



#### 5.定义产品列表组件

```jsx
// src/components/Productlist.jsx
import React from 'react';
import { Image, Ellipsis } from 'antd-mobile'
import { useHistory } from 'react-router-dom'
// 产品列表组件

const Productlist = (props) => {
    const { prolist } = props
    // console.log(prolist);
    const his = useHistory();

    return (
        <ul className='probox' style={{
            display: 'flex',
            flexWrap: 'wrap'

        }}>
            {
                prolist.map(item => <li
                    key={item.proid}
                    style={{
                        width: "50%",
                        display: 'flex',
                        flexDirection: 'column',
                    }}
                    onClick={() => {
                        his.push('/detail/' + item.proid)
                    }}
                >
                    <Image src={item.img1} width={'100%'}></Image>
                    <Ellipsis direction='end' rows={2} content={item.proname}></Ellipsis>
                    <p style={{
                        color: 'red',
                        fontSize: '15px'
                    }}>${item.originprice.toFixed(2)}</p>
                </li>)
            }
        </ul>
    );
}

export default Productlist;

```



#### 6.定义首页组件

实现上拉加载下拉刷新功能

```jsx
import React, { useEffect, useState } from 'react';
// 引入搜索组件
import Mysearch from '@/components/Mysearch';
// 引入轮播图组件
import Myswiper from '@/components/Myswiper';
// 引入接口
import { getSwiperApi, getSeckillApi, getProlistApi } from '@/api/home'
// 引入导航菜单组件
import Mynavmenu from '@/components/Mynavmenu';
// 引入京东秒杀
import Seckill from '@/components/Seckill';
// 引入产品列表组件
import Productlist from '@/components/Productlist';
// 引入antd 组件 
import { InfiniteScroll, DotLoading, PullToRefresh } from 'antd-mobile'
import { sleep } from 'antd-mobile/es/utils/sleep'

const Home = () => {
    // 定义轮播图数组
    const [swiperArr, setSwiperArrFn] = useState([])
    // 定义秒杀数据
    const [seckillArr, setSecKillArrFn] = useState([])
    // 定义产品列表当前页
    const [count, setCount] = useState(1)
    // 定义产品列表每页数据的条数
    const [limitNum, setLimitNumFn] = useState(10)
    // 定义产品列表数组
    const [prolist, setProListFn] = useState([])
    // 定义是否有更多 为boolean
    const [hasMore, setHasMore] = useState(true)

    // 自定义触底文案组件
    const InfiniteScrollContent = ({ hasMore }) => {
        return (
            <>
                {hasMore ? (
                    <>
                        <span>Loading</span>
                        <DotLoading />
                    </>
                ) : (
                    <span>--- 我是有底线的 ---</span>
                )}
            </>
        )
    }

    async function loadMore() {
        // 将页面count+1 
        setCount(count + 1) //  setCount(count + 1) 修改是异步的, 修改完直接获取值为上一次的值
        console.log('loadMore-count', count); //1 ,2,3,4,,5
        const append = await getProlistApi({ count, limitNum })
        setProListFn(val => [...val, ...append.data.data])
        setHasMore(append.data.data.length > 0)
        if (append.data.data.length == 0) {
            setCount(1)
        }
    }
    useEffect(() => {
        // 请求轮播图
        getSwiperApi().then(res => {
            // console.log('res', res);
            setSwiperArrFn(res.data.data)
        })
        //请求秒杀
        getSeckillApi().then(res => {
            // console.log(res);
            setSecKillArrFn(res.data.data)
        })
        // 请求产品列表数据
        // getProlistApi({ count, limitNum }).then(res => {
        //     console.log(res);
        //     setProListFn(res.data.data)
        // })
    }, [])
    return (
        <div className='home' style={{
            paddingBottom: '50px'
        }}>
            {/* 搜索框组件 */}
            <Mysearch></Mysearch>
            <PullToRefresh onRefresh={async () => {
                await sleep(1000)
                setProListFn([]) // 清空数据列表
                setHasMore(true) // 设置可以滚动
                loadMore()       // 调用加载下一页数据

            }}>
                {/* 轮播图组件 */}
                {
                    swiperArr.length > 0 ? <Myswiper swiperArr={swiperArr}></Myswiper> : null
                }
                {/* 导航菜单组件 */}
                <Mynavmenu></Mynavmenu>
                {/*京东秒杀  */}
                <Seckill seckillArr={seckillArr}></Seckill>
                {/* 菜单列表 */}
                <Productlist prolist={prolist}></Productlist>

                {/* 设置无线滚动 */}
                <InfiniteScroll loadMore={loadMore} hasMore={hasMore}>
                    <InfiniteScrollContent hasMore={hasMore} />
                </InfiniteScroll>
            </PullToRefresh>
        </div>
    );
}

export default Home;

```



## 12.登录页布局

```jsx
// src/views/login.jsx

import React from 'react';
// 引入头部导航条组件
import Mynavbar from '@/components/Mynavbar'
// 引入登录表单组件
import Myloginform from '@/components/Myloginform';
import { Card } from 'antd-mobile'
import { RightOutline } from 'antd-mobile-icons'
import { useHistory } from 'react-router-dom'
const Login = () => {
    const his = useHistory()
    // 去注册事件
    const onHeaderClick = () => {
        // 跳转到注册页
        his.push('/regist/step1')
    }
    return (
        <div className='loginbox'>
            {/* 头部导航条 */}
            <Mynavbar isshowright={false}>京东登录</Mynavbar>
            {/* 登录表单 */}
            <Myloginform></Myloginform>
            {/* 去注册 */}
            <Card

                title={
                    <div style={{ fontWeight: 'normal' }}>
                        快速注册
                    </div>
                }
                extra={<RightOutline />}
                onHeaderClick={onHeaderClick}
                style={{ borderRadius: '16px', width: '90%', margin: '0 auto' }}
            />
        </div>
    );
}

export default Login;

```

#### 1.定义登录表单组件

```jsx
// src/components/Myloginform.jsx
import React from 'react';
import {
    Form,
    Input,
    Button,
} from 'antd-mobile'
//引入接口
import { loginApi } from '@/api/login';
import { Toast } from 'antd-mobile'
import { useHistory } from 'react-router-dom'
import { useSelector, useDispatch } from 'react-redux'
import { updateUserAction } from '@/store/actions/userAction'
const Myloginform = () => {
    const his = useHistory()

    const dispatch = useDispatch()
    // 当提交表单的时候触发
    const onFinish = (values) => {
        console.log(values);
        // 调用登录接口
        loginApi(values).then(res => {
            console.log(res);
            if (res.data.code == 200) {
                //登录成功
                // 01: 提示
                // 02: 数据的存储
                // 03:跳转页面
                Toast.show({
                    icon: 'success',
                    content: res.data.message,
                    duration: 1000,
                    afterClose() {
                        his.push('/home')
                    }
                })

                localStorage.setItem('token', res.data.data.token)
                // 将userId 存放到redux
                dispatch(updateUserAction(res.data.data.userid))

            } else {
                //未登录成功提示
                Toast.show({
                    icon: 'fail',
                    content: res.data.message,
                    duration: 1000,
                })
            }

        })
    }
    return (
        <div>
            <Form
                layout='horizontal'
                onFinish={onFinish}
                footer={
                    <Button block type='submit' color='primary' size='large'>
                        提交
                    </Button>
                }
            >
                <Form.Item
                    name='loginname'
                    label='用户名'
                    rules={[{ required: true, message: '姓名不能为空' }]}
                >
                    <Input onChange={console.log} placeholder='请输入用户名' />
                </Form.Item>
                <Form.Item
                    name='password'
                    label='密码'
                    rules={[{ required: true, message: '密码不能为空' }]}
                >
                    <Input onChange={console.log} placeholder='请输入密码' />
                </Form.Item>
            </Form>

        </div>
    );
}

export default Myloginform;
```



## 13. 定义redux 仓库,存放登录数据

```js
// src/store/index.js

//定义仓库
import { legacy_createStore as createStore } from 'redux';
import userReducer from './modules/user';

// 数据持久化
import { persistStore, persistReducer } from "redux-persist";
import storage from 'redux-persist/lib/storage'

// 数据持久化:是指页面刷新后，数据仍然能够保持原来的状态
const persistConfig = {
    key: 'myroot',
    storage,
    // whitelist: ['CityReducers'] // 设置某个reducer数据持久化.whitelist:白名单；blacklist:黑名单
}

const persistedReducer = persistReducer(persistConfig, userReducer)


const store = createStore(persistedReducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
let persistor = persistStore(store)
export {
    persistor,
    store
}
```

定义user.js 模块化仓库

```js
// src/store/moudles/user.js

// 用户相关的state数据
const defaultState = {
    userId: ''
}

const userReducer = (state = defaultState, action) => {
    if (action.type == 'updateUserInfo') {
        return {
            ...state,
            userId: action.payload
        }
    }

    return state
}

export default userReducer
```

定义userActions.js

```js
// src/actions/userActions.js

// 定义操作仓库中的user相关的所有的action 任务

export function updateUserAction(data) {
    return { type: 'updateUserInfo', payload: data }
}
```



## 14.注册页布局

定义注册页的父组件,配置二级路由

```jsx
// src/views/Regist.jsx

import React, { lazy, Suspense } from 'react';
import { Switch, Route } from 'react-router-dom'
const Step1 = lazy(() => import('@/views/Step1'));
const Step2 = lazy(() => import('@/views/Step2'))
const Step3 = lazy(() => import('@/views/Step3'))
const Notfind = lazy(() => import('@/views/Notfind'))
const Regist = () => {
    return (
        <Suspense>
            <Switch>
                <Route path='/regist/step1' component={Step1}></Route>
                <Route path='/regist/step2' component={Step2}></Route>
                <Route path='/regist/step3' component={Step3}></Route>
                <Route component={Notfind}></Route>
            </Switch>
        </Suspense>
    );
}

export default Regist;

```

#### 1.注册步骤1

```jsx
// src/views/Step1.jsx

import React from 'react';
// 引入导航条组件
import Mynavbar from '@/components/Mynavbar';
// 引入接口
import { doCheckPhoneApi } from '@/api/regist'

// 引入antd
import { Modal } from 'antd-mobile'
import {
    Form,
    Input,
    Button,
} from 'antd-mobile';

import { useHistory } from 'react-router-dom'

const Step1 = () => {
    const his = useHistory()
    const onFinish = (value) => {
        //console.log(value)
        // 调用接口
        doCheckPhoneApi(value).then(res => {
            console.log(res);
            if (res.data.code == '10005') {
                // 已被注册
                Modal.confirm({
                    content: '账号已注册,是否去登录?',
                    onCancel() {
                        console.log('取消');
                    },
                    onConfirm() {
                        console.log('确定');
                        // 跳转到登录页
                        his.push('/login')
                    }
                })
            }
            if (res.data.code == 200) {
                //注册成功
                Modal.confirm({
                    content: `将向手机号${value.tel}发送验证码!`,
                    onCancel() {
                        //console.log('取消');
                    },
                    onConfirm() {

                        // 跳转到注册步骤2
                        his.push({
                            pathname: '/regist/step2',
                            state: {
                                tel: value.tel
                            }
                        })
                    }
                })
            }
        })
    }

    const checkMobile = (_, value) => {
        //console.log('value', value);
        if (/^1[3-9]\d{9}$/.test(value)) {
            return Promise.resolve()
        }
        return Promise.reject(new Error('手机号输入有误!'))
    }
    return (
        <div>
            <Mynavbar isshowright={false}>京东注册</Mynavbar>
            {/* 表单 */}
            <Form
                layout='horizontal'
                onFinish={onFinish}
                footer={
                    <Button block type='submit' color='primary' size='large'>
                        下一步
                    </Button>
                }
            >
                <Form.Item
                    name='tel'
                    label='手机号'
                    rules={[{ required: true, message: '手机号不能为空' }, { validator: checkMobile }]}
                >
                    <Input onChange={console.log} placeholder='请输入用户名' />
                </Form.Item>
            </Form>

        </div>
    );
}

export default Step1;

```

#### 2.注册步骤2

```jsx
// src/views/Step2.jsx
import React from 'react';
import {
    Form,
    Input,
    Button,
    Toast
} from 'antd-mobile';
// 引入导航条组件
import Mynavbar from '@/components/Mynavbar';
import { doSendMessageApi } from '@/api/regist';
import { useLocation, useHistory } from 'react-router-dom'
const Step2 = () => {
    const loc = useLocation();
    // console.log('loc', loc);
    const tel = loc.state.tel
    const his = useHistory()
    const onFinish = (value) => {
        // 跳转到step3 携带手机号
        his.push({
            pathname: '/regist/step3',
            state: {
                tel
            }
        })
    }

    return (
        <div>
            <Mynavbar isshowright={false}>京东注册</Mynavbar>
            {/* 表单 */}
            <Form
                layout='horizontal'
                onFinish={onFinish}
                footer={
                    <Button block type='submit' color='primary' size='large'>
                        下一步
                    </Button>
                }
            >
                <Form.Item
                    name='tel'
                    label='短信验证码'
                    rules={[{ required: true, message: '验证码不能为空' }]}
                    extra={<Button size='small' onClick={(e) => {
                        const currentDOM = e.target
                        // console.log('currentDOM', currentDOM)
                        let count = 10
                        currentDOM.disabled = true
                        let timer = setInterval(() => {
                            count--
                            currentDOM.innerHTML = `${count}s发送验证码`
                            if (count <= 0) {
                                currentDOM.innerHTML = '发送验证码'
                                currentDOM.disabled = false
                                clearInterval(timer)
                            }
                        }, 1000)
                        doSendMessageApi({ tel }).then(res => {
                            // console.log(res);
                            Toast.show({
                                content: res.data.message,
                                duration: 1000
                            })
                        })
                    }}>发送验证码</Button>}
                >
                    <Input onChange={console.log} placeholder='请输入用户名' />


                </Form.Item>
            </Form>
        </div>
    );
}

export default Step2;

```

3.注册步骤3

```jsx
// src/views/step3.jsx
import './App.scss';

// 引入路由规则组件
import Index from '@/router/Index';

// 引入tarbar组件
import Apptabbar from '@/components/Apptabbar';

import { useLocation } from 'react-router-dom'

function App() {
  // 将不显示tabbar 的路由地址存放到一个数组中, 然后每次切换路由的时候, 获取当前的路由地址
  // 判断当前的路由地址是否在 该数组中, 在的话,说明不显示, 否则正常显示即可
  const isNotShowtabbarArr = ['/login', '/detail', '/regist', '/cart'];
  const loc = useLocation();
  //console.log('loc', loc);
  const currentPathName = loc.pathname.split('/')[1]
  // console.log('currentPathName', currentPathName);

  return (
    <div className="App">
      {/* 使用路由规则和坑 */}
      <Index></Index>
      {/* 使用tabbar组件 */}
      {
        isNotShowtabbarArr.includes('/' + currentPathName) ? null : <Apptabbar></Apptabbar>
      }
    </div>
  );
}

export default App;

```



## 15.详情页

```jsx
// src/views/Detail.jsx
import React from 'react';
// 详情页组件
import { useParams, useHistory } from 'react-router-dom';
// 引入头部导航组件
import Mynavbar from '@/components/Mynavbar';
// 引入轮播图组件
import Myswiper from '@/components/Myswiper'
// 引入接口
import { getDetailApi, addCarApi, getCarListApi } from '@/api/detail'
import { useState, useEffect, useRef } from 'react'
// 引入antd
import { Button, Space, Mask, Badge, Modal } from 'antd-mobile'
// 引入图标
import { PlayOutline } from 'antd-mobile-icons'
// 引入scss 样式文件
import '@/assets/css/detail.scss';
import { useSelector } from 'react-redux'


const Detail = () => {
    const par = useParams();
    const { id } = par;
    // 定义详情数据
    const [detailObj, setDetailobj] = useState({});
    // 定义轮播图数组
    const [swiperArr, setSwiperArr] = useState([]);
    // 定义购物车类别
    const [carNum, setCarNumFn] = useState(0);

    const his = useHistory();
    async function getdata() {
        const res = await getDetailApi({ proid: id })
        // console.log(res)
        setDetailobj(res.data.data);
        const { img1, img2, img3, img4 } = res.data.data
        const swiperNewArr = [img1, img2, img3, img4].map((item, index) => ({
            bannerid: index,
            img: item
        }))

        //console.log('swiperNewArr', swiperNewArr);
        setSwiperArr(swiperNewArr)
    }
    // 定义遮罩层是否显示变量
    const [visible, setVisible] = useState(false)
    const [videoduration, setVideoDuration] = useState(0)
    // 创建ref 引用对象
    const ref1 = useRef()

    // 获取仓库中的数据
    const useId = useSelector(state => state.userId)
    //console.log('useId', useId);
    // 查询购物车数量
    const getCarlist = () => {
        getCarListApi({ userid: useId }).then(res => {
            console.log('res', res);
            setCarNumFn(res.data?.data?.length)
        })
    }

    useEffect(() => {
        getdata()
        // 获取video 标签

        const videoDOM = ref1.current
        // console.log('videoDOM', videoDOM);
        // 给video 标签绑定loadeddata或 canplay事件,表示浏览器已经加载完视频了就触发该事件
        videoDOM.oncanplay = () => {
            // 获取视频时长
            //console.log(videoDOM.duration);
            setVideoDuration(`00'${Math.ceil(document.querySelector('video').duration)}'`)
        }

        // 调用查询购物车接口
        getCarlist()
    }, [])


    return (
        <div className='detail'>
            {/* 头部导航组件 */}
            <Mynavbar>详情</Mynavbar>
            {/* 轮播图组件 */}
            {
                swiperArr.length > 0 ? <Myswiper swiperArr={swiperArr}></Myswiper> : null
            }
            {/* 商品详情数据 */}
            <p style={
                {
                    color: 'red',
                    fontSize: '20px'
                }
            }>${detailObj.originprice}</p>
            <div style={{
                fontSize: '20px',
                fontWeight: 800
            }}>{detailObj.proname}</div>
            {/* 播放按钮 */}
            <Button shape='rounded' size='small' className='playBtn' onClick={() => {
                // 显示遮罩层
                setVisible(true)
            }}>
                <Space>
                    <PlayOutline />
                    <span>{videoduration}</span>
                </Space>
            </Button>
            {/* 遮罩层布局 */}
            <video
                style={{
                    display: 'none'
                }}
                ref={ref1}
                preload='preload'
                src='https://jvod.300hu.com/vod/product/b1024a62-8c46-464c-8ec7-f57d26cceb38/42f70c8e8bbb442e986874ec556fc882.mp4'>
            </video>
            <Mask visible={visible} onMaskClick={() => setVisible(false)}>
                <div className='content'>
                    {visible ? <video
                        preload={'preload'}
                        controls='controls'
                        autoPlay
                        src='https://jvod.300hu.com/vod/product/b1024a62-8c46-464c-8ec7-f57d26cceb38/42f70c8e8bbb442e986874ec556fc882.mp4'>
                    </video> : null}

                </div>
            </Mask>
            {/* 加入购物车tabbar */}
            <Space className='addtabbarbox'>
                <Space style={{ '--gap': '24px' }}>
                    <div className='carbrage' onClick={() => {
                        // 先判断用户是否登录
                        if (localStorage.getItem('token')) {
                            his.push('/cart')
                        } else {
                            Modal.confirm({
                                content: '请先登录!',
                                onCancel() {

                                },
                                onConfirm() {
                                    his.push('/login')
                                }
                            })
                        }

                    }}>
                        <Badge content={carNum}>
                            <span className='iconfont icon-gouwuchekong'></span>
                        </Badge>
                        <span>购物车</span>
                    </div>
                </Space>
                <Button shape='rounded' color='primary' onClick={() => {
                    // 调用加入购物车接口
                    //console.log(detailObj)
                    if (localStorage.getItem('token')) {
                        addCarApi({
                            proid: detailObj.proid,
                            userid: useId,
                            num: 1
                        }).then(res => {
                            // console.log(res)
                            //重新获取购物车类别数量
                            getCarlist()
                        })
                    } else {
                        Modal.confirm({
                            content: '请先登录!',
                            onCancel() {

                            },
                            onConfirm() {
                                his.push('/login')
                            }
                        })
                    }

                }}>加入购物车</Button>
                <Button shape='rounded' color='danger'>立即购买</Button>
            </Space>

        </div>
    );
}

export default Detail;

```

## 16.购物车页面

```jsx
// src/views/Cart.jsx

import React from 'react';
import Mynavbar from '@/components/Mynavbar';
import { useEffect, useState, useRef } from 'react';
import { useSelector } from 'react-redux';
import { getCarListApi } from '@/api/detail'
import { useCallback, useMemo } from 'react';
import {
    Image,
    List,
    Stepper,
    Ellipsis,
    Empty,
    Button,
    Space,
    Checkbox,
    Dialog,
    SwipeAction
} from 'antd-mobile';
import { useHistory } from 'react-router-dom'

import {
    updateCheckedAllApi,
    updateCheckedOneApi,
    updateOneNumApi,
    deleteOneApi
} from '@/api/cart'
// import { Action, SwipeActionRef } from 'antd-mobile/es/components/swipe-action'
const Cart = () => {
    // 获取仓库数据
    const userId = useSelector(state => state.userId);
    // console.log('userId', userId);
    const [carNum, setCarNum] = useState(0);
    const [cartlist, setCartList] = useState([]);
    const his = useHistory();

    // 定义全选变量
    const [checkedAll, setCheckedAllFn] = useState(false)
    const getcartlist = useCallback(() => {
        getCarListApi({ userid: userId }).then(res => {
            // console.log(res);
            if (res.data.code == '10020') {
                setCarNum(0);
                setCartList([]);
                setCheckedAllFn(false);
            } else {
                setCartList(res.data.data)
                setCarNum(res.data?.data?.length)
                // console.log(res.data.data.every((item) => item.flag));
                setCheckedAllFn(res.data.data.every((item) => item.flag))
            }
        })
    }, [])
    useEffect(() => {
        getcartlist();
    }, [])

    // const ref = useRef < SwipeActionRef > (null)
    // 使用计算属性 计算商品总价
    // 语法:const 变量=  useMemo(()=>{return 值1},[依赖的变量]) 值1就是变量的值
    const arr = [1, 2, 3, 4, 5];
    // const total = arr.reduce((sum, item) => { return sum += item }, 0)
    //console.log('total', total);
    const totalPrice = useMemo(() => {
        return cartlist.reduce((sum, item) => { return item.flag ? sum += item.originprice * item.num : sum }, 0)
    }, [cartlist])
    return (
        <div>
            {/* 头部导航 */}
            <Mynavbar isshowright={false}>{`购物车(${carNum})`}</Mynavbar>
            {/* 购物车列表 */}
            {
                cartlist.length == 0 ? <Empty
                    style={{ padding: '64px 0' }}
                    imageStyle={{ width: 128 }}
                    description={
                        <div>
                            <p style={{
                                textAlign: 'center',
                                marginBottom: '10px'
                            }}>{'暂无数据'}</p>
                            <Button color='danger' size='small' onClick={() => {
                                // 跳转到首页
                                his.push('/home')
                            }}>立即购物</Button>
                        </div>
                    }
                /> :
                    <List>
                        {cartlist.map(item => (
                            <SwipeAction
                                key={item.cartid}
                                rightActions={[
                                    {
                                        key: 'delete',
                                        text: '删除',
                                        color: 'danger',
                                        onClick: async () => {
                                            await Dialog.confirm({
                                                content: '确定要删除吗？',
                                            })

                                            //console.log('123')
                                            // ref.current?.close()
                                            deleteOneApi({ cartid: item.cartid }).then(res => {
                                                console.log(res)
                                                getcartlist()
                                            })
                                        },
                                    }
                                ]}
                            >
                                <List.Item
                                    key={item.cartid}
                                    prefix={
                                        <Space align='center'>
                                            <div onClick={e => e.stopPropagation()}>
                                                <Checkbox checked={item.flag} onChange={(boolean) => {
                                                    console.log(boolean)
                                                    updateCheckedOneApi({
                                                        cartid: item.cartid,
                                                        flag: boolean
                                                    }).then(res => {
                                                        console.log(res)
                                                        getcartlist()
                                                    })
                                                }} />
                                            </div>
                                            <Image
                                                src={item.img1}
                                                fit='cover'
                                                width={60}
                                                height={60}
                                            />
                                        </Space>

                                    }
                                    description={
                                        <div style={{
                                            display: 'flex',
                                            justifyContent: 'space-between',
                                            alignItems: 'center'
                                        }}>
                                            <p>{item.originprice}</p>
                                            {/* 步进器 */}
                                            <Stepper
                                                value={item.num}
                                                onChange={value => {
                                                    console.log(value)
                                                    if (value == 0) {
                                                        deleteOneApi({ cartid: item.cartid }).then(res => {
                                                            console.log(res)
                                                            getcartlist()
                                                        })
                                                    } else {
                                                        updateOneNumApi({
                                                            cartid: item.cartid,
                                                            num: value
                                                        }).then(res => {
                                                            console.log(res)
                                                            getcartlist()
                                                        })
                                                    }

                                                }}
                                            />
                                        </div>
                                    }
                                >
                                    <div >
                                        <Ellipsis
                                            direction='end'
                                            rows={2}
                                            style={{
                                                fontSize: '14px'
                                            }}
                                            content={item.proname}
                                        />
                                    </div>
                                </List.Item>
                            </SwipeAction>
                        ))}
                    </List>
            }
            {/* 底部tabbar 组件 */}
            <div style={{
                position: 'fixed',
                left: 0,
                bottom: 0,
                display: 'flex',
                justifyContent: 'space-around',
                width: '100%',
                alignItems: 'center'
            }}>
                <Checkbox checked={checkedAll} onChange={(bool) => {
                    //调用接口
                    updateCheckedAllApi({ type: bool, userid: userId }).then(res => {
                        // console.log(res)
                        getcartlist()
                    })
                }}>全选</Checkbox>
                <p>总价:{totalPrice.toFixed(2)}元</p>
                <Button color='danger'>合计</Button>
            </div>

        </div>
    );
}

export default Cart;

```



## 17. redux 持久化配置

首先需要下载插件

 ```shell
npm i  redux-persist --save
 ```

store/index.js 中代码

```js
// 创建store 仓库
import { legacy_createStore as createStore } from 'redux';

// +++++++++
import { persistStore, persistReducer } from 'redux-persist';
//+++++
import storage from 'redux-persist/lib/storage'

// 定义默认state数据
const defaultState = {
    userId: ''  //用户信息
}

// 创建reducer
const reducer = (state = defaultState, actions) => {
    if (actions.type == 'setuser') {
        return {
            ...state,
            userId: actions.payload
        }
    }
    return state
};


//+++++++++在localStorge中生成key为root的值
const persistConfig = {
    key: 'root',
    storage,
    blacklist: ['loading']  //设置某个reducer数据不持久化
}
//++++++++
const myPersistReducer = persistReducer(persistConfig, reducer)

//+++
const store = createStore(myPersistReducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
// ++++
const persistor = persistStore(store)
//+++
export {
    store,
    persistor
}
```

项目入口文件需要配置 index.js

```js
// 导入store仓库
//++++
import { store, persistor } from './store/index'
import { Provider } from 'react-redux'

//++++
import { PersistGate } from 'redux-persist/integration/react'
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
	
    <Router>
    // ++++++
      <PersistGate loading={null} persistor={persistor}>
      <App />
      </PersistGate>
    </Router>

  </Provider>

);
```





































































































































13: RTK 的持久化配置**

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

