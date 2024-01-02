## 微信小程序及 UinAPP 求职无忧含答案



### 1. 微信小程序是什么？

> 微信小程序本质上来讲，可以理解为一个基于「数据驱动」的 MVVM 框架，通过 state、setState 响应式的监听 数据并渲染页面，包含其自身内置的 应用、页面生命周期，封装了相对丰富的开箱即用微信原生组件，丰富了基于微信解析引擎的 API 方法，拓展了云开发等更多业务场景。详细来讲，微信小程序对比 Web 页面开发，发生了以下几方面的变化：
>
> 1. 每个页面由 页面配置 page.json、页面结构 page.wxml、页面样式 page.scss、页面逻辑 page.js(ts) 四部分组成，各司其职其对应的业务能力。
> 2. app.json、page.json 通过脚本规范对应用及页面相关功能、展示进行配置即可实现逻辑，尤其需要注意的在微信小程序中不用配置路由，直接在 app.json 中配置 pages 属性即可，也可以配置分包路由，提高小程序启动性能。
> 3. 使用微信小程序自己的布局组件 view、text 来替代 div、sapn 等标签能力，增加了 block 模块区分标签，使用 scroll-view、cover-view 等定制化场景组件标签来完成定制化的业务能力。
> 4. 微信小程序有自己的样式规范，WXSS(WeiXin Style Sheets) 是一套样式语言，具有 CSS 大部分特性，可以看作一套简化版的 css，同时为了更适合开发微信小程序，还对 CSS 进行了扩充以及修改，直接帮我们把适配的一部分工作都做了，比如他的rpx（responsive pixel），可以根据屏幕宽度进行自适应，规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。
> 5. 微信小程序内置了丰富的 API 能力，比如上传图片、获取地理位置等等。
> 6. 微信小程序也有自己完整的生态能力，可以使用 WeUI 调用更全的UI组件库，也可以使用 UniAPP 进行快速混合开发（目前市面上占比逐步增高），还有其提供的云开发能力，让构建应用更加方便快捷。



### 2. 微信小程序的原生组件有哪些？

> 微信小程序的原生组件即为微信小程序内置封装好的直接使用的组件，主要分为以下几类：
>
> 1. 容器组件
> 2. 基础组件
> 3. 表单组件
> 4. 媒体组件
> 5. 开放能力组件等



### 3. 微信小程序应用生命周期钩子函数有哪些？

> 1. onLaunch(options)
>
>    小程序被加载完毕的时候调用。这个方法一般用来做一些初始化的事情。比如获取用户 信息、获取历史缓存信息、获取小程序打开来源等。
>
> 2. onShow(options)
>
>    小程序启动，或从后台进入前台显示时调用。如果想要在小程序每次进入到前台的时候 都执行一些事情，那么可以把代码放在这个里面。比如一些实时动态更改的数据，用户每次进来都要从服务器更新，那么我们就可以在这个里面做。
>
> 3. onHide()
>
>    小程序被切换到后台（包括微信自身被切换到后台或者小程序暂时被切换到后台时)。可以在这个方法中做一些数据的保存。
>
> 4. onError(String error)
>
>    小程序发生脚本错误，或者 api 调用失败时触发。在小程序发生错误的时候，会把错误 信息发送到这个函数中，所以可以在这个函数中做一些错误收集。
>
> 5. onPageNotFound(Object)
>
>    小程序要打开的页面不存在时触发。一般在代码更新的时候，有些页面被删除了，但是 其他地方没有改过来的情况下会发生这种情况，或者一些活动页面，活动结束后被关掉了。也可以 在这个里面做一些错误的收集和页面的重新跳转。
>
> 6. getApp()
>
>    获取当前的 app 对象。一般在app.js外的地方调用。在app.js内部可以使用this获得当前的大对象；在外面要用定义在app.js的全局数据时，要用getApp()。



### 4. 微信小程序页面生命周期钩子函数有哪些？

> 1. onReady 生命周期函数--监听页面初次渲染完成
> 2. onShow 生命周期函数--监听页面显示
> 3. onHide 生命周期函数--监听页面隐藏
> 4. onUnload 生命周期函数--监听页面卸载
> 5. onPullDownRefresh 页面相关事件处理函数--监听用户下拉动作
> 6. onReachBottom 页面上拉触底事件的处理函数
> 7. onShareAppMessage 用户点击右上角转发
> 8. onPageScroll 页面滚动触发事件的处理函数
> 9. onTabItemTap 当前是 tab 页时，点击 tab 时触发



