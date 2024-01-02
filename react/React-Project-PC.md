React综合案例

# 一、概要

项目的接口地址: http://47.94.148.165:3001/admindoc

项目参考地址:  http://47.94.148.165/

2220班级项目录屏: 链接：https://pan.baidu.com/s/1G5Cq_N-rprhPAbJZzs2gNA?pwd=j72e 


## 1、开发背景 

因公司某项目的业务数据管理需要，公司决定安排开发人员组成项目小组，为该项目开发一个后台管理系统，实现该项目日常业务数据的展示和维护。 【**切勿直接写增删改查**】

## 2、技术栈

使用react框架来完成本次项目的实现，采用前后端分离式开发，使用前端技术有如下一些：

- react：主框架  react18 .2

- react-router-dom@6：路由包

- rtk：大规模状态管理库

- react-redux：给redux做强化

- styled-components ：css-in-js技术的实现

- antd：前端组件库（ant design）

- axios：网络请求库
- ……

后端技术有：

- PHP/JAVA/Go/Python/NodeJS：后端语言
- MySQL/Oracle：数据库软件 (关系型数据库)
- Redis：数据库软件 (非关系型数据库)
- Laravel(php)：后端框架  Spring(JAVA)
- ......

## 3、开发环境

开发环境：Windows

开发工具 IDE：vscode + jsx插件 

开发调试工具：chrome浏览器

开发运行环境：node环境>= v14

上线环境为：linux(centos/unbetu)+ nginx + git

## 4、开发准备

创建项目:

create-react-app 官网地址: https://create-react-app.bootcss.com/docs/getting-started

```shell
npx create-react-app my-app --template typescript
```

需要安装的包有 :

**状态管理包:** 

react, react-redux  redux-thunk  rtk  react-router-dom@6

```shell
npm install @reduxjs/toolkit react-redux redux 
```

**路由包:**

```shell
npm install react-router-dom@6
```

**数据请求**

以前的版本 需要安装 npm i axios

```
npm i axios
```

**引入UI 组件库**

```
 npm install antd --save
```

**引入 css-in-js 技术**

```shell
npm i styled-components 
```

 ```shell
cnpm i --save-dev @types/styled-components   // 安装类型声明文件
 ```

**对create-react-app 脚手架进行配置,达到覆盖修改脚手架的默认webpack配置**

参考地址: https://www.npmjs.com/package/@craco/craco;

```
npm i -D @craco/craco
```

在项目的根目录创建一个  craco.config.ts 文件, 修改`package.json`中的脚本命令为如下：

```js
 const path = require('path');
 module.exports = {
     webpack: {
         alias: {
             "@": path.resolve(__dirname, 'src')
         }
     }
 }
```

 如果如上报错的话, 可能会报错, 需要安装 npm i @types/node -D 这个声明文件.同时保证项目在vscode编辑器工作区的根目录下, 如果运行项目, 报错如下, 则需要  **找到tsconfig.json配置文件，把isolatedModules字段改为false** 

 如上修改完webpack配置,需要修改package.json中的一些脚本命令配置,这样如上修改的配置才能生效. 将react-scripts 替换成 如下

 > 参考来源：https://www.npmjs.com/package/@craco/craco

 ![1671980571549](C:\Users\wusheng\AppData\Roaming\Typora\typora-user-images\1671980571549.png)

注意: 配置路径别名 还需要再 项目的根目录 中的 ts.config.json 文件中 做一下相关别名配置

```json
{
  "compilerOptions": {
   ...
    "baseUrl": "./",  //添加baseUrl
    "paths": {        // 添加 paths 路径配置别名@
      "@/*": [
        "src/*"
      ]
    }
   ...
}
```



**自定义主题**

在入口文件index.tsx  中定义主题

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { Button, ConfigProvider } from 'antd';
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLDivElement
);
root.render(
  <ConfigProvider
    theme={{
      token: {
        colorPrimary: '#00b96b', // 自定义的主题色
      },
    }}
  >
    <App />
  </ConfigProvider>

);

```

**使用第三方的工具包** 

在ts中使用需要下载它的变量的类型声明文件

```shell
cnpm i --save lodash 
cnpm i --save-dev @types/lodash
```

使用 loadsh 

`````js
import _ from 'lodash'
console.log(_.chunk(['a', 'b', 'c', 'd'], 2));
`````

 **安装sass**  

create-react-app 脚手架支持sass , 所以只需要安装 sass 直接使用就可以了.

参考地址: https://create-react-app.bootcss.com/docs/adding-a-sass-stylesheet

```
$ npm install sass
# 或者
$ yarn add sass
```

 现在，你可以将 `src/App.css` 重命名为 `src/App.scss`，并更新 `src/App.js` 以引入 `src/App.scss`。 如果以扩展名 `.scss` 或 `.sass` 引入，则此文件和其他文件都将会自动编译。 





## 5. 定义初始路由

```shell
npm i react-router-dom
```

配置路由模式

```tsx
// 入口文件 index.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { ConfigProvider } from 'antd';
import { Provider } from 'react-redux'
import store from '@/store/index'

import { BrowserRouter } from 'react-router-dom'

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLDivElement
);
root.render(
  <ConfigProvider
    theme={{
      token: {
        colorPrimary: '#00b96b',
      },
    }}
  >
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  </ConfigProvider>
);

```

创建基础组件

src/views/Login.tsx
src/views/layout/Index.tsx

```tsx
// src/App.tsx 根组件
import React from 'react';
import { Routes, Route } from 'react-router-dom';
import Login from '@/views/Login'
import Layout from '@/views/layout/Index'
type Props = {}

export default function App({ }: Props) {
  return (
    <Routes>
      <Route path='/login' element={<Login></Login>}></Route>
      <Route path='/*' element={<Layout></Layout>}></Route>
    </Routes>
  )
}
```



## 6.项目首页布局

参考地址: https://ant.design/components/layout-cn
在src/layout/Index.tsx 制作首页的基础布局

```tsx
// src/layout/Index.tsx 

import React, { useState } from 'react';
import '@/App.css';

// 使用redux
import { useAppDispatch, useAppSelector } from '@/store/hooks/index';
import { add } from '@/store/slices/counterSlice'
import routes from '@/router/Index';
import { useRoutes } from 'react-router-dom'
import {
    MenuFoldOutlined,
    MenuUnfoldOutlined,
    UploadOutlined,
    UserOutlined,
    VideoCameraOutlined,
} from '@ant-design/icons';
import { Layout, Menu, Button, theme } from 'antd';

const { Header, Sider, Content } = Layout;

const App: React.FC = () => {

    const Store = useAppSelector(state => state)
    console.log('Store', Store);
    const dispatch = useAppDispatch();
    console.log(useAppDispatch);

    const [collapsed, setCollapsed] = useState(false);
    const {
        token: { colorBgContainer },
    } = theme.useToken();

    const ele = useRoutes(routes);
    return (
        <Layout>
            <Sider trigger={null} collapsible collapsed={collapsed}>
                <div className="demo-logo-vertical" />
                <Menu
                    theme="dark"
                    mode="inline"
                    defaultSelectedKeys={['1']}
                    items={[
                        {
                            key: '1',
                            icon: <UserOutlined />,
                            label: 'nav 1',
                        },
                        {
                            key: '2',
                            icon: <VideoCameraOutlined />,
                            label: 'nav 2',
                        },
                        {
                            key: '3',
                            icon: <UploadOutlined />,
                            label: 'nav 3',
                        },
                    ]}
                />
            </Sider>
            <Layout>
                <Header style={{ padding: 0, background: colorBgContainer }}>
                    <Button
                        type="text"
                        icon={collapsed ? <MenuUnfoldOutlined /> : <MenuFoldOutlined />}
                        onClick={() => setCollapsed(!collapsed)}
                        style={{
                            fontSize: '16px',
                            width: 64,
                            height: 64,
                        }}
                    />
                </Header>
                <Content
                    style={{
                        margin: '24px 16px',
                        padding: 24,
                        minHeight: 280,
                        background: colorBgContainer,
                    }}
                >
                    Content
                    {ele}
                    <p onClick={() => {

                        dispatch(add(10))
                        //console.log(dispatch);


                    }}>count:{Store.counter.value}</p>
                </Content>
            </Layout>
        </Layout>
    );
};

export default App;
```



## 7.将layout 组件中拆分

将 /src/views/layout/Index.tsx 组件的 Aside . Header , content部分 拆分成不同的组件

**Appaside组件**

```tsx
// src/components/Appaside.tsx
import React, { useState } from 'react'
import {
    UploadOutlined,
    UserOutlined,
    VideoCameraOutlined,
} from '@ant-design/icons';
import { Layout, Menu, } from 'antd';
// 使用兄弟组件通信events
import Bus from '@/utils/events';
import { useEffect } from 'react'
const { Sider } = Layout;
type Props = {}

export default function Appaside({ }: Props) {
    const [collapsed, setCollapsed] = useState(false)
    useEffect(() => {
        // 使用兄弟组件通信events,接收数据
        Bus.on("myevent", (data) => {
            setCollapsed(data)
        });
    })

    return (
        <Sider trigger={null} collapsible collapsed={collapsed}>
            <div className="demo-logo-vertical" />
            <Menu
                theme="dark"
                mode="inline"
                defaultSelectedKeys={['1']}
                items={[
                    {
                        key: '1',
                        icon: <UserOutlined />,
                        label: 'nav 1',
                    },
                    {
                        key: '2',
                        icon: <VideoCameraOutlined />,
                        label: 'nav 2',
                    },
                    {
                        key: '3',
                        icon: <UploadOutlined />,
                        label: 'nav 3',
                    },
                ]}
            />
        </Sider>
    )
}
```

**Appheader.tsx组件;**

```tsx
// src/components/Appheader.tsx

import React, { useEffect } from 'react'
import {
    MenuFoldOutlined,
    MenuUnfoldOutlined
} from '@ant-design/icons';
import { Layout, Button, theme } from 'antd';
import { useState } from 'react'
// 兄弟组件通信
import Bus from "@/utils/events"
const { Header } = Layout;

type Props = {};

