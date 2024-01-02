# 零、uniapp基本语法学习

> 可以参考千锋教育推出的【uniapp蛋糕项目】

# 一、项目搭建及认识

## 1.1 uniapp项目创建

1. 创建uniapp项目
2. .vue文件扮演的角色
   + 扮演的页面，内部需要使用onLoad、onShow系列的生命周期
   + 扮演的公共组件，内部需要使用created系列生命周期
3. 注意平台差异性
   + 全局配置的角度 [参考]([pages.json - uni-app官网 (dcloud.io)](https://uniapp.dcloud.io/collocation/pages?id=tabbar))
   + API的调用方面 [参考]([发起请求 - uni-app官网 (dcloud.io)](https://uniapp.dcloud.io/api/request/request))

4. 通过条件编译做不同平台适配

   [文档]([条件编译 解决各端差异 - uni-app官网 (dcloud.io)](https://uniapp.dcloud.io/platform))

   ````
   #ifndef H5
   这里的代码，只会在H5平台生效
   #endif
   ````

   

5. uniapp跨端适配可能会遇到的问题

   [文档]([uni-app 跨端开发注意 - uni-app官网 (dcloud.io)](https://uniapp.dcloud.io/matter))

## 1.2 uniapp目录结构
+ components   公共组件
+ pages    存放项目页面
+ static    静态资源
+ App.vue     根组件
+ main.js      项目入口文件
+ menifest.json  项目配置文件（App打包相关配置、路由模式配置...）
+ pages.json    应用配置（底部菜单、导航栏....）
+ uni.scss   全局样式文件

## 1.3 uniapp开发流程

[锋选菁英移动端设计图](https://js.design/f/ZI-xQt?p=mi9pO3)

1. 按需进行全局配置

   例如:底部菜单

2. 进行页面布局

   + upx     作用跟rpx一样

     > 需要保证设计图宽度为750px        1px=1upx

   + Flex布局

   + 组件库

3. 使用vue语法，做交互

   + 数据对接
   + 功能交互

## 1.4 底部菜单配置

1. 准备素材
2. 参考文档
3. 实现底部菜单配置

```json
{
	"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		...
    ],
	"globalStyle": {
		...
	},
	"tabBar": {
		"color": "#d8d8d8",
		"selectedColor": "#FD7753",
		"list": [
			{
				"text": "首页",
				"pagePath": "pages/home/home",
				"iconPath": "static/img/home_icon.png",
				"selectedIconPath": "static/img/home_icon_act.png"
			},{
				"text": "企业",
				"pagePath": "pages/company/company",
				"iconPath": "static/img/company_icon.png",
				"selectedIconPath": "static/img/company_icon_act.png"
			},{
				"text": "我的",
				"pagePath": "pages/mine/mine",
				"iconPath": "static/img/min_icon.png",
				"selectedIconPath": "static/img/mine_icon_act.png"
			}
		]
	},
	"uniIdRouter": {}
}

```



## 1.5 组件库配置
1. colorUI     倾向于布局  

   [GitHub文档](https://github.com/weilanwl/ColorUI)

   [ColorUI-UniApp - DCloud 插件市场](https://ext.dcloud.net.cn/plugin?id=239)

   + 下载核心代码
   + 将colorui文件夹引入项目中
   + 使用组件

4. 图鸟UI  

   [图鸟UI - DCloud 插件市场](https://ext.dcloud.net.cn/plugin?id=7088)

   [TuniaoUI (tuniaokj.com)](https://vue2.tuniaokj.com/)

[拓展组件文档 (dcloud.net.cn)](https://uniapp.dcloud.net.cn/component/uniui/uni-ui.html)

# 二、首页开发

## 2.1 自定义组件封装

> 跟Vue中封装组件一样，引入、挂载、使用的方式，也一样

1. 组件与页面的生命周期有差异

   + 页面
     + created   可用Vue的生命周期
     + onLoad     可用小程序的生命周期    【建议使用这个】
   + 公共组件
     + created    可用Vue的生命周期   【建议使用这个】

2. uniapp的setup中使用onLoad

   ```javascript
   import {onLoad} from '@dcloudio/uni-app'  //如果是vue3，需要引入才能使用onLoad
   ```

3. 组件封装

   > 对热门岗位、最新岗位、热门公司等结构做可复用性分析及封装，形成可复用组件
   >
   > company-job-item.vue

   ```vue
   <template>
   	<view class="job-item margin-bottom-sm padding" :class="cType">
   	    <view class="flex align-center justify-between">
   	        <view  class="flex align-center company-info">
   	            <image class="margin-right-sm" src="/static/img/avatar1.png"></image>
   	            <text class="text-grey">千锋互联</text>
   	        </view>
   	        <view class="iconfont icon-bookmark-o"></view>
   	    </view>
   	    <view class="margin-tb-sm text-lg">前端开发</view>
   	    <view class="text-grey margin-bottom-sm">
   			重庆 · 九龙坡
   			<text class="text-orange" v-if="cType=='new'"> · 全职</text>
   		</view>
   	    <view class=" text-orange" v-if="cType=='hot1'">全职</view>
   		<view class="margin-top-sm" v-if="cType=='new'">
   			岗位描述
   		</view>
   	    <view class="flex margin-top-sm">
   	        <view class="margin-right">
   	            <text class="iconfont icon-renqun"></text>
   	            80
   	        </view>
   	        <view>
   	            <text class="iconfont icon-browse"></text>
   	            300
   	        </view>
   	    </view>
   	</view>
   </template>
   
   <script>
   	export default {
   		name:"company-job-item",
   		props: {
   			cType: {
   				type: String,
   				default:'hot1', //hot1热门岗位  new 最新  hot2热门公司
   				validator(value){ //自定义验证函数
   					return ['hot1','hot2','new'].includes(value)
   				}
   			},
   		},
   		data() {
   			return {
   				
   			};
   		},
   		created() {
   			console.log('组件的created');
   		}
   	}
   </script>
   
   <style>
   .job-item{
       /* height: 336upx; */
       border: 1px solid #d8d8d8;
   }
   .hot1{
   	width: 344upx;
   }
   .new{
   	margin:20upx
   }
   .job-item image{
       height: 40upx;
       width: 40upx;
   }
   .company-info{
       max-width: 80%;
   }
   </style>
   ```

   

## 2.2 首页结构搭建及交互

1. swiper轮播

2. 宫格布局

   ```vue
   <view class="cu-list grid col-4 no-border" >
       <view 
             class="cu-item" 
             v-for="(item,index) in cateList" 
             :key="index" 
             @click="handleJobList(item)"
             >
           <view>
               <image :src="item.icon" class="gird-icon" mode="heightFix"></image>
           </view>
           <text>{{item.name}}</text>
       </view>
       <view class="cu-item">
           <view>
               <image src="../../static/img/home_grid5.png" class="gird-icon" mode="heightFix"></image>
           </view>
           <text>更多</text>
       </view>
   </view>
   
   ```

   

3. 自定义封装输入框组件

## 2.3 数据请求方式

#### 方法1：使用uni.request

```javascript
uni.request({
    url:'https://woniu.h5project.cn/1.1/classes/Job',
    method:'GET',
    header:{
        "X-LC-Id": "自己的id",
        "X-LC-Key": "自己的key",
        "Content-Type": "application/json"
    },
    success: (res) => {
        console.log(res);
    }
})
```

#### 方法2：自行封装uni.request

```javascript
// promise的三种状态： pendding处理中  resolve成功   reject失败

import {BASE_URL,ID,KEY} from '../config/index.js'

function http({url,method="GET",data={}}){
  return new Promise((resolve,reject)=>{
    uni.request({
      url:BASE_URL+url,
      method,
      data,
      header:{
        'X-LC-Id': ID, //此处必须使用自己的ID
        'X-LC-Key': KEY,  //此处必须使用自己的Key
        'Content-Type': 'application/json'
      },
      success:(res)=>{
        resolve(res)   //交给then
      },
      fail:(err)=>{
        reject(err)  //交给catch
      }
    })
  })
}

function get(url,data){
  return http({url,method:'GET',data})
}

function post(url,data){
  return http({url,method:'POST',data})
}

function del(url,data){
  return http({url,method:'DELETE',data})
}

export {
  http,
  get,
  post,
  del
}
```

#### 方法3：使用luch-request第三方库

[luch-request - DCloud 插件市场](https://ext.dcloud.net.cn/plugin?id=392)

[3.x文档 | luch-request (quanzhan.co)](https://www.quanzhan.co/luch-request/guide/3.x/#介绍)

> axios 可能不兼容小程序

```javascript
import Request from '../js_sdk/luch-request/luch-request/index.js'
import {BASE,ID,KEY} from '../config/index.js'
const http = new Request()
/* config 为默认全局配置*/
http.setConfig((config) => { 
    config.baseURL = BASE+'/1.1/'; /* 根域名 */
    config.header = {
        "X-LC-Id": ID,
        "X-LC-Key": KEY,
        "Content-Type": "application/json"
    }
    return config
})
// 请求拦截器
http.interceptors.request.use((config) => { // 可使用async await 做异步操作
  config.header = {
    ...config.header,
    // a: 1 // 演示拦截器header加参
  }
  console.log('请求拦截器');
  return config
}, config => { // 可使用async await 做异步操作
  return Promise.reject(config)
})
//响应拦截器
http.interceptors.response.use((response) => { /* 对响应成功做点什么 可使用async await 做异步操作*/
  console.log('响应拦截器',response)
  return response
}, (response) => { /*  对响应错误做点什么 （statusCode !== 200）*/
  console.log(response)
  return Promise.reject(response)
})

export default http
```



## 2.4 scroll-view组件使用

1. 基本使用

2. 拓展使用

   > 横向滚动指示器

   ```
   已知条件
   1. scroll区域的最大滑动距离    max = wrapperWidth-scrollWidth
   2. scroll-view的实时滚动距离  sLeft = ev.scrollLeft
   3. 指示器dot的最大移动距离    dotMax = 200-30
   
   ? / dotMax = sLeft / max
   
   ```

   ```vue
   <template>
   	<view>
   		<scroll-view scroll-x id="scroll" @scroll="handleScroll">
   			<view id="wrapper" class="scroll-wrapper flex justify-between padding-sm" :style="{width:`${374*5}upx`}">
   				<CompanyJobItem class="margin-right" v-for="(item,index) in newJobList" :jobdata="item" v-show="index<5"/>
   			</view>
   		</scroll-view>
   		<view class="bar">
   			<view class="dot" :style="{transform:`translateX(${move}px)`}">
   			</view>
   		</view>
   	</view>
   </template>
   
   <script>
   	import CompanyJobItem from '../../components/company-job-item.vue'
   	import {ID,KEY,BASE} from '../../config/index.js'
   	import request from '../../utils/request.js'
   	export default {
   		components: {
   			CompanyJobItem
   		},
   		data() {
   			return {
   				newJobList:[],
   				scrollWidth:0,  //页面宽度
   				wrapperWidth:0, //横向滚动内容宽度
   				move:0  //指示器实时移动距离
   			};
   		},
   		onLoad() {
   			request.get('classes/ReactJob').then(res=>{
   				this.newJobList = res.data.results
   			})
   		},
   		onReady() {
   			const query = uni.createSelectorQuery().in(this);
   			query.select('#scroll').boundingClientRect(data => {
   			  // console.log(data);
   			  this.scrollWidth = data.width
   			}).exec();
   			query.select('#wrapper').boundingClientRect(data => {
   			  // console.log(data);
   			  this.wrapperWidth = data.width
   			}).exec();
   		},
   		methods: {
   			handleScroll(ev) {
   				let {scrollWidth,wrapperWidth} = this
   				console.log(scrollWidth,wrapperWidth);
   				let max = wrapperWidth - scrollWidth
   				let sLeft = ev.detail.scrollLeft
   				let dotMax = 200-30
   				this.move = (sLeft/max)*dotMax
   			}
   		},
   	}
   </script>
   
   <style lang="scss">
   swiper{
   	margin: 20upx 20upx;
   }
   .swiper-item{
   	image{
   		height: 380upx;
   	}
   }
   .home-grid-item{
   	image{
   		width: 80upx;
   		height: 80upx;
   	}
   }
   .scroll-wrapper{
   	// border: 1px solid red;
   	// overflow: hidden;
   	// white-space: nowrap;
   	
   }
   .bar{
   	width: 200px;
   	height: 5px;
   	margin: 20upx auto;
   	background-color: #d8d8d8;
   	.dot{
   		width: 30px;
   		height: 5px;
   		background-color: orange;
   	}
   }
   </style>
   
   ```


# 三、岗位列表交互

## 3.1 岗位列表搭建及交互分析

1. 列表渲染
2. 分页功能
3. 下拉刷新
4. 按条件进行岗位查询



## 3.2 约束查询完成数据按分类加载

1. 约束查询的文档

   ```
   curl -X GET \
     -H "X-LC-Id: vvatm5BpnB9IVOCNdPkDMeNl-gzGzoHsz" \
     -H "X-LC-Key: U1HDuXYkYqcBlYbuL8Vt7C3d" \
     -H "Content-Type: application/json" \
     -G \
     --data-urlencode 'where={"bcid":"1"}' \ 查询条件
     --data-urlencode 'limit=10' \ 加载的数据条数
     --data-urlencode 'skip=10' \  跳过的数据条数
     https://API_BASE_URL/1.1/classes/Post
   ```

2. 约束查询的方法封装

   ```javascript
   import request from '../utils/request.js'
   export const cakeGet = (page=1,bcid=1)=>{
   	return request.get('classes/CakeGoods',{
   		params:{
   			limit:8,
   			skip:(page-1)*8,
   			where:{bcid}
   		}
   	})
   }
   ```

3. 用户点击不同分类时，只需要改变条件，重新发起请求

## 3.3 数据请求方法封装复用

```javascript
const fetchData = ()=>{
	cakeGet(page,bcid.value).then(res=>{
		uni.stopPullDownRefresh()
		let {results} = res.data
		if(results.length){
			list.value = [
				...list.value,
				...results
			]
			page++
		}else{
			uni.showToast({
				title:'没有更多了',
				icon:'none'
			})
		}
	})
}
```



## 3.4 分类宫格跳转列表

1. 条件查询
2. home页跳转岗位列表页

> 必须使用uni.switchTab才能跳转底部菜单页面

```javascript
handleJobList({name}){
    uni.setStorage({
        key:'cateName',
        data:name
    })
    uni.switchTab({  
        url:`/pages/company/company`
    })
}
```

3. 岗位列表页获取分类条件

```javascript
onShow() {
    uni.getStorage({
        key:'cateName',
        success: ({data}) => {
            console.log('提取',{data});
            this.jobList = []
            this.page = 1
            this.cateName = data  //记录分类名称，方便分页请求时继续使用
            this.fetchData(data)
        }
    })
},
onReachBottom() {
    console.log('触底了');
    this.fetchData(this.cateName)
},
methods:{
    fetchData(name){
        jobGet(this.page,name).then(res=>{
            uni.stopPullDownRefresh() //关闭下拉刷新动画
            let {results} = res.data
            if(results.length){
                this.jobList = [
                    ...this.jobList,
                    ...results
                ]
                this.page++
                return
            }
            uni.showToast({
                title:"没有更多数据了",
                icon:'none'
            })
        })
    }
}
```

## 3.5 列表访问逻辑完善

1. 检测岗位菜单点击的特殊生命周期

```javascript
//pages/company/company
onTabItemTap() {
    console.log('点击了底部菜单');
    uni.removeStorage({
        key:'cateName'
    })
},
```

2. 确保点击菜单的事件先执行

```javascript
onShow() {
    setTimeout(()=>{
        uni.getStorage({
            key:'cateName',
            success: ({data}) => {
                console.log('提取',{data});
                this.jobList = []
                this.page = 1
                this.cateName = data
                this.fetchData(data)
            },
            fail: (err) => {
                this.jobList = []
                this.page = 1
                this.cateName = ''
                this.fetchData()
            }
        })
    },200) //定时器为了保证onTabItemTap先执行
},
```



## 3.6 空列表及代码优化

1. 优化uni.getStorage代码逻辑

```javascript
onShow() {
    setTimeout(()=>{
        uni.getStorage({
            key:'cateName',
            complete: ({data}) => {
                console.log('complete',data);
                this.jobList = []
                this.page = 1
                this.cateName = data
                this.fetchData(data)
            }
        })
    },200) //定时器为了保证onTabItemTap先执行
},
```



2. 数据为空的组件

```vue
<view class="padding margin" v-if="emptyShow">
    <tn-empty 
              mode="data" 
              icon="https://tuniao.ahuaaa.cn/componentsPage/static/images/empty/data.jpg"
              imgWidth="300"
    ></tn-empty>
</view>
```

3. 列表为空的代码逻辑优化

```vue
<template>
	<view>
		<view class="padding margin" v-if="emptyShow">
			<tn-empty 
			mode="data" 
			icon="https://tuniao.ahuaaa.cn/componentsPage/static/images/empty/data.jpg"
			imgWidth="300"
			></tn-empty>
		</view>
		<view class="padding-sm flex flex-wrap justify-between">
			<job-item v-for="item in jobList" :jobdata="item"/>
		</view>
	</view>
</template>

<script>
import { jobGet } from '../../api/job'
	export default {
		data() {
			return {
				jobList:[],
				page:1,
				cateName:'',
				emptyShow:false  //1. 控制数据为空组件的显示隐藏
			}
		},
		onShow() {
			setTimeout(()=>{
				uni.getStorage({
					key:'cateName',
					complete: ({data}) => {
						console.log('complete',data);
						this.jobList = []
						this.page = 1
						this.cateName = data
						this.fetchData(data)
					}
				})
			},200) //定时器为了保证onTabItemTap先执行
		},
		onReachBottom() {
			console.log('触底了');
			this.fetchData(this.cateName)
		},
		onPullDownRefresh() {
			this.jobList = []
			this.page = 1
			this.fetchData()
		},
		onTabItemTap() {
			console.log('点击了岗位菜单');
			uni.removeStorage({
				key:'cateName'
			})
		},
		methods:{
			fetchData(name){
				jobGet(this.page,name).then(res=>{
					uni.stopPullDownRefresh() //关闭下拉刷新动画
					let {results} = res.data
					if(this.page==1&&!results.length){ //2. 首页数据为空时，显示空组件
						this.emptyShow = true
					}
					if(results.length){
						this.emptyShow = false //3. 首页有数据的情况下，隐藏空组件
						this.jobList = [
							...this.jobList,
							...results
						]
						this.page++
						return
					}
					uni.showToast({
						title:"没有更多数据了",
						icon:'none'
					})
				})
			}
		}
	}
</script>
```





# 四、vuex状态机实践

## 4.1 vuex状态机的引入

> 在状态机中维护定位信息
>
> 使用步骤，跟vue中一样，但是不需要安装



## 4.2 城市定位功能

[高德web服务API](https://lbs.amap.com/api/webservice/guide/api/georegeo/)

1. 申请高德的web服务API的key
2. 使用uni.getLocation获取当前用户位置经纬度
3. 使用高德的api接口，逆地理编码获取位置信息
   [API接口地址](https://restapi.amap.com/v3/geocode/regeo?location=106.480852,29.535204&key=你自己的key)
4. 核心代码

```javascript
uni.getLocation({ //获取经纬度
				success: (res) => {
					console.log(res);
					let {longitude,latitude} = res
					// 让高德地图根据经纬度，下发城市信息
					let url = `https://restapi.amap.com/v3/geocode/regeo?key=8797592ffcb983c041b54da66bee871d&location=${longitude},${latitude}`
					uni.request({
						url,
						success: (info) => {
							console.log(info);
							let {province} = info.data.regeocode.addressComponent
							this.$store.commit('location/cityChangeMut',province)
						}
					})
				}
			})
```

## 4.3 登录页搭建

> 使用图鸟UI快速搭建

## 4.4 登录功能实现



1. 封装登录请求api

```javascript
import request from '../utils/request.js'

export const userLogin = (info)=>{
	return request.post('login',info)
}
```



3. 新增登录的状态机模块

```javascript
import { userLogin } from "../api/user"

export default {
	namespaced:true,
	state(){
		return {
			userInfo:null
		}
	},
	mutations:{
		initInfoMut(state,info){
			state.userInfo = info
		}
	},
	actions:{
		userLoginAct(context,user){
			userLogin(user).then(res=>{
				context.commit('initInfoMut',res.data)
				uni.setStorage({
					key:'uni-2206-userInfo',
					data:res.data
				})
				uni.navigateBack({delta:1})
			})
		}
	}
}
```

4. 在登录页触发登录的异步action

```javascript
handleLoginReg(){
    if(this.currentModeIndex==0){
        // userLogin(this.account)
        this.$store.dispatch('user/userLoginAct',this.account)
    }
}
```

5. 在App.vue中触发初始化用户信息的mutation尝试提取本地存储用户信息

```javascript
<script>
	export default {
		onLaunch: function() {
			...
			// 尝试提取本地存储用户信息
			try {
				const value = uni.getStorageSync('uni-2206-userInfo');
				if (value) {
					console.log('本地存储用户信息',value);
					this.$store.commit('user/initInfoMut',value)
				}
			} catch (e) {
				// error
				console.log('用户信息提取失败');
			}
		},
	}
</script>

```

## 4.5 注册功能

1. 注册接口

   ```
   curl -X POST \
     -H "X-LC-Id: {{appid}}" \
     -H "X-LC-Key: {{appkey}}" \
     -H "Content-Type: application/json" \
     -d '{"username":"tom","password":"f32@ds*@&dsa","phone":"18612340000"}' \
     https://API_BASE_URL/1.1/users
   ```

2. 封装注册api

   ```javascript
   //注册
   export const userReg = (account)=>{
   	return request.post('users',account)
   }
   ```

   

3. 触发注册请求

   ```javascript
   handleLogin(){
       if(this.currentModeIndex==0){
           this.$store.dispatch('user/userLoginAction',this.info) //登录
       }else{
           userReg(this.info)  //注册
       }
   },
   ```

4. 响应拦截器添加交互提示

   ```javascript
   http.interceptors.response.use((response) => { /* 对响应成功做点什么 可使用async await 做异步操作*/
     console.log('响应拦截器',response)
     let {url} = response.config
     if(url=='login'||url=='users'){
   	  uni.showToast({
   	  	title:'操作成功'
   	  })
     }
     return response
   }, (response) => { /*  对响应错误做点什么 （statusCode !== 200）*/
     console.log(response)
     uni.showToast({
     	title:'操作失败',
   	icon:'none'
     })
     return Promise.reject(response)
   })
   ```

   

# 五、个人中心相关交互

## 5.1 光速搭建个人中心

> 使用图鸟UI的个人中心模板搭建

## 5.2 用户信息设置页

> 复用custom-input组件

## 5.3 custom-input组件交互完善

1. 组件内部需要能够接收并修改输入框数据

   > 当我们给custom-input绑定v-model的时候，vue会向该组件内部传递两个内容
   >
   > 1. props数据，默认名称为 value ，可以通过props提取直接使用
   > 2. 自定义事件，默认名称为 @input ，可以通过this.$emit('input',可选数据)触发

   ```vue
   // Vue2版本的自定义组件v-model
   <template>
   	<view class="custom-input flex align-center padding-lr">
   		<text class="iconfont margin-right" :class="icon"></text>
   		<input type="text" v-bind:value="value" @input="handleInput" :placeholder="placeholder"/>
   	</view>
   </template>
   
   <script>
   	export default {
   		name:"custom-input",
   		props: {
   			placeholder: {
   				type: String,
   				default: '开始寻找你梦寐以求的工作...'
   			},
   			icon:{
   				type:String,
   				default:'icon-a-001_sousuo'
   			},
   			value:{
   				type:String
   			}
   		},
   		data() {
   			return {
   				
   			};
   		},
   		methods: {
   			handleInput(ev) {
   				this.$emit('input',ev.target.value) //当我们给custom-input绑定v-model的时候，vue会自动给custom-input里面传递input自定义事件
   			}
   		},
   	}
   </script>
   
   <style lang="scss">
   .custom-input{
   	height: 96upx;
   	opacity: 1;
   	border-radius: 8px;
   	background: rgba(255, 255, 255, 1);
   	border: 1px solid rgba(238, 238, 238, 1);
   	.iconfont{
   		font-size: 40upx;
   		color: rgba(102, 110, 122, 1);
   	}
   }
   </style>
   ```

   

2. 调用自定义input组件

   ```vue
   <custom-input v-model="name" placeholder="请输入姓名" icon="icon-wode1"/>
   ```


## 5.4 简历上传

1. 下载leancloudSDK

   [JavaScript SDK 安装指南 - LeanCloud 文档](https://leancloud.cn/docs/sdk_setup-js.html#hash1620893804)

   ```
   npm install leancloud-storage --save
   ```

2. 在main.js做SDK的初始化

   ```
   import CloudSDK from 'leancloud-storage'
   import { BASE, ID, KEY } from '../../config';
   CloudSDK.init({
     appId: ID,
     appKey: KEY,
     serverURL:BASE
   });
   ```

3. uni.chooseFile选取本地图片，得到临时路径

4. 使用图片转换工具(image-tools)，将本地资源临时路径，转换为base64
   [转换工具](https://ext.dcloud.net.cn/plugin?id=123)

5. 使用LeanCloudSDK提供的方法，将base64资源转换成file对象进行存储操作

   [sdk使用指南]([数据存储开发指南 · JavaScript - LeanCloud 文档](https://leancloud.cn/docs/leanstorage_guide-js.html#hash825935))

```javascript
handleUpload(){
    uni.chooseFile({
        success: async(res) => {
            console.log(res);
            this.show = true
            this.step = 40
            let localFileUri = res.tempFilePaths[0] //本地临时资源路径
            let base64  = await pathToBase64(localFileUri)
            this.step = 60
            const file = new CloudSDK.File('resume.pdf', {base64})
            this.step = 90
            let res1 = await file.save()
            console.log(res1);
            this.step = 100
            this.info.resume = res1.attributes.url
            uni.showToast({
                title:'上传成功',
                icon:'none'
            })
        }
    })
}
```

## 5.5 上传进度条交互

```vue
<view v-if="step!==0" class="cu-progress radius striped active">
    <view class="bg-orange" :style="[{ width:step+'%'}]">{{step}}%</view>
</view>
```



## 5.6 更新用户信息接口需要携带session



```javascript

//更新用户信息
export const userUpdate = (userid,token,info)=>{
	return request.put(`users/${userid}`,info,{
		header:{
			'X-LC-Session':token //更新用户信息必须携带sessionToken
		}
	})
}
```





# 六、岗位详情交互

## 6.1 岗位详情页渲染

1. 详情页搭建
2. uni.navigateTo路由跳转传参
3. 详情页数据渲染

1. 

## 6.2 岗位距离计算

1. 封装距离计算函数

   ```javascript
   // 根据经纬度，计算两点的距离
   export function getDistance(lat1, lon1, lat2, lon2) {
     const earthRadius = 6371; // 地球半径，单位千米
     const dLat = (lat2 - lat1) * Math.PI / 180; // 将纬度转换为弧度
     const dLon = (lon2 - lon1) * Math.PI / 180; // 将经度转换为弧度
     const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) + Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * Math.sin(dLon / 2) * Math.sin(dLon / 2);
     const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
     const distance = earthRadius * c;
     return distance;
   }
   
   ```

2. 在岗位详情调用测距

   ```javascript
   export default {
       data() {
           return {
               detail:null,
               distance:0
           }
       },
       computed: {
           lnglat() {
               return this.$store.state.loc.lnglat //用户位置 
           }
       },
       onLoad(options) {
           // console.log(options);
           jobDetailGet(options.id).then(res=>{
               this.detail = res.data
               let {cityName,areaDistrict,city} = res.data
               let address = cityName + areaDistrict
               this.getDist(address,city)
           })
   
       },
       methods: {
           getDist(address,city){
               //计算岗位距离
               let url = `https://restapi.amap.com/v3/geocode/geo?key=e0119d3cbd6eb284068484cadeb50e07&address=${address}&city=${city}`
               uni.request({
                   url,
                   success: (res) => {
                       let {location} = res.data.geocodes[0]
                       location = location.split(',') //岗位经纬度
                       let n = getDistance(location[1],location[0],this.lnglat[1],this.lnglat[0])
                       this.distance = n.toFixed(1)
                   }
               })
           }
       }
   }
   ```
   



## 6.3 地图组件呈现企业位置

1. 通过地理编码，获取岗位经纬度

   ```javascript
   onLoad(options) {
       console.log(options);
       jobDetailGet(options.id).then(res=>{
           console.log(res);
           this.detail = res.data
           let {cityName,areaDistrict,businessDistrict} = res.data
           this.getLngLat(`${cityName}${areaDistrict}${businessDistrict}`)
       })
   },
       methods: {
           getLngLat(address) {
               let url = `https://restapi.amap.com/v3/geocode/geo?key=3cdf846aab977e514073cb7297e94f47&address=${address}`
               uni.request({
                   url,
                   success: (res) => {
                       console.log(res);
                       let {location} = res.data.geocodes[0]
                       console.log(location);
                       this.jobLocation = location.split(',')
                   }
               })
           }
       },
   ```

   

2. 在manifest.json中配置高德地图 的key

3. 使用map组件展示岗位地址

   ```
   <map :latitude="jobLocation[1]" :longitude="jobLocation[0]"></map>
   ```

   

## 6.4 岗位报名功能

1. 需要一个单独的报名表，例如：join表

2. 记录哪些报名信息

   + 用户id
   + 岗位id
   + 报名状态
     + 已投简历  1
     + 已查看   2
     + 约面试  3
     + 已通过  4
     + 已拒绝  5
   + 冗余字段  【优势：提高查询效率】【缺点：增加了数据库的存储压力】
     + 公司logo
     + 公司名称
     + 岗位名称
     + 岗位地址
     + 薪资待遇

3. 封装报名api

   ```javascript
   // 报名
   export const userJoin = (joinObj)=>{
   	return request.post('classes/ReactJoin',joinObj)
   }
   
   ```

4. 提交报名请求

   ```javascript
   // 报名
   handleJoin(){
       if(!this.userInfo){
           uni.navigateTo({
               url:'/pages/login/login'
           })
           return
       }
       let userId = this.userInfo.objectId
       let jobId = this.detail.objectId
       let {brandLogo,brandName,jobName,cityName,areaDistrict,salaryDesc} = this.detail
       userJoin({
           userId,jobId,brandLogo,
           brandName,jobName,cityName,
           areaDistrict,salaryDesc,
           status:1
       })
   }
   ```

5. 查询报名状态api

   ```javascript
   // 报名状态查询
   export const joinGet = (userId,jobId)=>{
   	return request.get('classes/ReactJoin',{
   		where:{
   			userId,
   			jobId
   		}
   	})
   }
   ```

6. 详情页初始化报名状态

   ```javascript
   onShow() { //onShow可以确保用户从登录页返回详情后，岗位状态能够正确同步
       //查询报名状态
       if(this.userInfo){
           let userId = this.userInfo.objectId
           joinGet(userId,this.jobId).then(res=>{
               let {results} = res.data
               if(results.length){
                   this.status = results[0].status
               }
           })
       }
   },
   ```

   