### 5. 微信小程序是如何上线的？审核时间一般多久？

> 1. 在微信 web 开发者工具里找到项目，并且设置好服务器的域名，如果小程序没有用到外网请求，可以不用配置服务器，配置好服务器，先预览一下，看看有没有问题，如果没有问题的话，点击上传。
> 2. 上传代码之后，在微信公众号平台登录微信小程序后台，点击开发管理，就可以看到刚刚上传的代码，点击提交审核，然后等待微信官方的审核。
> 3. 微信官方审核一般是 1-3 个工作日，可以申请加急审核 24 小时内，即可完成上线。



### 6. 微信小程序中如何获取用户信息？

> 1. 如果单纯的是在页面 wxml 中用来展示用户信息，直接使用  open-data 组件即可。
>
>    ```html
>    <open-data type="userAvatarUrl"></open-data>
>    <open-data type="userNickName"></open-data>
>    ```
>
> 2. 如果在 js 逻辑中需要获取用户的信息，微信版本升级更新后，需要用户点击按钮手动授权获取用户信息。
>
>    ```html
>     <button bindtap="getUserProfile"> 获取头像昵称 </button>
>    ```
>
>    ```js
>    getUserProfile() {
>        wx.getUserProfile({
>          // 声明获取用户个人信息后的用途，后续会展示在弹窗中，请谨慎填写
>          desc: '展示用户信息', 
>          success: (res) => {
>            console.log(res)
>            this.setData({
>              userInfo: res.userInfo,
>              hasUserInfo: true
>            })
>          }
>        })
>    },
>    ```
>
> 3. 补充说明，小程序为升级前，可使用wx.getUserInfo直接获取用户信息。



### 7. 微信小程序分享功能实现？