export default function Appheader({ }: Props) {
    const [collapsed, setCollapsed] = useState(false);

    const {
        token: { colorBgContainer },
    } = theme.useToken();

    // 监听collapse 发生变化,立即获取最新的值 
    useEffect(() => {
        //兄弟组件通信发送数据
        Bus.emit('myevent', collapsed)
    }, [collapsed])
    return <Header style={{ padding: 0, background: colorBgContainer }}>
        <Button
            type="text"
            icon={collapsed ? <MenuUnfoldOutlined /> : <MenuFoldOutlined />}
            onClick={() => {
                setCollapsed(!collapsed)
            }}
            style={{
                fontSize: '16px',
                width: 64,
                height: 64,
            }}
        />
    </Header>
}
```

**Appmain.tsx组件**

```tsx
// src/components/Appmain.tsx

import React from 'react'
import {
    MenuFoldOutlined,
    MenuUnfoldOutlined,
    UploadOutlined,
    UserOutlined,
    VideoCameraOutlined,
} from '@ant-design/icons';
import { Layout, Button, theme } from 'antd';

import { useRoutes } from 'react-router-dom'
import routes from '@/router/Index';
const { Content } = Layout;
type Props = {}

export default function Appmain({ }: Props) {
    const {
        token: { colorBgContainer },
    } = theme.useToken();

    const ele = useRoutes(routes);
    return (
        <Content
            style={{
                margin: '24px 16px',
                padding: 24,
                minHeight: 280,
                background: colorBgContainer,
            }}
        >
            Content
            {ele}
        </Content>
    )
}
```

**src/layout/Index.tsx**

```tsx
//src/layout/Index.tsx

import React from 'react';
import '@/App.css';
import { Layout } from 'antd';

import Appaside from '@/components/Appaside';
import Appheader from '@/components/Appheader';
import Appmain from '@/components/Appmain';
const App: React.FC = () => {

    return (
        <Layout>
            {/* 侧边栏组件 */}
            <Appaside></Appaside>
            <Layout>
                {/* header组件 */}
                <Appheader></Appheader>
                {/* 主体部分组件 */}
                <Appmain></Appmain>
            </Layout>
        </Layout>
    );
};

export default App;
```



## 8.定义首页路由

在src/router/routes.tsx 文件中定义路由规则

```tsx
// src/router/routes.tsx

import React, { lazy } from 'react'
import { Navigate } from 'react-router-dom';
// 系统首页组件
const Home = lazy(() => import('@/views/layout/home/Home'));
const Notfind = lazy(() => import('@/views/Notfind'));
// 轮播图组件
const Banner = lazy(() => import('@/views/layout/banner/Index'));
const Bannerlist = lazy(() => import('@/views/layout/banner/Bannerlist'));
// 产品相关组件
const Product = lazy(() => import('@/views/layout/product/Index'));
const Productlist = lazy(() => import('@/views/layout/product/Productlist'));
const Productsearch = lazy(() => import('@/views/layout/product/Productsearch'));
const Productrecommend = lazy(() => import('@/views/layout/product/Productrecommend'))
const Productseckill = lazy(() => import('@/views/layout/product/Productseckill'))
// 账户相关组件
const Account = lazy(() => import('@/views/layout/account/Index'))
const Adminlist = lazy(() => import('@/views/layout/account/Adminlist'))
// 系统设置
const Setting = lazy(() => import('@/views/layout/setting/Index'))

const routes = [
    {
        path: '/home',
        element: <Home />,
        label: '系统首页',
    },
    {
        path: '/banner',
        element: <Banner></Banner>,
        label: '轮播图管理',
        children: [
            {
                path: '/banner/list',
                element: <Bannerlist></Bannerlist>,
                label: '轮播图列表'
            }
        ]
    },
    {
        path: '/product',
        element: <Product></Product>,
        label: '产品管理',
        children: [
            {
                path: '/product/list',
                element: <Productlist></Productlist>,
                label: '产品列表',
            },
            {
                path: '/product/search',
                element: <Productsearch />,
                label: '产品筛选',
            },
            {
                path: '/product/seckill',
                element: <Productseckill />,
                label: '秒杀列表',
            },
            {
                path: '/product/search',
                element: <Productrecommend />,
                label: '推荐列表',
            }
        ]
    },
    {
        path: '/account',
        element: <Account></Account>,
        label: '账户管理',
        children: [
            {
                path: '/account/adminlist',
                element: <Adminlist></Adminlist>,
                label: '管理员列表',
            }
        ]
    },
    {
        path: '/setting',
        element: <Setting></Setting>,
        label: '系统设置',
    },
    {
        path: '*',  // 定义404 路由
        element: <Notfind></Notfind>
    }
]

export default routes
```

将路由引入到Appmain.tsx 组件中使用

```tsx
// src/components/Appmain.tsx
import React from 'react'
import {
    MenuFoldOutlined,
    MenuUnfoldOutlined,
    UploadOutlined,
    UserOutlined,
    VideoCameraOutlined,
} from '@ant-design/icons';
import { Layout, Button, theme } from 'antd';

import { useRoutes } from 'react-router-dom'
// 引入路由表
import routes from '@/router/routes';
const { Content } = Layout;
type Props = {}

export default function Appmain({ }: Props) {
    const {
        token: { colorBgContainer },
    } = theme.useToken();
	// 使用路由表创建组件
    const ele = useRoutes(routes);
    return (
        <Content
            style={{
                margin: '24px 16px',
                padding: 24,
                minHeight: 280,
                background: colorBgContainer,
            }}
        >
            {/*使用路由表坑组件*/}
            {ele}
        </Content>
    )
}
```

同时需要注意要在一级路由对应的组件中 使用Outlet 组件, 以显示二级路由组件

```tsx
//例如: src/views/layout/account/Index  二级组件

import React from 'react';
import { Outlet } from 'react-router-dom';
type Props = {}

export default function Index({ }: Props) {
    return (
        // 给三级组件留的坑,用来显示三级组件
        <Outlet></Outlet>
    )
}
```



## 9.封装Loading 组件

使用Suspense 组件配合Lazy懒加载方式,单独使用lazy方法引入组件会报错

```tsx
//src/components/Loading.tsx
import React from 'react'
import { Spin } from 'antd';

//引入 styled-components
import styled from 'styled-components';
const Center = styled.div`
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
`

type Props = {
    tit?: string
};
export default function Loading(props: Props) {
    const tip = props.tit ? props.tit : '加载中...'
    return (
        <Center>
            <Spin tip={tip} size="large">
                <div style={
                    {
                        padding: '50px',
                        borderRadius: "4px"
                    }
                } />
            </Spin>
        </Center>

    )
}
```

在src/components/Appmain组件中使用 Loading 组件

```tsx
// src/components/Appmain.tsx

import React from 'react';
import { Layout, theme } from 'antd';
// 引入路由routes 数组
import routes from '@/router/routes';
// 引入useRoutes hook
import { useRoutes } from 'react-router-dom'
// 引入suspense 组件, 用来显示loading 效果,必须配合lazy 使用+++++++++++++
import { Suspense } from 'react'
// 引入loading组件++++++++++++++++++
import Loading from '@/components/Loading'
const { Content } = Layout;
type Props = {}

export default function Appmain({ }: Props) {
    const {
        token: { colorBgContainer },
    } = theme.useToken();

    const element = useRoutes(routes);
    return (
        <Content
            style={{
                margin: '24px 16px',
                padding: 24,
                minHeight: 280,
                background: colorBgContainer,
            }}
        >
            {/* 二级路由匹配组件显示在这里++++++++++++++ */}
            {/* Suspense当路由匹配的组件还没加载出来之前显示fallback中的组件 */}
            <Suspense fallback={<Loading></Loading>}>
                {element}
            </Suspense>
        </Content>
    )
}
```



## 10.渲染左侧菜单栏组件

将 src/components/Appaside.tsx 组件中的menu 组件抽离成Appmenu.tsx

```tsx
// src/components/Appaside.tsx
import React, { useState } from 'react'

import { Layout, } from 'antd';
import Bus from '@/utils/events';
import { useEffect } from 'react';
import Appmenu from './Appmenu';
const { Sider } = Layout;
type Props = {}

export default function Appaside({ }: Props) {
    const [collapsed, setCollapsed] = useState(false)
    useEffect(() => {
        Bus.on("myevent", (data) => {
            setCollapsed(data)
        });
    })

    return (
        <Sider trigger={null} collapsible collapsed={collapsed}>
            <div className="demo-logo-vertical" />
            {/* 左侧菜单组件 */}
            <Appmenu></Appmenu>
        </Sider>
    )
}

```

在components组件中定义Appmenu.tsx 组件

```tsx
// src/components/Appmenu.tsx
import React from 'react'
import {
    UploadOutlined,
    UserOutlined,
    VideoCameraOutlined,
} from '@ant-design/icons';
import { Menu } from 'antd';
import type { MenuProps } from 'antd';
import { NavLink } from 'react-router-dom';
type MenuItem = Required<MenuProps>['items'][number];

type Props = {}
function getItem(
    label: React.ReactNode,
    key: React.Key,
    icon?: React.ReactNode,
    children?: MenuItem[],
    type?: 'group',
): MenuItem {
    return {
        key,
        icon,
        children,
        label,
        type,
    } as MenuItem;
}

// 根据路由routes路由表定义菜单数组
const items: MenuItem[] = [
    getItem(<NavLink to='/home'>系统首页</NavLink>, '/home', <UploadOutlined />),
    getItem(<NavLink to='/banner'>轮播图管理</NavLink>, '/banner', <UploadOutlined />, [
        getItem(<NavLink to='/banner/list'>轮播图列表</NavLink>, '/banner/list', <UploadOutlined />)
    ]),
    getItem(<NavLink to='/product'>产品管理</NavLink>, '/product', <UploadOutlined />, [
        getItem(<NavLink to='/product/list'>产品列表</NavLink>, '/product/list', <UploadOutlined />),
        getItem(<NavLink to='/product/search'>筛选列表</NavLink>, '/product/search', <UploadOutlined />),
        getItem(<NavLink to='/product/seckill'>秒杀列表</NavLink>, '/product/seckill', <UploadOutlined />),
        getItem(<NavLink to='/product/recommend'>推荐列表</NavLink>, '/product/recommend', <UploadOutlined />)
    ]),
    getItem(<NavLink to='/account'>账户管理</NavLink>, '/account', <UploadOutlined />, [
        getItem(<NavLink to='/account/adminlist'>管理员列表</NavLink>, '/account/adminlist', <UploadOutlined />)
    ]),
    getItem(<NavLink to='/setting'>系统设置</NavLink>, '/setting', <UploadOutlined />),
]

export default function Appmenu({ }: Props) {
    return (
        <Menu
            theme="dark"
            mode="inline"
            defaultSelectedKeys={['1']}
            items={items}
        />
    )
}
```

修改src/components/Appmain.tsx组件

```tsx
// src/components/Appmain.tsx