> 1. onShareAppMessage(Object object)
>
>    监听用户点击页面内转发按钮（[button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件 `open-type="share"`）或右上角菜单「转发」按钮的行为，并自定义转发内容。
>
> 2.  onShareTimeline()
>
>    监听右上角菜单「分享到朋友圈」按钮的行为，并自定义分享内容。



### 8. 微信小程序单页模式有什么限制？

> 小程序「单页模式」适用于纯内容展示场景，可实现的交互与接口能力有限，因此存在如下限制：
>
> 1. 页面无登录态，与登录相关的接口，如 `wx.login` 均不可用。
> 2. 不允许跳转到其它页面，包括任何跳小程序页面、跳其它小程序、跳微信原生页面。
> 3. 不允许横屏使用。
> 4. 若页面包含 tabBar，tabBar 不会渲染，包括自定义 tabBar。
> 5. 本地存储与小程序普通模式不共用。
>
> 对于一些会产生交互的组件或接口，在点击后调用时，会弹 toast 提示「请前往小程序使用完整服务」，达到良好的用户体验。



### 9. 微信小程序如何做好优化？（重要）

> 微信小程序优化的核心有两点：
>
> 一、提高加载性能
>
> 1. 运行环境预加载，这一步是微信自身就会做好的，我们在打开微信小程序之前微信就会准备好环境，然后直接运行小程序的代码包。
> 2. 控制代码包的大小，这一步是最核心的，也是最直接的方法。
>    - 压缩代码，清理无用的代码。
>    - 图片放在 CDN 服务器上。
>    - 采用分包策略，分包预加载、独立分包（最新版本）
> 3. 对网络异步请求进行优化。
>    - onload 预先发出请求，不用等到 onready 阶段。
>    - 对变化比较小的请求可以放在 缓存中，下次直接使用。
>    - 如果请求的数据量大，可以考虑使用 骨架屏 解决方案让用户体验更好。
>    - 先反馈，再请求。比如说：点赞按钮，先改变样式，再后台发出请求。
>
> 二、提高渲染性能
>
> 1. 减少 setData 的数据量，如果一个数据不影响页面渲染，则不要放在 setData 中。
> 2. 合并 setData 的次数，尽量放在一起。
> 3. 避免大量的列表数据全局更新，根据 index 进行局部更新。
> 4. 对于 scroll 事件使用 防抖、节流 解决方案，避免过于频繁 的更新。
> 5. 可以使用 懒加载、预加载的方式来增强用户的体验友好性。
> 6. 针对长列表海量数据渲染，可以使用 虚拟滚动解决方案进行优化。



### 10. 微信小程序路由跳转实现？

> 1. 通过组件navigator跳转，设置url属性指定跳转的路径，设置open-type属性指定跳转的类型（可
>
>    选），open-type的属性有 redirect, switchTab, navigateBack，我们也称作为「声明式导航」
>
>    ```js
>    // redirect 对应 API 中的 wx.redirect 方法 
>    <navigator url="/page/redirect/redirect?title=redirect" open-type="redirect">在当 前页打开</navigator>
>    // navigator 组件默认的 open-type 为 navigate 
>    <navigator url="/page/navigate/navigate?title=navigate">跳转到新页面</navigator> 
>    // switchTab 对应 API 中的 wx.switchTab 方法 
>    <navigator url="/page/index/index" open-type="switchTab">切换 Tab</navigator>
>    // reLanch 对应 API 中的 wx.reLanch 方法 
>    <navigator url="/page/redirect/redirect?title=redirect" open-type="redirect">//关 闭所有页面，打开到应用内的某个页面
>    // navigateBack 对应 API 中的 wx.navigateBack 方法 
>    <navigator url="/page/index/index" open-type="navigateBack">关闭当前页面，返回上一级 页面或多级页面</navigator>
>    ```
>
> 2. 通过api跳转，wx.navigateTo() , wx.navigateBack(), wx.redirectTo() , wx.switchTab(), wx.reLanch() ，我们也辰作为「编程式导航」。
>
>    ```js
>    wx.navigateTo({
>      url: 'page/home/home?user_id=1' // 页面 A 
>    })
>    wx.navigateTo({
>      url: 'page/detail/detail?product_id=2' // 页面 B 
>    })
>    // 跳转到页面 A 
>    wx.navigateBack({
>      delta: 2 //返回指定页面 
>    })
>    // 关闭当前页面，跳转到应用内的某个页面。 
>    wx.redirectTo({ url: 'page/home/home?user_id=111' })
>    // 跳转到tabBar页面（在app.json中注册过的tabBar页面），同时关闭其他非tabBar页面。 
>    wx.switchTab({ url: 'page/index/index' })
>    // 关闭所有页面，打开到应用内的某个页面。 
>    wx.reLanch({ url: 'page/home/home?user_id=111' })
>    ```



### 11. 微信小程序兼容性问题都有哪些？

> 1. ios下的zIndex层级问题，主要发生在 iphone7 和 iphoneX 下 绝对定位必须有一个共同的父元素。
> 2. 左右边框不生效 当边框的宽度设置为奇数的时候，可能会不生效 解决方法：将宽度设置为偶数的时候，在ios下就可以解决。
> 3. 还有尽量不要用margin-bottom ，当元素是在整个页面的最底部的时候，在ios下可能margin- bottom会失效，所以建议，都使用padding-bottom 。
> 4. new Date跨平台兼容性问题，在Andriod使用new Date(“2018-05-30 00:00:00”)木有问题，但是在ios下面识别不出来。因为IOS下面不能识别这种格式，需要用2018/05/30 00:00:00格式。可以使用正则表达式对做字符串替换，将短横替换为斜杠。var iosDate= date.replace(/-/g, '/');。 
> 5. wx.getUserInfo()接口更改，微信小程序最近被吐槽最多的一个更改，就是用户使用wx.getUserInfo（开发和体验版）时不会弹出授权，正式版不受影响。现在授权方式是需要引导用户点击一个授权按钮，然后再弹出授权。



### 12. 微信小程序开发框架都有哪些？

> 1. Taro 
> 2. uni-app 
> 3. WeUI 
> 4. mpvue 
> 5. iView Weapp



### 13. 微信小程序如何获取用户的电话号码？

> 1. 准备一个button组件, 将 **button** 组件 open-type 的值设置为 getPhoneNumber ，当用户点击并同意之后，可以通过 bindgetphonenumber 事件回调获取到动态令牌 code ;。
>
>    ```html
>    <button open-type="getPhoneNumber" bindgetphonenumber="getPhoneNumber">点击获取电话号码</button>
>    ```
>
>    ```js
>    Page({ getPhoneNumber (e) { console.log(e.detail.code) } })
>    ```
>
> 2. 接着把 code 传到开发者后台，并在开发者后台调用微信后台提供的 **phonenumber.getPhoneNumber** 接口，消费 code 来换取用户手机号。每个 code 有效期为5分钟，且只能消费一次。
>
>    ```js
>    getPhoneNumber: function (e) {
>        var that = this;
>        console.log(e.detail.errMsg == "getPhoneNumber:ok"); 
>        if (e.detail.errMsg == "getPhoneNumber:ok") { 
>          wx.request({ 
>            url: 'http://localhost/index/users/decodePhone', 
>            data: { 
>              encryptedData: e.detail.encryptedData, 
>              iv: e.detail.iv, 
>              sessionKey: that.data.session_key, 
>              uid: "", 
>            }, 
>            method: "post", 
>            success: function (res) { 
>              console.log(res); 
>            } }) 
>          }
>    }
>    ```
>
>    注： getPhoneNumber 返回的 code 与 wx.login 返回的 code 作用是不一样的，不能混用.
>
>    注：从基础库 2.21.2 开始，对获取手机号的接口进行了安全升级, 需要用户主动触发才能发起获取
>
>    手机号接口，所以该功能不由 API 来调用，需用 **button** 组件的点击来触发。另外，新版本接口**不** 
>
>    **再**需要提前调用 wx.login 进行登录. 



### 14. 微信小程序的登陆逻辑？

> 1. 首次登录
>
>    - 调用小程序api接口 wx.login() 获取 临时登录凭证code ，这个code是有过期时间的。
>   - 将这个 code 回传到开发者服务器（就是请求开发者服务器的登录接口，通过凭证进而换取用户登录态信息，包括用户的唯一标识（openid）及本次登录的会话密钥（session_key）等）。
>    - 拿到开发者服务器传回来的会话密钥（session_key）之后，前端需要保存起来.
> 
>    ```js
>   wx.setStorageSync('sessionKey', 'value')
>    ```
> 
> 2. 再次登录的时候，就要判断存储的 session_key 是否过期了
>   - 获取缓存中的session_key， wx.getStorageSync('sessionKey')、
>    - 如果缓存中存在session_key，那么调用小程序api接口 wx.checkSession() 来判断登录态是否过期，回调成功说明当前 session_key 未过期，回调失败说明 session_key 已过期。登录态过期后前端需要再调用 wx.login()获取新的用户的code，然后再向开发者服务器发起登录请求。
>    - 一般在项目开发，开发者服务器也会对用户的登录态做过期限制，所以这时在判断完微信服务器中登录态如果没有过期之后还要判断开发者服务器的登录态是否过期。（请求开发者服务器给定的接口进行请求判断就好）。



### 15. 微信小程序弹窗被覆盖怎么解决？

> 如果弹窗被别的内容覆盖，且设置很大的z-index也无法解决，这种情况多半是被一些如 map 、 video 、 textarea 、 canvas 等原生组件遮盖，因为原生组件层级高于前端组件，我们可以使用 cover-view 组件解决。



### 16. 微信小程序关联公众号如何确定用户唯一性？

> 微信小程序中有 openid、union_id 用来标识用户角色。
>
> - openid 是同一用户同一应用唯一。
>
> - union_id 是同一用户不同应用唯一。
>
>
> 所以，想要确定用户的唯一性，只能用union_id，使用 wx.getUserInfo方法 withCredentials 为 true 时 可获取 encryptedData，里面有 union_id，需要注意的是后端需要进行对称解密。
>



### 17. 小程序安卓版和 IOS 版是如何开发出来的？

> 小程序开发基于 html、css、javascript，与web开发一样具有跨平台特性，一次开发即可在安卓和iOS等平台访问，但与普通web开发不同，小程序运行环境并不是浏览器，而是依附于各自的软件App，如微信小程序必须在微信中访问，支付宝小程序必须在支付宝中访问等，小程序的开发流程也有所不同，需要经过申请小程序帐号、安装小程序开发者工具、配置项目、开发、调试、上线发布等过程方可完成。



### 18. 小程序如果版本更新了怎么通知用户？

> 当小程序发布新的版本后，用户如果之前访问过该小程序，通过已打开的小程序进入（未手动删除），则会弹出提示，提醒用户更新新的版本。用户点击确定就可以自动重启更新，点击取消则关闭弹窗，不再更新。
>
> 1. 核心步骤
>
>    - 打开小程序, 检查小程序是否有新版本发布
>
>    ```js
>    updateManager.onCheckForUpdate(function (res) {})
>    ```
>
>    - 小程序有新版本，则静默下载新版本，做好更新准备
>
>    ```js
>    updateManager.onUpdateReady(function () {})
>    ```
>
>    - 新的版本已经下载好，调用 applyUpdate 应用新版本并重启小程序
>
>    ```js
>    updateManager.applyUpdate()
>    ```
>
> 2. 更新版本的模拟测试
>
>    微信开发者工具上可以通过「编译模式」下的「下次编译模拟更新」开关来调试。点击编译模式设置下拉列表，然后点击“添加编译模式”，在自定义编译条件弹窗界面，点击下次编译时模拟更新，然后点击定，重新编译就可以了。注: 需要注意的是，这种方式模拟更新一次之后就失效了，后边再测试仍需要对这种编译模式进行重新设置才可以。
>
> 3. 核心代码
>
>    ```js
>    App({
>      onLaunch: function (options) { this.autoUpdate() }, autoUpdate: function () {
>        var self = this
>        // 获取小程序更新机制兼容 
>        if (wx.canIUse('getUpdateManager')) {
>          const updateManager = wx.getUpdateManager()
>          //1. 检查小程序是否有新版本发布 
>          updateManager.onCheckForUpdate(function (res) {
>            // 请求完新版本信息的回调 
>            if (res.hasUpdate) {
>              //2. 小程序有新版本，则静默下载新版本，做好更新准备 
>              updateManager.onUpdateReady(function () {
>                wx.showModal({
>                  title: '更新提示', content: '新版本已经准备好，是否重启应用？', success: function (res) {
>                    if (res.confirm) {
>                      //3. 新的版本已经下载好，调用 applyUpdate 应用新版本并重启 
>                      updateManager.applyUpdate()
>                    } else if (res.cancel) {
>                      //不应用 
>                    }
>                  }
>                })
>              })
>              updateManager.onUpdateFailed(function () {
>                // 新的版本下载失败 
>                wx.showModal({ title: '已经有新版本了哟~', content: '新版本已经上线啦~，请您删除当前小程序，重新搜索打开哟~', })
>              })
>            }
>          })
>        } else {
>          // 如果希望用户在最新版本的客户端上体验您的小程序，可以这样子提示 
>          wx.showModal({ title: '提示', content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后重试。' })
>        }
>      }
>    })
>    ```



### 19. 小程序嵌入 H5 页面怎么做？

> 使用 web-view 组件来 指向网页的链接，可打开关联的公众号的文章，其它网页需登录**小程序管理后台**配置业务域名。
>
> 具体实现步骤：
>
> 1. 登陆小程序管理后台, 配置服务器域名( h5页面所在的域名 )
>
> 2. 在小程序里面嵌入h5
>
>    - 在小程序里面定义一个你想要的H5入口
>
>    ```html
>    <navigator url="/page/navigate/navigate" hover-class="navigator-hover">跳转到 新页面</navigator>
>    ```
>
>    - 新建一个页面，放置 webview , src指向h5网页的链接
>
>    ```html
>    <web-view src="{{url}}" bindmessage="getMessage"></web-view> </block>
>    ```
>
> 3. 需要注意的是： 实际开发中在h5页面中有可能需要向小程序发送消息，实现h5页面和小程序页面的通信需要使用 postMessage 向小程序发送消息， 在 h5 中 postMessage 注意，key 必须叫做 data，否则取不到。



### 20. 微信小程序传递数据方法？

> 1. 使用全局变量实现数据传递
> 2. 页面跳转或重定向时，使用url带参数传递数据
> 3. 使用组件模板 template 传递参数
> 4. 使用缓存传递参数
> 5. 使用数据库传递数据



### 21. 微信小程序中如何使用第三方组件？

> 1. 打开cmd，进入你的项目中，在cmd中执行：npm init，初始化项目
> 2. 使用 npm i -s xxx  安装对应的第三方组件
> 3. 打开小程序客户端，选择【工具】菜单 -> 选择【构建 npm】命令



### 22. 如何阻止小程序的事件冒泡？

> 在小程序中除了通过bind之外，还可以通过catch进行事件绑定，通过catch绑定的事件不会触发事件冒泡。



### 23. 如何让事件在捕获阶段触发？

>  事件的触发分为两个阶段，首先是捕获阶段，其次是冒泡阶段。默认情况下事件都是在冒泡阶段触发。如果希望事件可以在捕获阶段触发，可以通过 capture-bind 进行事件绑定。



### 24. UniAPP 优缺点有哪些？

> 优点：
>
> 1. 一套代码可以生成多端
> 2. 学习成本低,语法是vue的,组件是小程序的
> 3.  拓展能力强
> 4.  使用HBuilderX开发,支持vue语法
> 5.  突破了系统对H5调用原生能力的限制
>
> 缺点:
>
> 1. 问世时间短，很多地方不完善
> 2. 社区不大
> 3. 官方对问题的反馈不及时
> 4. 在Android平台上比微信小程序和iOS差
> 5. 文件命名受限



### 25. UniAPP 是如何做到多端兼容的？

> uni-app 已将常用的组件、JS API 封装到框架中，开发者按照 uni-app 规范开发即可保证多平台兼容，大部分业务均可直接满足，但每个平台有自己的一些特性，因此会存在一些无法跨平台的情况。
>
> - 大量写 if else，会造成代码执行性能低下和管理混乱。
> - 编译到不同的工程后二次修改，会让后续升级变的很麻烦。
>
> 在 C 语言中，通过 #ifdef、#ifndef 的方式，为 windows、mac 等不同 os 编译不同的代码。 `uni-app` 参考这个思路，为 `uni-app` 提供了条件编译手段，在一个工程里优雅的完成了平台个性化实现。

> 条件编译是用特殊的注释作为标记，在编译时根据这些特殊的注释，将注释里面的代码编译到不同平台。
>
> **写法：**以 #ifdef 或 #ifndef 加 **%PLATFORM%** 开头，以 #endif 结尾。
>
> - `\#ifdef`：if defined 仅在某平台存在
> - `\#ifndef`：if not defined 除了某平台均存在
> - **%PLATFORM%**：平台名称

> **%PLATFORM%** **可取值如下：**
>
> | 值                      | 平台                                                         |
> | :---------------------- | :----------------------------------------------------------- |
> | APP-PLUS                | App                                                          |
> | APP-PLUS-NVUE           | App nvue                                                     |
> | H5                      | H5                                                           |
> | MP-WEIXIN               | 微信小程序                                                   |
> | MP-ALIPAY               | 支付宝小程序                                                 |
> | MP-BAIDU                | 百度小程序                                                   |
> | MP-TOUTIAO              | 字节跳动小程序                                               |
> | MP-QQ                   | QQ小程序                                                     |
> | MP-360                  | 360小程序                                                    |
> | MP                      | 微信小程序/支付宝小程序/百度小程序/字节跳动小程序/QQ小程序/360小程序 |
> | QUICKAPP-WEBVIEW        | 快应用通用(包含联盟、华为)                                   |
> | QUICKAPP-WEBVIEW-UNION  | 快应用联盟                                                   |
> | QUICKAPP-WEBVIEW-HUAWEI | 快应用华为                                                   |



### 26.  UniAPP 开发过程中，都遇到哪些坑？

> 1. 上传图片的时候。
>
>    小程序时必须要写 `header:{“Content-Type”: “multipart/form-data”}`， h5是必须省略。
>
> 2. UniAPP h5 端图片在 IOS 环境下加载不出来。
>
>    UniAPP 在 IOS 环境下，只能加载 https 的图片。
>
> 3. **uni-app** 使用 deep 穿透微信小程序生效 h5 无作用 ，需要在methods同级下加一个 ： `options: { styleIsolation: ‘shared’ }`。
>
> 4. UniAPP 在开发中我们接口上传图片是 post 请求，无法传递一个数组，我们可以把数据转换成字符串 然后拼接到请求地址后后面，拼接字符串格式：`image[]=arr[0]&image[]=arr[1] `