//引入 Suspense 组件,配合懒加载使用,否则单独使用懒加载报错
import React, { Suspense } from 'react'
import { Layout, theme } from 'antd';
import { useRoutes } from 'react-router-dom'
import routes from '@/router/routes';
const { Content } = Layout;
type Props = {}

export default function Appmain({ }: Props) {
    const {
        token: { colorBgContainer },
    } = theme.useToken();

    const ele = useRoutes(routes);
    return (
        <Content
            style={{
                margin: '24px 16px',
                padding: 24,
                minHeight: 280,
                background: colorBgContainer,
            }}
        >
            {/* 引入 Suspense 组件,配合懒加载使用*/}  
            <Suspense fallback={<>加载中。。。</>}>
                {ele}
            </Suspense>
        </Content>
    )
}
```



10.优化左侧菜单栏渲染

使用routes数组遍历渲染,同时需要给routes数组中添加一些额外的属性 label, key, icon .....

```tsx
// src/router/routes.tsx
import React, { lazy } from 'react'
import { Navigate } from 'react-router-dom';
const Home = lazy(() => import('@/views/layout/home/Home'));
const Notfind = lazy(() => import('@/views/Notfind'));

const Banner = lazy(() => import('@/views/layout/banner/Index'));
const Bannerlist = lazy(() => import('@/views/layout/banner/Bannerlist'));

const Product = lazy(() => import('@/views/layout/product/Index'));
const Productlist = lazy(() => import('@/views/layout/product/Productlist'));
const Productsearch = lazy(() => import('@/views/layout/product/Productsearch'));
const Productrecommend = lazy(() => import('@/views/layout/product/Productrecommend'))
const Productseckill = lazy(() => import('@/views/layout/product/Productseckill'))

const Account = lazy(() => import('@/views/layout/account/Index'))
const Adminlist = lazy(() => import('@/views/layout/account/Adminlist'))

const Setting = lazy(() => import('@/views/layout/setting/Index'))

const routes = [
    {
        path: '/home',
        element: <Home />,
        label: '系统首页',
        key: '/home'
    },
    {
        path: '/banner',
        element: <Banner></Banner>,
        label: '轮播图管理',
        key: '/banner',
        children: [
            {
                path: '/banner/list',
                element: <Bannerlist></Bannerlist>,
                label: '轮播图列表',
                key: '/banner/list',
            }
        ]
    },
    {
        path: '/product',
        element: <Product></Product>,
        label: '产品管理',
        key: '/product',
        children: [
            {
                path: '/product/list',
                element: <Productlist></Productlist>,
                label: '产品列表',
                key: '/product/list',
            },
            {
                path: '/product/search',
                element: <Productsearch />,
                label: '产品筛选',
                key: '/product/search',
            },
            {
                path: '/product/seckill',
                element: <Productseckill />,
                label: '秒杀列表',
                key: '/product/seckill',
            },
            {
                path: '/product/recommend',
                element: <Productrecommend />,
                label: '推荐列表',
                key: '/product/recommend',
            }
        ]
    },
    {
        path: '/account',
        element: <Account></Account>,
        label: '账户管理',
        key: '/account',
        children: [
            {
                path: '/account/adminlist',
                element: <Adminlist></Adminlist>,
                label: '管理员列表',
                key: '/account/adminlist',
            }
        ]
    },
    {
        path: '/setting',
        element: <Setting></Setting>,
        label: '系统设置',
        key: '/setting',
    },
    {
        path: '*',  // 定义404 路由
        element: <Notfind></Notfind>
    }
]


export default routes
```

使用routes 渲染左侧菜单栏

```tsx
// src/components/Appmenu.tsx

import React from 'react'
import {
    UploadOutlined,
    UserOutlined,
    VideoCameraOutlined,
} from '@ant-design/icons';
import { Menu } from 'antd';
import type { MenuProps } from 'antd';
import { NavLink } from 'react-router-dom';
import routes from '@/router/routes'
type MenuItem = Required<MenuProps>['items'][number];

type Props = {}
function getItem(
    label?: React.ReactNode,
    key?: React.Key,
    icon?: React.ReactNode,
    children?: MenuItem[],
    type?: 'group',
): MenuItem {
    return {
        key,
        icon,
        children,
        label,
        type,
    } as MenuItem;
}

// 重新渲染使用routes 路由数组遍历渲染
const items: MenuItem[] = routes.map((item, index) => {
    if (item.label) {
        if (item.key) {
            if (item.children) {
                return getItem(<NavLink to={item.path}>{item.label}</NavLink>, item.key, <UploadOutlined />, item.children.map(child => {
                    return getItem(<NavLink to={child.path}>{child.label}</NavLink>, child.key, <UploadOutlined />)
                }))
            } else {
                return getItem(<NavLink to={item.path}>{item.label}</NavLink>, item.key, <UploadOutlined />)
            }
        } else {
            return null
        }
    } else {
        return null
    }
})
export default function Appmenu({ }: Props) {
    return (
        <Menu
            theme="dark"
            mode="inline"
            defaultSelectedKeys={['1']}
            items={items}
        />
    )
}
```



## 11.定义全局状态管理工具

RTK ts语法格式:  https://www.reduxjs.cn/tutorials/typescript-quick-start

```ts
// src/store/index.ts

import { configureStore } from '@reduxjs/toolkit'
// ...
import appReducer from './modules/appSlice'
const store = configureStore({
    reducer: {
        app: appReducer,

    }
})

// 从 store 本身推断出 `RootState` 和 `AppDispatch` 类型
export type RootState = ReturnType<typeof store.getState>
// 推断出类型: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch


export default store
```

```ts
// src/store/hooks

import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './index';

// 在整个应用程序中使用，而不是简单的 `useDispatch` 和 `useSelector`
// 给useDispatch 和useSelector 重新定义类型, 这样后面使用useDispatch 和 useSelector 时
// 就是用如下的 useAppDispatch 和 useAppSelector,因为不需要再每个使用到该hook的组件中重新定义
// 这两个hook 的数据类型啦
export const useAppDispatch: () => AppDispatch = useDispatch
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
```

```ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit'
import type { RootState } from '../index'

// 为 slice state 定义一个类型
interface CounterState {
    value: number,
    collapsed: boolean
}

// 使用该类型定义初始 state
const initialState: CounterState = {
    value: 0,
    // collapsed: false  // 刷新数据还原
    collapsed: localStorage.getItem('collapsed') == 'true'  // 刷新数据不还原
}

export const appSlice = createSlice({
    name: 'app',
    // `createSlice` 将从 `initialState` 参数推断 state 类型
    initialState,
    reducers: {
        increment: state => {
            state.value += 1
        },
        // 使用 PayloadAction 类型声明 `action.payload` 的内容
        setcollapsed: (state, action: PayloadAction<boolean>) => {
            state.collapsed = action.payload
            localStorage.setItem('collapsed', String(action.payload)) //  // 刷新数据不还原
        }
    }
})

export const { increment, setcollapsed } = appSlice.actions

export default appSlice.reducer
```



## 12. 面包屑导航组件

在src/components/Appbreadcrumb.tsx 编写面包屑导航组件代码

参考面包屑导航: https://ant.design/components/breadcrumb-cn  中  结合[react-router V6](https://ant.design/components/breadcrumb-cn#components-breadcrumb-demo-react-router)  部分代码

```tsx
// src/components/Appbreadcrumb.tsx

import React from 'react'
import { Breadcrumb } from 'antd';
import { useLocation, Link } from 'react-router-dom';
import routes from '@/router/routes'
type Props = {}

//参考文档,根据routes路由映射表,创建如下面包屑导航映射表
// const breadcrumbNameMap: Record<string, string> = {
//     '/home': '系统首页',
//     '/banner': '轮播图管理',
//     '/banner/list': '轮播图列表',
//     '/product': '产品管理',
//     '/product/list': '产品列表',
//     '/product/search': '产品筛选',
//      '/product/seckill': '产品秒杀',
//     '/product/recommend': '产品推荐',
//     '/account': '账户列表',
//     '/account/adminlist': '管理员列表',
//     '/setting': '系统设置'
// };

// 使用实现如上的对象结构
const breadcrumbNameMap: Record<string, string> = {};

function getbreadcrumbNameMap(routes: any) {
    routes.forEach((item: any) => {
        if (item.key) {
            breadcrumbNameMap[item.key] = item.label
            if (item.children) {
                getbreadcrumbNameMap(item.children)
            }
        }
    })
}

getbreadcrumbNameMap(routes)
console.log('breadcrumbNameMap', breadcrumbNameMap);
// 找到不能点击的面包屑导航的routes组成的数组
const disabledpathArr = routes.filter(item => {
    return item.children && item.key
})


export default function Appbreadcrumb({ }: Props) {
    const location = useLocation();
    const pathSnippets = location.pathname.split('/').filter((i) => i);
    // console.log('pathSnippets', pathSnippets);
    const extraBreadcrumbItems = pathSnippets.map((_, index) => {
        const url = `/${pathSnippets.slice(0, index + 1).join('/')}`;
        //console.log('url', url);
        //判断如果是二级路由, 则不让其点击, 不使用Link标签,否则使用Link 标签
        if (disabledpathArr.some(item1 => item1.key == url)) {
            return {
                key: url,
                title: <>{breadcrumbNameMap[url]}</>,
            };
        } else {
            return {
                key: url,
                title: <Link to={url}>{breadcrumbNameMap[url]}</Link>,
            };
        }

    });

    //console.log('extraBreadcrumbItems', extraBreadcrumbItems);
    const breadcrumbItems = [
        {
            title: <Link to="/">仪表盘</Link>,
            key: '/',
        },
    ].concat(extraBreadcrumbItems);

    return (
        <Breadcrumb
            items={breadcrumbItems}
        />
    )
}
```

最后将面包屑导航组件引入到头部组件 Appheader 组件中

```tsx
// src/components/Appheader.tsx
import React from 'react'
import {
    MenuFoldOutlined,
    MenuUnfoldOutlined,
} from '@ant-design/icons';
import { Button, Layout, theme } from 'antd';
import { useState, useEffect } from 'react';
import Bus from '@/utils/events'
// 引入面包屑导航组件+++++++++++++++++
import Appbreadcrumb from '@/components/Appbreadcrumb';
const { Header } = Layout;
type Props = {}

export default function Appheader({ }: Props) {
    const [collapsed, setCollapsed] = useState(false);
    // console.log('Appheader函数组件执行');
    useEffect(() => {
        // 将collapsed 属性传递给Appaside 组件
        Bus.emit('myevents', collapsed)
        console.log(" Bus.emit('myevents', collapsed)");
    }, [collapsed])
    const {
        token: { colorBgContainer },
    } = theme.useToken();
    return (
        <Header style={{
            padding: 0,
            background: colorBgContainer,
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'space-between',
            paddingRight: '20px'
        }}>
            {/*折叠图标  */}
            <div style={{
                display: 'flex',
                alignItems: 'center',
            }}>
                <Button
                    type="text"
                    icon={collapsed ? <MenuUnfoldOutlined /> : <MenuFoldOutlined />}
                    onClick={() => {
                        setCollapsed(!collapsed)
                    }}
                    style={{
                        fontSize: '16px',
                        width: 64,
                        height: 64,
                    }}
                />
                {/* 面包屑导航 ++++++++++++++++*/}
                <Appbreadcrumb></Appbreadcrumb>
            </div>
        </Header >
    )
}
```



## 13. 封装axios 请求

```tsx
// src/utils/request.ts
//  封装axios

import axios from 'axios';
import store from 'store';

const baseURL = process.env.NODE_ENV == 'development' ? '/api' : 'http://47.94.148.165:3001'

const instance = axios.create({
    baseURL: baseURL,
    timeout: 5000,
    headers: { 'Content-Type': 'application/json' }
})

// 
instance.interceptors.request.use(function (config) {
    // 
    if (store.get('userinfo') && store.get('userinfo')['token']) {
        config.headers['token'] = store.get('userinfo')['token']
    }
    return config;
}, function (error) {
    // Do something with request error
    return Promise.reject(error);
});



// Add a response interceptor
instance.interceptors.response.use(function (response) {
    // Do something with response data
    return response;
}, function (error) {
    // Do something with response error
    return Promise.reject(error);
});

export default instance

```



## 14.定义接口请求

在src/api/login.ts 中定义接口的请求

```ts
import instance from "@/utils/request";

// 定义登录接口
interface Iloginparam {
    adminname: string,
    password: string
}

export function loginapi(data: Iloginparam) {
    return instance({
        url: '/admin/admin/login',
        method: 'post',
        data
    })
}
```



## 15.配置开发环境反向代理

在src目录下新建 setupProxy.js 文件

该文件的文件类型必须是js 文件类型, 否则代理不生效

```shell
// 安装代理中间件
npm i http-proxy-middleware -D
```

```js
// 配置代理
// src/setupProxy.js
// 引入 代理中间件
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



## 16.登录页布局及功能

在src/views/Login.tsx中编写登录页

```tsx
import React from 'react';
import { LockOutlined, UserOutlined } from '@ant-design/icons';
import { Button, Form, Input, message } from 'antd';
import '@/assets/css/login.scss';
import { loginapi } from '@/api/login'
import store from 'store';  // 引入第三方包store (一个永久性存储的包)

const App: React.FC = () => {
    const [messageApi, contextHolder] = message.useMessage()
    const onFinish = (values: any) => {
        console.log('Received values of form: ', values);
        // 调用登录接口
        loginapi(values).then(res => {
            //console.log('res', res);
            let result = res.data.data
            if (res.data.code == 200) {
                // 表示登录成功
                // messageApi.success('登录成功')
                messageApi.open({
                    type: 'success',
                    content: '登录成功',
                    duration: 2,
                    onClose(){
                          // 将用户信息对象存到本地
                        store.set('userinfo', {
                            loginState: true,
                            adminname: result.adminname,
                            token: result.token,
                            role: result.role,
                            checkedKeys: result.checkedkeys
                        });
                    }
                });
              

            } else {
                messageApi.open({
                    type: 'error',
                    content: res.data.messgae,
                });
            }
        })
    }
    return (
        <div id='loginview'>
            <h2>嗨购后台管理系统</h2>
            {contextHolder}
            <Form
                name="normal_login"
                className="login-form"
                initialValues={{ adminname: 'admin', password: '123456' }}
                onFinish={onFinish}
            >
                <Form.Item
                    name="adminname"
                    rules={[{ required: true, message: '请输入用户名' }]}
                >
                    <Input prefix={<UserOutlined className="site-form-item-icon" />} placeholder="Username" />
                </Form.Item>
                <Form.Item
                    name="password"
                    rules={[{ required: true, message: '请输入密码' }]}
                >
                    <Input
                        prefix={<LockOutlined className="site-form-item-icon" />}
                        type="password"
                        placeholder="Password"
                    />
                </Form.Item>
                <Form.Item>
                    <Button type="primary" htmlType="submit" className="login-form-button">
                       登录
                    </Button>
                </Form.Item>
            </Form>
        </div>
    );
};

export default App;

```



## 17.创建user切片,存储登录时的信息

创建 src/store/slices/userSlice.ts 文件. 编写切片代码

```ts
//  src/store/slices/userSlice.ts

import { createSlice, PayloadAction } from '@reduxjs/toolkit';
import store from 'store';
// console.log('store', store);

interface Istate {
    loginState: boolean,
    adminname: string,
    token: string,
    role: number,
    checkedKeys: any[]
}

const initialState: Istate = {
    loginState: store.get('userinfo') ? store.get('userinfo')['loginState'] : false,
    adminname: store.get('userinfo') ? store.get('userinfo')['adminname'] : '',
    token: store.get('userinfo') ? store.get('userinfo')['token'] : '',
    role: store.get('userinfo') ? store.get('userinfo')['role'] : 0,
    checkedKeys: store.get('userinfo') ? store.get('userinfo')['checkedKeys'] : []
}
const userSlice = createSlice({
    name: 'admin',
    initialState,
    reducers: {
        changeloginstate(state, action: PayloadAction<boolean>) { // boolean为payload的数据类型
            state.loginState = action.payload
        },
        changeadminname(state, action: PayloadAction<string>) { // string为payload的数据类型
            state.adminname = action.payload
        },
        changetoken(state, action: PayloadAction<string>) {
            state.token = action.payload
        },
        changerole(state, action: PayloadAction<number>) {
            state.role = action.payload
        },
        changecheckedkeys(state, action: PayloadAction<any[]>) {
            state.checkedKeys = action.payload
        },
    }

})

// 导出所有的修改state 的方法
export const {
    changeloginstate,
    changeadminname,
    changetoken,
    changerole,
    changecheckedkeys
} = userSlice.actions

// 导出reducer
export default userSlice.reducer

```



## 18.进一步完善登录操作

```tsx
// src/views/Login.tsx

import React from 'react';
import { LockOutlined, UserOutlined } from '@ant-design/icons';
import { Button, Form, Input, message } from 'antd';
import '@/assets/css/login.scss';
import { loginapi } from '@/api/login'
import store from 'store';
//++++++++++ 包含部分为新增的代码
import { useAppDispatch } from '@/store/hooks/index';
import {
    changeadminname,
    changecheckedkeys,
    changeloginstate,
    changerole,
    changetoken
} from '@/store/slices/userSlice'
import { useNavigate } from 'react-router-dom'
//++++++++++
const App: React.FC = () => {
    // ++++++++包含部分为新增的代码
    const dispatch = useAppDispatch();
    const navigate = useNavigate();
    // ++++++++
    const [messageApi, contextHolder] = message.useMessage()
    const onFinish = (values: any) => {
        console.log('Received values of form: ', values);
        // 调用登录接口
        loginapi(values).then(res => {
            // console.log('res', res);
            let result = res.data.data
            if (res.data.code == 200) {
                // 表示登录成功
                // messageApi.success('登录成功')
                messageApi.open({
                    type: 'success',
                    content: '登录成功',
                    duration:2,
                    onClose(){
                        // 将用户信息对象存到本地
                        store.set('userinfo', {
                            loginState: true,
                            adminname: result.adminname,
                            token: result.token,
                            role: result.role,
                            checkedKeys: result.checkedkeys
                        });
                        // ++++++++++++++++++++++++++++++++包含部分为新增的代码
                         // 将登录后的信息存到rtk 中
                         dispatch(changeloginstate(true))
                         dispatch(changeadminname(result.adminname))
                         dispatch(changecheckedkeys(result.checkedkeys))
                         dispatch(changerole(result.role))
                         dispatch(changetoken(result.token))
                         // 跳转到系统首页
                         navigate('/', { replace: true })  // 直接替换, 不能back上上一个登录页 
                   	    // ++++++++++++++++++++++++++++++++
                    }
                });
            } else {
                messageApi.open({
                    type: 'error',
                    content: res.data.messgae,
                });
            }
        })
    }
    return (
        <div id='loginview'>
            <h2>嗨购后台管理系统</h2>
            {contextHolder}
            <Form
                name="normal_login"
                className="login-form"
                initialValues={{ adminname: 'admin', password: '123456' }}
                onFinish={onFinish}
            >
                <Form.Item
                    name="adminname"
                    rules={[{ required: true, message: '请输入用户名' }]}
                >
                    <Input prefix={<UserOutlined className="site-form-item-icon" />} placeholder="Username" />
                </Form.Item>
                <Form.Item
                    name="password"
                    rules={[{ required: true, message: '请输入密码' }]}
                >
                    <Input
                        prefix={<LockOutlined className="site-form-item-icon" />}
                        type="password"
                        placeholder="Password"
                    />
                </Form.Item>
                <Form.Item>
                    <Button type="primary" htmlType="submit" className="login-form-button">
                       登录
                    </Button>
                </Form.Item>
            </Form>
        </div>
    );
};

export default App;

```



## 19.登录鉴权校验

修改App.tsx 组件,根据登录状态显示当前的页面

```tsx
// src/App.tsx

import React from 'react';
import { Routes, Route, Navigate } from 'react-router-dom';
import Login from '@/views/Login';
import Layout from '@/views/layout/Index';
// ++++++++ (包含为新增的代码)
import { useAppSelector } from '@/store/hooks/index'
// ++++++++
type Props = {};

export default function App({ }: Props) {
  //+++++获取登录状态 
  const loginState = useAppSelector(state => state.userinfo.loginState)
 //console.log('loginState', loginState);

  return (
    <Routes>
          {/*登录状态+++++判断*/}
      	<Route path='/login' element={loginState ? <Navigate to='/home'></Navigate> : 			<Login></Login>} ></Route>
     	<Route path='/*' element={loginState ? <Layout></Layout> : <Navigate to='/login' 			/>}></Route>
    </Routes>
  )
}
```



## 20.退出登录操作

退出时保留原页面, 下次登录直接进入原页面的需求

```tsx
// src/layout/Appheader.tsx 

import React, { useState } from 'react';

import {
    MenuFoldOutlined,
    MenuUnfoldOutlined,
    DownOutlined
} from '@ant-design/icons';
import { Layout, theme, Dropdown, Space, Typography } from 'antd';
import { useAppDispatch, useAppSelector } from '@/store/hooks';
// 引入面包屑导航组件
import Appbreadcrumb from '@/components/Appbreadcrumb'
import type { MenuProps } from 'antd';
import { useLocation, useNavigate } from 'react-router-dom';
// 引入store 中的用户信息切片
import { changeloginstate } from '@/store/modules/adminSlice'
import store from 'store'

const { Header } = Layout;
type Props = {}

export default function Appheader({ }: Props) {
    //
    const dispatch = useAppDispatch();
    // 定义主题
    const {
        token: { colorBgContainer },
    } = theme.useToken();
    // 下拉菜单选项
    const items = [
        {
            key: '/setting',
            label: '系统设置',
        },
        {
            key: 'loginout',
            label: '退出登录',
        },
        {
            key: 'other',
            label: '其他操作',
        },
    ];

    // 获取跳转
    const navigate = useNavigate();
    const onClick: MenuProps['onClick'] = ({ key }) => {
        // console.log(key);
        if (key == '/setting') {
            navigate(key)
        }
        if (key == 'loginout') {
            // 退出登录
            // 清除登录时本地的存储信息
            store.remove('userinfo');
            // 修改登录状态
            dispatch(changeloginstate(false))
            navigate('/login') 
        }

    };

    // 获取用户信息
    const adminname = useAppSelector(state => state.admin.adminname);

    return (
        <Header style={{
            padding: 0,
            background: colorBgContainer,
            display: 'flex',
            alignItems: 'center',
            borderBottom: '1px solid lightgray'
        }}>
            {/* 切换图标 */}
            {React.createElement(collapsed ? MenuUnfoldOutlined : MenuFoldOutlined, {
                className: 'trigger',
                onClick: () => dispatch(setcollapsed(!collapsed)),
            })}
            {/* 面包屑导航 */}
            <Appbreadcrumb></Appbreadcrumb>
            {/* 退出登录 */}
            <div style={{ position: 'absolute', right: '16px' }}>
                <Dropdown menu={{ items, onClick }}>
                    <a onClick={(e) => e.preventDefault()}>
                        <Space>
                            {/* 用户信息 */}
                            {adminname}
                            <DownOutlined />
                        </Space>
                    </a>
                </Dropdown>
            </div>

        </Header>
    )
}
```



## 21.渲染用户列表

```tsx
// src/layout/views/account/Adminlist.tsx

import React from 'react'
import { addAdminApi, deleteAdminApi, getAdminInfoApi, getAdminlistApi } from '@/api/account'
import { Button, Space, Table, Tag } from 'antd';
import type { ColumnsType } from 'antd/es/table';
import { EditOutlined, DeleteOutlined } from '@ant-design/icons'
import { useEffect, useState } from 'react';
type Props = {}

export default function Adminlist({ }: Props) {
    const [userlist, setuserlist] = useState<Array<DataType>>([])

    useEffect(() => {
        // 请求数据
        getAdminlistApi().then(res => {
            console.log('res', res);
            setuserlist(res.data.data)
        })

    }, []);

    interface DataType {
        adminid: string;
        adminname: string;
        password: string;
        role: number;
        checkedKeys: any[];
        _id: string
    }

    const columns: ColumnsType<DataType> = [
        {
            title: '序号',
            render: (_, record, index) => <span>{(current - 1) * pageSize + index + 1}</span>,
        },
        {
            title: '账户',
            dataIndex: 'adminname',
            key: 'adminname',
            render: (text) => <a>{text}</a>,
        },
        {
            title: '角色',
            dataIndex: 'role',
            key: 'role',
            render: (record) => {
                // console.log('record', record);
                if (record == 1) {
                    return <Tag color="red">{'管理员'}</Tag>
                }
                if (record == 2) {
                    return <Tag color="green">{'普通用户'}</Tag>
                }
                if (record == 3) {
                    return <Tag color="green">{'其他'}</Tag>
                }
            }
        },
        {
            title: '操作',
            key: 'action',
            render: (_, record) => (
                <Space size="middle">
                    <Button icon={<EditOutlined />}></Button>
                    <Button icon={<DeleteOutlined />}></Button>
                </Space>
            ),
        },
    ];

    // 定义当前页
    const [current, setCurrent] = useState(1)
    // 每页条数
    const [pageSize, setPageSize] = useState(10)

    return (
        <div>
            <Table
                columns={columns}
                dataSource={userlist}
                rowKey={(record) => record.adminid}
                scroll={{ y: 240 }}
                pagination={{
                    position: ['bottomLeft'],
                    current: current,
                    pageSize: pageSize,
                    pageSizeOptions: [10, 20, 50, 100],
                    showTotal: (total) => <span>当前总条数为{userlist.length}条</span>,
                    showQuickJumper: true,
                    showSizeChanger: true,
                    onChange: (page, pageSize) => {
                        setCurrent(page)
                        setPageSize(pageSize)
                        console.log(page, pageSize);

                    }
                }}
            />
        </div>
    )
}

```



## 22.配置antd中文语言包

参考地址:  https://ant.design/docs/react/i18n-cn


```tsx
import zhCN from 'antd/locale/zh_CN';

return (
  <ConfigProvider locale={zhCN}>
    <App />
  </ConfigProvider>
);
```



## 23.添加用户

```tsx
// src/views/layout/account/Adminlist.tsx

import React from 'react'
import { addAdminApi, deleteAdminApi, getAdminInfoApi, getAdminlistApi } from '@/api/account'
import { Button, Space, Table, Tag, Input, Select, Tree, message } from 'antd';
import type { ColumnsType } from 'antd/es/table';
import { EditOutlined, DeleteOutlined } from '@ant-design/icons'
import { useEffect, useState } from 'react';
import { getTreedata } from '@/utils/common';
import routes from '@/router/routes';
// ++++
import { Drawer } from 'antd';
//++++
type Props = {}

export default function Adminlist({ }: Props) {
    const [userlist, setuserlist] = useState<Array<DataType>>([]);
    // +++++
    const [open, setOpen] = useState(false);
    // +++
    const getuserlist = () => {
        getAdminlistApi().then(res => {
            //console.log('res', res);
            setuserlist(res.data.data)
        })
    }
    useEffect(() => {
        // 请求数据
        getuserlist()

    }, []);


    interface DataType {
        adminid: string;
        adminname: string;
        password: string;
        role: number;
        checkedKeys: any[];
        _id: string
    }

    const columns: ColumnsType<DataType> = [
        {
            title: '序号',
            render: (_, record, index) => <span>{(current - 1) * pageSize + index + 1}</span>,
        },
        {
            title: '账户',
            dataIndex: 'adminname',
            key: 'adminname',
            render: (text) => <a>{text}</a>,
        },
        {
            title: '角色',
            dataIndex: 'role',
            key: 'role',
            render: (record) => {
                // console.log('record', record);
                if (record == 1) {
                    return <Tag color="red">{'管理员'}</Tag>
                }
                if (record == 2) {
                    return <Tag color="green">{'普通用户'}</Tag>
                }
                if (record == 3) {
                    return <Tag color="green">{'其他'}</Tag>
                }
            }
        },
        {
            title: '操作',
            key: 'action',
            render: (_, record) => (
                <Space size="middle">
                    <Button icon={<EditOutlined />}></Button>
                    <Button icon={<DeleteOutlined />}></Button>
                </Space>
            ),
        },
    ];

    // 定义当前页
    const [current, setCurrent] = useState(1)
    // 每页条数
    const [pageSize, setPageSize] = useState(2)
    const [adminname, setadminname] = useState(''); // 用户名
    const [password, setpassword] = useState(''); // 密码
    const [role, setRole] = useState(1); // 角色  // 1 是超级管理员 2是管理员

    const [checkedKeys, setcheckedKeys] = useState(['/home'])
    const [messageApi, contextHolder] = message.useMessage();
    return (
        <div>
            <Button onClick={() => { // 添加按钮
                setOpen(true)
				// 清空输入框
                setadminname('');
                setpassword('');
                setRole(1)
                setcheckedKeys(['/home'])
            }}>添加</Button>
            <Table
                columns={columns}
                dataSource={userlist}
                rowKey={(record) => record.adminid}
                scroll={{ y: 240 }}
                pagination={{
                    position: ['bottomLeft'],
                    current: current,
                    pageSize: pageSize,
                    pageSizeOptions: [2, 4, 6],
                    showTotal: (total) => <span>当前总条数为{userlist.length}条</span>,
                    showQuickJumper: true,
                    showSizeChanger: true,
                    onChange: (page, pageSize) => {
                        setCurrent(page)
                        setPageSize(pageSize)
                        console.log(page, pageSize);

                    }
                }}
            />
            {/* ++++++++ */}
            <Drawer title="Basic Drawer" placement="right" onClose={() => {
                setOpen(false)
            }} open={open}>
                <Space direction="vertical" style={{ display: 'flex' }}>
                    <div>
                        <Input placeholder="账户名" value={adminname} onChange={(e) => setadminname(e.target.value)} />
                    </div>
                    <div><Input placeholder="密码" value={password} onChange={(e) => setpassword(e.target.value)} /></div>
                    <div>
                        <Select
                            style={{ width: '100%' }}
                            placeholder="选择角色"
                            onChange={(value) => {
                                setRole(value)
                            }}
                            value={role}
                            options={[
                                { value: 1, label: '超级管理员' },
                                { value: 2, label: '管理员' }
                            ]}
                        />
                    </div>
                    <div> <span>请选择用户权限菜单:</span></div>
                    <div style={{ border: '1px solid lightgray', borderRadius: '5px' }}>
                        {/* 树形菜单 */}
                        <Tree
                            checkable
                            onCheck={(checkedKeysValue: any) => {
                                console.log('checkedKeysValue', checkedKeysValue);

                                setcheckedKeys(checkedKeysValue)
                            }}
                            checkedKeys={checkedKeys}
                            treeData={getTreedata(routes)}
                        />
                    </div>
                    <Space>
                        <Button type='primary' onClick={() => {
                            addAdminApi({ adminname, password, role: String(role), checkedKeys }).then(res => {
                                // console.log('res', res);
                                if (res.data.code == 200) {
                                    // 关闭输入框
                                    setOpen(false)
                                    // 更新管理员列表
                                    getuserlist()
                                } else {
                                    messageApi.open({
                                        type: 'error',
                                        content: res.data.message
                                    })
                                }
                            })
                        }}>添加确认</Button>
                    </Space>
                </Space>
            </Drawer>
        </div>
    )
}

```

在src/utils/common.ts 文件中定义方法

```ts
// src/utils/common.ts 

// 方法: 将路由routes 数组转成符合Treedata树形菜单数据的处理函数
export function getTreeDataArr(routes: any) {
    const targetArr = routes.map((item: any) => {
        // 同时判断item.label和item.key
        if (item.label && item.key) {
            if (item.children) {
                return {
                    title: item.label,
                    key: item.key,
                    children: getTreeDataArr(item.children)
                }
            } else {
                return {
                    title: item.label,
                    key: item.key
                }
            }
        }
    })

    return targetArr.filter((item: any) => item != undefined)
}

```



## 24.编辑和删除用户

```tsx
// src/layout/views/account/Adminlist.tsx

import React from 'react'
import { addAdminApi, deleteAdminApi, getAdminInfoApi, getAdminlistApi, updateAdminApi } from '@/api/account'
import { Button, Space, Table, Tag, Input, Select, Tree, message } from 'antd';
import type { ColumnsType } from 'antd/es/table';
import { EditOutlined, DeleteOutlined } from '@ant-design/icons'
import { useEffect, useState } from 'react'
import { getTreedata } from '@/utils/common'
import routes from '@/router/routes';
// ++++
import { Drawer, Popconfirm } from 'antd';
//++++
type Props = {}

// console.log(getTreedata(routes))
export default function Adminlist({ }: Props) {
    const [userlist, setuserlist] = useState<Array<DataType>>([])
    // +++++
    const [open, setOpen] = useState(false);
    // +++
    const getuserlist = () => {
        getAdminlistApi().then(res => {
            //console.log('res', res);
            setuserlist(res.data.data)
        })
    }
    useEffect(() => {
        // 请求数据
        getuserlist()

    }, []);

    interface DataType {
        adminid: string;
        adminname: string;
        password: string;
        role: number;
        checkedKeys: any[];
        _id: string
    }

    const columns: ColumnsType<DataType> = [
        {
            title: '序号',
            render: (_, record, index) => <span>{(current - 1) * pageSize + index + 1}</span>,
        },
        {
            title: '账户',
            dataIndex: 'adminname',
            key: 'adminname',
            render: (text) => <a>{text}</a>,
        },
        {
            title: '角色',
            dataIndex: 'role',
            key: 'role',
            render: (record) => {
                // console.log('record', record);
                if (record == 1) {
                    return <Tag color="red">{'管理员'}</Tag>
                }
                if (record == 2) {
                    return <Tag color="green">{'普通用户'}</Tag>
                }
                if (record == 3) {
                    return <Tag color="green">{'其他'}</Tag>
                }
            }
        },
        {
            title: '操作',
            key: 'action',
            render: (_, record) => (
                <Space size="middle">
                    <Button icon={<EditOutlined />} onClick={
                        //++++++++++++点击编辑++++++++++++++++++
                        () => {
                            // 01: 显示抽屉
                            setOpen(true)
                            // 数据回填
                            setadminname(record.adminname)
                            setpassword(record.password)
                            setRole(record.role)
                            setcheckedKeys(record.checkedKeys)
                            setaddflag(false)
                        }
                    }></Button>
                    {/*++++++++++++点击删除++++++++++++++++++*/}
                    <Popconfirm
                        title="提示"
                        description="确认要删除吗?"
                        okText="Yes"
                        cancelText="No"
                        onConfirm={() => {
                            // console.log(record);
                            // 删除用户 
                            deleteAdminApi({ adminid: record.adminid }).then(res => {
                                // console.log('res', res);
                                if (res.data.code == 200) {
                                    // 重新请求用户数据
                                    getuserlist()
                                }

                            })
                        }}
                    >
                        <Button icon={<DeleteOutlined />}></Button>
                    </Popconfirm>
                </Space>
            ),
        },
    ];

    // 定义当前页
    const [current, setCurrent] = useState(1)
    // 每页条数
    const [pageSize, setPageSize] = useState(2)
    const [adminname, setadminname] = useState(''); // 用户名
    const [password, setpassword] = useState(''); // 密码
    const [role, setRole] = useState(1); // 角色  // 1 是超级管理员 2是管理员
    const [checkedKeys, setcheckedKeys] = useState(['/home']);
    const [messageApi, contextHolder] = message.useMessage();
    //+++++++设置当前是添加还是修改操作+++++++++++
    const [addflag, setaddflag] = useState(true); // true 默认是添加, false 是编辑
    return (
        <div>
            <Button onClick={() => {
                setOpen(true)
                // 清空输入框
                setadminname('');
                setpassword('');
                setRole(1)
                setcheckedKeys(['/home'])
                setaddflag(true)
            }}>添加</Button>
            <Table
                columns={columns}
                dataSource={userlist}
                rowKey={(record) => record.adminid}
                scroll={{ y: 240 }}
                pagination={{
                    position: ['bottomLeft'],
                    current: current,
                    pageSize: pageSize,
                    pageSizeOptions: [2, 4, 6],
                    showTotal: (total) => <span>当前总条数为{userlist.length}条</span>,
                    showQuickJumper: true,
                    showSizeChanger: true,
                    onChange: (page, pageSize) => {
                        setCurrent(page)
                        setPageSize(pageSize)
                        console.log(page, pageSize);

                    }
                }}
            />
            {/* 抽屉 */}
            <Drawer title="Basic Drawer" placement="right" onClose={() => {
                setOpen(false)
            }} open={open}>
                <Space direction="vertical" style={{ display: 'flex' }}>
                    <div>
                        <Input placeholder="账户名" value={adminname} onChange={(e) => setadminname(e.target.value)} />
                    </div>
                    <div><Input placeholder="密码" value={password} onChange={(e) => setpassword(e.target.value)} /></div>
                    <div>
                        <Select
                            style={{ width: '100%' }}
                            placeholder="选择角色"
                            onChange={(value) => {
                                setRole(value)
                            }}
                            value={role}
                            options={[
                                { value: 1, label: '超级管理员' },
                                { value: 2, label: '管理员' }
                            ]}
                        />
                    </div>
                    <div> <span>请选择用户权限菜单:</span></div>
                    <div style={{ border: '1px solid lightgray', borderRadius: '5px' }}>
                        {/* 树形菜单 */}
                        <Tree
                            checkable
                            onCheck={(checkedKeysValue: any) => {
                                console.log('checkedKeysValue', checkedKeysValue);
                                setcheckedKeys(checkedKeysValue)
                            }}
                            checkedKeys={checkedKeys}
                            treeData={getTreedata(routes)}
                        />
                    </div>
                    <Space>
                   {/*+++++添加确认按钮和编辑确认按钮不同时显示,根据addflag条件判断++++++++++ */}
                        {
                            addflag ? <Button type='primary' onClick={() => {
                                addAdminApi({ adminname, password, role: String(role), checkedKeys }).then(res => {
                                    // console.log('res', res);
                                    if (res.data.code == 200) {
                                        // 关闭输入框
                                        setOpen(false)
                                        // 更新管理员列表
                                        getuserlist()
                                    } else {
                                        messageApi.open({
                                            type: 'error',
                                            content: res.data.message
                                        })
                                    }
                                })
                            }}>添加确认</Button> :
                                <Button type='primary' onClick={() => {
                                    updateAdminApi({ adminname, password, role: String(role), checkedKeys }).then(res => {
                                       // console.log('res', res);
                                        if (res.data.code == 200) {
                                            // 关闭输入框
                                            setOpen(false)
                                            // 更新管理员列表
                                            getuserlist()
                                        } else {
                                            messageApi.open({
                                                type: 'error',
                                                content: res.data.message
                                            })
                                        }
                                    })
                                }}>编辑确认</Button>
                        }

                    </Space>
                </Space>
            </Drawer>
        </div >
    )
}

```



## 25.按钮权限

设置 添加按钮/ 删除按钮/ 编辑按钮 的权限 

```tsx
// src/views/layout/account/Adminlist.tsx

// 引入封装好的 useAppSelector
import { useAppSelector } from '@/store/hooks/index';

// 获取redux 中存贮的登录时用户的角色
const rolePermission = useAppSelector(state => state.userinfo.role)
// 进行添加操作时, 
 <Button type='primary' onClick={() => {
     if (rolePermission != 1) {
            messageApi.warning({
                type: 'warning',
                content: '只有超级管理员才可以删除!!!'
            })
            return
        }
     // 打开侧边栏抽屉
     setOpen(true);
     // 清空输入框
     setadminname('');
     setpassword('');
     setRole(1)
     setcheckedKeys(['/home'])
     setaddflag(true)
 }}>添加</Button>

// 编辑和删除同上
//.......
```



## 25.根据登录后的用户返回的菜单项渲染菜单

在 /src/utils/common.ts中定义两个方法用来处理将登录后的用户返回的数组处理成routes 菜单

```ts
// 定义一个方法, 将 checkedKeys数组  
// checkedKeys =['/home', '/banner/home', '/banner/active', '/product/list', '/account', '/account/list', '/account/adminlist']处理成
//  checkedKeys =['/home', '/banner','/banner/home', '/banner/active', '/product','/product/list', '/account', '/account/list', '/account/adminlist']
export function getnewcheckedKeys(checkedKeys: any) {
    let newcheckedKeys: any = [];
    for (const path of checkedKeys) {
        const temparr = path.split('/');
        let currPath = '';
        for (let i = 1; i < temparr.length; i++) {
            currPath += '/' + temparr[i];
            if (!newcheckedKeys.includes(currPath)) {
                newcheckedKeys.push(currPath);
            }
        }
    }
    // console.log('newcheckedKeys', newcheckedKeys);
    return newcheckedKeys
}

export function filterRoutes(routes: any, checkedKeys: any) {
    // 参数1: 本地定义的 routes菜单
    // 参数2: 为方法1 得到的处理后的 checkedKeys数组
    // 将 checkedKeys= ['/home', '/banner','/banner/home', '/banner/active', '/product','/product/list', '/account', '/account/list', '/account/adminlist']
    // 和 路由routes菜单比较, 得到用户需要渲染的左侧菜单栏数据
    // 注意: 一定要使用数组的倒循环, 否则根据下标删除的话, 导致数组下标改变, 删除出问题
    for (let i = routes.length - 1; i >= 0; i--) {
        const route = routes[i]; // 这样可以避免ts类型报错
        if (route.key && !checkedKeys.includes(route.key)) {
            routes.splice(i, 1);
            console.log('route', route);
        }
        if (!route.key) {
            routes.splice(i, 1);
        }
        if (route?.children) {
            filterRoutes(route.children, checkedKeys);
        }
    }

    return routes;
}
```

重新渲染菜单

```tsx
// /src/components/Appmenu.tsx

import React from 'react'
import { Menu } from 'antd';
import type { MenuProps } from 'antd';
import { Link } from 'react-router-dom';
// 引入如有数组
import routes from '@/router/routes';
//导入方法用来处理 用户返回的菜单数组
import { getNewCheckedKeysFn, getNewRoutes } from '@/utils/common';
import { useAppSelector } from '@/store/hooks/index';
// 引入loadsh 使用该方法进行递归深拷贝
import _ from 'lodash';

type MenuItem = Required<MenuProps>['items'][number];
function getItem(
    label: React.ReactNode,
    key: React.Key,
    icon?: React.ReactNode,
    children?: MenuItem[],
    type?: 'group',
): MenuItem {
    return {
        key,
        icon,
        children,
        label,
        type,
    } as MenuItem;
}
type Props = {}

export default function Appmenu({ }: Props) {
//注意: copyRoutes 为深拷贝的路由routes ,所以当操作该copyRoutes进行删除操作时, 不影响原routes路由,深拷贝操作一定要写在该位置, 因为只有写在函数组件中的函数内, 函数组件每次渲染才会都会触发.写在外边只触发一次
	
    let copyRoutes = _.cloneDeep(routes);
    // 获取当前登录的用户信息中的checkedKeys菜单项
    const checkedKeys = useAppSelector(state => state.userinfo.checkedkeys)
    // 判断当菜单项长度为0,则表示当前是admin 账户,则显示全部菜单
    const newRoutes = checkedKeys.length==0 ? copyRoutes : getNewRoutes(copyRoutes, getNewCheckedKeysFn(checkedKeys))
    //console.log('newRoutes', newRoutes);
    const getItemsFn = (newRoutes: any) => {
        let targetRoutes = newRoutes.map((item: any) => {
            if (item.label && item.key) {
               // console.log(item.label && item.key);
                if (item.children) {
                    // 递归调用getItemsFn
                    return getItem(item.label, item.key, item.icon, getItemsFn(item.children)) 
                } else {
                    return getItem(<Link to={item.path}>{item.label}</Link>, item.key, item.icon)
                }

            } else {
                return null
            }
        })

        return targetRoutes
    }

    return (
        <Menu
            theme="dark"
            mode="inline"
            defaultSelectedKeys={['1']}
            items={getItemsFn(newRoutes)}
        />
    )
}
```



## 26.系统首页

系统首页可以进行echarts 一些数据图标的信息展示

```tsx
 // src/views/layout/home/home.tsx
import React from 'react'
import Kechart from '@/components/Kecharts';
import { getKdataApi } from '@/api/home';
import { useState, useEffect } from 'react'
type Props = {}

export default function Home({ }: Props) {
    const [kdata, setKdata] = useState<{ x: Array<any>, val: Array<any> } | {}>({})
    useEffect(() => {
        getKdataApi().then(res => {
            console.log(res);
            setKdata(res.data.data)
        })
    }, [])
    return (
        <div>
            {
                (kdata as { x: Array<any>, val: Array<any> }).x ? <Kechart id='box1' w='50%' h='500px' data={kdata as { x: Array<any>, val: Array<any> }}></Kechart> : null
            }

        </div>
    )
}

```

## 27. 封装echart 组件

```tsx
// src/components/Kechart.tsx

import React from 'react'
import * as echarts from 'echarts';
import { useEffect } from 'react'
type Props = {
    id: string,
    w?: string,
    h?: string,
    data: {
        x: Array<any>,
        val: Array<any>
    }
}

export default function Kecharts({ id, w, h, data }: Props) {
    // 基于准备好的dom，初始化echarts实例
    useEffect(() => {
        var myChart = echarts.init(document.getElementById(id) as HTMLElement);
        // /console.log(document.getElementById('box1'));
        // console.log(data.x, data.val);
        window.addEventListener('resize', function () {
            myChart.resize();
        });
        // 绘制图表
        myChart.setOption({
            xAxis: {
                data: data.x
            },
            yAxis: {},
            series: [
                {
                    type: 'candlestick',
                    data: data.val
                }
            ]
        });
    }, [])

    return (
        <div id={id} style={{
            width: w ? w : '100%',
            height: h ? h : '500px',
            border: '1px solid black'
        }}></div>
    )
}
```



## 28.轮播图管理

轮播图列表页

```tsx
// src/views/layout/banner/Bannerlist.tsx

import React, { useEffect, useState } from 'react';
import { Space, Table, Button, Popconfirm, Image } from 'antd';
import { DeleteOutlined } from '@ant-design/icons';

// 引入接口
import { getBannerListApi, addBannerApi, deleteBannerApi } from '@/api/banner';
import { ColumnsType } from 'antd/es/table';
import { useNavigate } from 'react-router-dom';
interface DataType {
    alt: string,
    bannerid: string,
    flag: boolean,
    img: string,
    link: string
}
type Props = {}
const { Column, ColumnGroup } = Table;
export default function Home({ }: Props) {

    // 定义当前页
    const [current, setCurrent] = useState(1);
    // 每页条数
    const [pageSize, setPageSize] = useState(10);

    const changeFn = (page: number, pageSize: number) => {
        setCurrent(page)
        setPageSize(pageSize)
        // console.log(page, pageSize);
    }

    const columns: ColumnsType<DataType> = [
        {
            title: '序号',
            render: (_, record, index) => <span>{(current - 1) * pageSize + index + 1}</span>,
        },
        {
            title: '图片',
            dataIndex: 'adminname',
            key: 'adminname',
            render: (_, record) => <Image src={record.img}></Image>,
        },
        {
            title: '链接',
            dataIndex: 'link',
            key: 'link',
            render: (_, record) => <a>{record.link}</a>,
        },
        {
            title: '提示',
            dataIndex: 'alt',
            key: 'alt',
            render: (_, record) => <span>{record.alt}</span>

        },
        {
            title: '操作',
            key: 'action',
            render: (_, record) => (
                <Space size="middle">
                    <Popconfirm
                        title="提示"
                        description="确认要删除吗?"
                        okText="Yes"
                        cancelText="No"
                        onConfirm={() => {
                            // console.log(record);
                            // 删除轮播图
                            deleteBannerApi({ bannerid: record.bannerid }).then(res => {
                                if (res.data.code == 200) {
                                    // 重新更新轮播图列表
                                    getbannerlist()
                                }
                            })
                        }}
                    >
                        <Button icon={<DeleteOutlined />}></Button>
                    </Popconfirm>
                </Space >
            ),
        },
    ];
    // 获取轮播图列表
    const [bannerList, setBannerList] = useState([]);

    const getbannerlist = () => {
        getBannerListApi().then(res => {
            //console.log(res);
            setBannerList(res.data.data)
        })
    }
    useEffect(() => {
        // 初始化执行
        getbannerlist()
    }, [])

    const navigate = useNavigate()
    return (
        <div>
            {/* {contextHolder} */}
            {/* 操作按钮 */}
            <Space>
                <Button type='primary' onClick={() => {
                    //打开添加页
                    navigate('/banner/add')

                }}>添加</Button>
            </Space>
            {/* 表格数据 */}
            <Table
                columns={columns}
                dataSource={bannerList}
                rowKey={(record) => record.bannerid}
                scroll={{ y: 500 }}
                pagination={{
                    position: ['bottomLeft'],
                    pageSizeOptions: [1, 2, 10, 20],
                    total: bannerList.length,
                    showTotal: (total: number) => <span>当前总条数为{total}条</span>,
                    showQuickJumper: true,
                    showSizeChanger: true,
                    onChange: changeFn
                }}
            />
        </div>
    )
}
```



## 29.添加轮播图

在路由表 /src/router/routes.tsx 中添加addbanner路由

```tsx
// /src/router/routes.tsx;
const routes = [
    // ......
    {
        path: '/banner',
        element: <Banner></Banner>,
        label: '轮播图管理',
        key: '/banner',
        icon: <MailOutlined />,
        children: [
            {
                path: '/banner/bannerlist',
                element: <Bannerlist />,
                label: '轮播图列表',
                key: '/banner/bannerlist',
                icon: <MenuFoldOutlined />,
            },
            {
                path: '/banner/addbanner',
                element: <AddBanner />, // +++++++
                label: '添加轮播图' //++++用于显示面包屑导航
            }
        ]
    },
```

创建/src/views/layout/banner/Addbanner.tsx 组件

```tsx
// src/views/layout/banner/Addbanner.tsx;

import React, { useState, useRef, useMemo } from 'react';
import { Input, Space, Button, message, Image } from 'antd';
import { addBannerApi } from '@/api/banner';
import { useNavigate } from 'react-router-dom';

type Props = {}

export default function Add({ }: Props) {
    const [link, setLink] = useState('');
    const [alt, setAlt] = useState('');
    const [img, setImg] = useState('');

    const file = useRef<any>();
    // 点击上传图片事件
    const onChange = function () {
        // 
        const fileInfo = file.current.input.files[0];
        // 利用 fileReader 对象生成base64图片格式
        const reader = new FileReader();
        reader.readAsDataURL(fileInfo);
        reader.onload = function () {
            setImg(this.result as string)
        }
    }

    const navigate = useNavigate()
    const [messageApi, contextHolder] = message.useMessage();
    // 控制提交按钮的显示和隐藏
    const flag = useMemo(() => {
        return link != '' && alt != '' && img != ''
    }, [link, alt, img])
    // 点击提交事件
    const submitFn = () => {
        addBannerApi({ link, alt, img }).then(res => {
            if (res.data.code == 200) {
                messageApi.open({
                    type: 'success',
                    content: '上传成功',
                    onClose: () => {
                        // 返回上一页,上一页肯定是 轮播图首页
                        navigate(-1)
                    }
                });
            }
        })

    }

    return (
        <div>
            {contextHolder}
            <Space direction='vertical' style={{ width: 500 }}>
                <Input placeholder='link' value={link} onChange={(e) => { setLink(e.target.value) }}></Input>
                <Input placeholder='alt' value={alt} onChange={(e) => setAlt(e.target.value)}></Input>
                <Input placeholder='img' ref={file} type='file' onChange={onChange}></Input>
                <Image src={img}></Image>
                <Input placeholder='图片地址' value={img} ></Input>
                <Button disabled={!flag} type='primary' onClick={submitFn}>提交</Button>

            </Space>
        </div >
    )
}
```



## 30.产品管理

产品列表

```tsx
// src/views/layout/product/productlist.tsx

// 产品列表
import React, { useEffect, useState } from 'react';
import { Space, Table, Button, Popconfirm, Image, Switch } from 'antd';
import { DeleteOutlined } from '@ant-design/icons';

// 引入接口
import { getProductListApi, updateProisSecStatus } from '@/api/product';
import { ColumnsType } from 'antd/es/table';
type Props = {}
const { Column, ColumnGroup } = Table;
interface DataType {
    brand: string,
    category: string,
    desc: string,
    discount: number,
    img1: string,
    img2: string,
    img3: string,
    img4: string,
    isrecommend: number,
    issale: number,
    isseckill: number,
    originprice: number,
    proid: string,
    proname: string,
    sales: number
    stock: number,
}

export default function Productlist({ }: Props) {
    // 获取轮播图列表
    const [proList, setproList] = useState([]);
    const getprolist = () => {
        getProductListApi({ count: 1, limitNum: 10 }).then(res => {
            console.log(res);
            setproList(res.data.data)
        })
    }
    useEffect(() => {
        // 初始化执行
        getprolist()
    }, []);
    const [current, setCurrent] = useState(1);
    const [pageSize, setPageSize] = useState(10)

    const columns: ColumnsType<DataType> = [
        {
            title: '序号',
            render: (_, record, index) => <span>{(current - 1) * pageSize + index + 1}</span>,
        },
        {
            title: '图片',
            dataIndex: 'adminname',
            key: 'adminname',
            render: (_, record) => <Image src={record.img1}></Image>,
        },
        {
            title: '名称',
            dataIndex: 'proname',
            key: 'proname',
            render: (_, record) => <span style={{
                // 'text-overflow': 'ellipsis',
                // 'display': '-webkit-box',
                // '-webkit-box-orient': 'vertical',
                // '-webkit-line-clamp': 2,
                // 'overflow': 'hidden'
            }}> {record.proname}</span >,
            ellipsis: true,
        },
        {
            title: '品牌',
            dataIndex: 'brand',
            key: 'brand',
            render: (_, record) => <span>{record.brand}</span>,
        },
        {
            title: '分类',
            dataIndex: 'category',
            key: 'category',
            render: (_, record) => <span>{record.category}</span>,
            filters: [
                { text: 'Joe', value: 'Joe' },
                { text: 'Jim', value: 'Jim' },
            ],
            onFilter: (val: any, record) => {
                console.log('val', val);

                return record.category.includes(val)
            }

        },
        {
            title: '原价',
            dataIndex: 'originprice',
            key: 'originprice',
            render: (_, record) => <span>{record.originprice}</span>,
        },
        {
            title: '折扣',
            dataIndex: 'discount',
            key: 'discount',
            render: (_, record) => <span>{record.discount}</span>,
        },
        {
            title: '销量',
            dataIndex: 'sales',
            key: 'sales',
            render: (_, record) => <span>{record.sales}</span>,
        },
        {
            title: '是否秒杀',
            dataIndex: 'isseckill',
            key: 'isseckill',
            render: (_, record) => <Switch defaultChecked={record.isseckill == 1} onChange={(checked: boolean) => {
                // 修改秒杀
                console.log('checked', checked);
                updateProisSecStatus({
                    proid: record.proid,
                    type: 'isseckill',
                    flag: checked
                }).then(res => {
                    console.log(res);

                })

            }} />,
        },
        {
            title: '是否推荐',
            dataIndex: 'isrecommend',
            key: 'isrecommend',
            render: (_, record) => <Switch defaultChecked={record.isrecommend === 1} onChange={() => {
                // 修改推荐

            }} />,
        },
        {
            title: '操作',
            key: 'action',
            render: (_, record) => (
                <Space size="middle">
                    <Popconfirm
                        title="提示"
                        description="确认要删除吗?"
                        okText="Yes"
                        cancelText="No"
                        onConfirm={() => {
                            // console.log(record);
                            // 删除轮播图

                        }}
                    >
                        <Button icon={<DeleteOutlined />}></Button>
                    </Popconfirm>
                </Space >
            ),
        },
    ];

    return (
        <div>
            {/* {contextHolder} */}
            {/* 表格数据 */}
            <Table
                columns={columns}
                dataSource={proList}
                rowKey={(record) => record.proid}
                scroll={{ y: 500 }}
                pagination={{
                    current: current,
                    pageSize: pageSize,
                    position: ['bottomLeft'],
                    pageSizeOptions: [1, 2, 10, 20],
                    total: setproList.length,
                    onChange(current, pageSize) {
                        setCurrent(current)
                        setPageSize(pageSize)
                    }
                }}
            />
        </div>
    )
}
```





## 31.导入导出

安装第三方包(导出使用的包)

```shell
npm i  js-export-excel
```

安装第三方包(导入使用的包)

```shell
cnpm i xlsx  -S
```

导出功能 

```tsx
// src/views/layout/product/productlist.tsx

// 产品列表组件

// 导入导出+++++++++++++
// 导出使用的包
import ExportJsonExcel from 'js-export-excel';
// 导入使用的包
import XLSX from 'xlsx';

export default function Productlist({ }: Props) {
    // 获取产品列表
    const [proList, setproList] = useState([]);
    //............代码省略................
    // 导入
    const importFn = () => {
        const file = (document.querySelector('#fileRef') as any).files[0];
        const reader = new FileReader();
        reader.readAsBinaryString(file); // 转成二进制格式文件
        reader.onload = function () {
            const workbook = XLSX.read(this.result, { type: 'binary' }) //
            const t = workbook.Sheets['产品列表1'] // 拿到表格数据  list 为上传的表的sheet页面名称
            const r: any = XLSX.utils.sheet_to_json(t) // 转换成json 格式
            setproList(r) // 将当前页产品列表数据重新赋值渲染
            // 将r 这个json 格式的数据上传到后端,调用后端接口
            console.log('r', r);
            // 将r 这个json 格式的数据上传到后端,调用后端接口
        }
    }
    //导出+++++++++++++++++++++++++
    const exportFn = () => {
        let options = {
            fileName: '产品列表',
            datas: [
                {
                    sheetData: proList,
                    sheetName: "产品列表1",
                    sheetFilter: ['proid', 'proname', 'img1', 'category'],
                    sheetHeader: ['proid', 'proname', 'img1', 'category'],// 表头的值
                    columnWidths: [20, 20],
                },
                {
                    sheetData: proList, //数据
                    sheetName: '产品列表2',// 导出的表格页签名
                    sheetFilter: ['proid','proname', 'img1', 'category', 'originprice'], // 需要导出的数据的字段
                    sheetHeader: ['proid','proname', 'img1', 'category','originprice'],// 表头的值
                    columnWidths: ['20', '20'], // 设置列宽
                }
            ]
        }
        var toExcel = new ExportJsonExcel(options); //new
        toExcel.saveExcel(); //保存
    }

    return (
        <div>
            {/* {contextHolder} */}
            <div>
                <Button onClick={() => {
                    (document.querySelector('#fileRef') as HTMLInputElement).click();
                }}>
                    导入
                </Button>
                <input type="file" hidden id='fileRef' onChange={importFn} />
                <Button onClick={exportFn}>导出</Button>
            </div>
            {/* 表格数据 */}
            <Table
                columns={columns}
                dataSource={proList}
                rowKey={(record) => record.proid}
                scroll={{ y: 500 }}
                pagination={{
                    current: current,
                    pageSize: pageSize,
                    position: ['bottomLeft'],
                    pageSizeOptions: [1, 2, 10, 20],
                    total: setproList.length,
                    onChange(current, pageSize) {
                        setCurrent(current)
                        setPageSize(pageSize)
                    }
                }}
            />
        </div>
    )
}
```

以上还有一种是后端实现的导出,  一般会后端会给一个链接, 前端 a 标签直接下载即可



## 32. 判断当用户手动输入地址栏,有权限提示

```ts
// src/utils/common.ts
// 封装将所有的routes转成数组形式 [/home,'/account','/account/adminlist',.....] 
const routesAllArr: any = [];
export function getallpathFn(routes: any) {
    routes.map((item: any) => {
        routesAllArr.push(item.path)
        if (item.children) {
            getallpathFn(item.children)
        }
    })
    return routesAllArr
}
```



```tsx
// src/App.tsx
// ++++++++++
import routes from '@/router/routes'
import { getallpathFn } from '@/utils/common'


const routesAllArr = getallpathFn(routes);


function App() {
  const store = useAppSelector(state => state);
  let token = store.userinfo.token;
  // +++++++++++++
  const [messageApi, contextHolder] = message.useMessage();
  // 获取当前的路由信息
  const loc = useLocation();
  let checkedKeys = store.userinfo.checkedkeys;
  const navigate = useNavigate();
  // 判断当用户手动在地址栏输入地址,提示权限不能访问
  useEffect(() => {
    if (
      routesAllArr.includes(loc.pathname) &&
      checkedKeys.length != 0
      && !checkedKeys.includes(loc.pathname)
    ) {
      // 当前登录的该用户的路由数组中不包含地址栏手动输入地址
      messageApi.open({
        type: 'error',
        content: '您没有权限访问该路由!!!',
        duration: 1,
        onClose() {
          // 返回上一页
          navigate(-1)
        }
      });
    }
  }, [loc.pathname])

  return (
    <div className="App">
      {contextHolder}
      <Routes>
        {/* 通过判断本地的token, 进而决定是否匹配正确的路由 */}
        {/* <Route path='/login' element={<Login />}></Route>
        <Route path='/*' element={<Layout></Layout>}></Route>
        <Route path='*' element={<Notfind></Notfind>}></Route> */}
        <Route path='/login' element={token ?
          <Navigate to='/' /> : <Login />}></Route>
        <Route path='/*' element={token ? <Layout></Layout> : <Navigate to='/login' />}></Route>
        <Route path='*' element={<Notfind></Notfind>}></Route>
      </Routes>
    </div >
  );
}

export default App;
```





