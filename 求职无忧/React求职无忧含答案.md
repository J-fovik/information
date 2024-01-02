## React求职无忧含答案

### 1、谈谈你对 JSX 语法的理解？为什么要使用 JSX？

> - JSX其实是react.createElement的语法糖，同时也是js的语法扩展，包含了所有js功能，是javascript和XML结合的一种语法格式。
> - React发明了JSX，利用HTML语法来创建虚拟DOM。当遇到标签符号时 <> ，JSX就当HTML解析，遇到花括号时 {} 就当JavaScript解析；
>
> JSX的优点：
>
> 1. JSX这种树状结构可读性强，代码更简洁；
> 2. JSX会将XML语法直接加入JS中,通过代码而非模板来高效的定义界面。在实际开发中，JSX在产品打包阶段都已经编译成纯JavaScript，JSX的语法不会带来任何性能影响；
> 3. JSX执行更快，因为它在编译为 JavaScript 代码后进行了优化



### 2、什么是高阶函数？什么是高阶组件？谈谈你在工作中都使用过哪些？

> - 高阶函数就是在函数中传入另外一个函数作为参数，或者将函数作为返回值的函数。map、filter、reduce、forEach、some等等都属于高阶函数；
> - 高阶组件(HOC)是 React 中复用组件逻辑的一种高级技巧。HOC自身不是 ReactAPI的一部分,它是一种基于 React 的组合特性而形成的一种设计模式。就是接受一个组件并返回另外一个新组件的函数。本质上和高阶函数的意思一样；
> - 使用高阶组件的目的是为了提高组件复用率，且分为属性代理和反向继承。
>
> 使用：
>
> 1. 使用ref分发获取子组件内部DOM节点；
> 2. 函数式组件的 ref 转发，使用 useRef 获取DOM；
> 3. 使用 useMemo 缓存组件，类似于我们的计算属性，useMemo 和 useEffect 非常相似，都是监听数据的变化，useMemo 会触发新的返回值。



### 3、什么是受控组件？什么是受控表单？

> - 受控组件就是可以被 react 状态控制的组件，就是通过 onChange 事件获取当前输入内容，将当前输入内容作为 value 传入，此时就成为受控组件；
> - 受控表单指的是所有的表单状态由 state 进行管理的表单，通过 setState 触发状态更新；
>
> 其优点是：都可以通过 onChange 事件控制用户输入，使用正则表达式过滤不合理输入。



### 4、React 的生命周期都有哪些？React Hooks 如何模拟生命周期？

> 首先，React组件定义的方式有两种，分别是类组件和函数组件，类组件有生命周期，函数组件必须依靠 HOOKS 编程才能有生命周期；
>
> - 在我们开发中，类组件中常使用的一些生命周期有 
>   1. constructor ：初始化state，以及为事件处理程序绑定this
>   2. componentWillMount ：组件被挂载在DOM之前调用，而且只会被调用一次
>   3. render ：实际上是调用React.creatElement() 来渲染UI；
>   4. componentDidMount ：组件被挂载在DOM之后调用，而且只会被调用一次，往往用来服务器端发送网络请求，以及处理依赖DOM节点的操作；
>   5. componentDidUpdate ：组件更新后被调用；
>
> - 函数组件 的本质是函数，没有 state 的概念的，因此不存在生命周期一说；
>
>   但是引入 Hooks 后能让组件在不使用 class 的情况下拥有 state，所以就有了生命周期的概念；
>
>   所谓的生命周期其实就是使用 useEffect() 来进行模拟，称为副作用函数
>
>   1. 该方法接收两个参数，当第二个参数不接收，代表监听所有的响应式对象数据（props），相当于 componentDidUpdate，在我们平时开发中很少用；
>   2. 当第二个参数为空数组时，代表不监听任何的响应式数据对象，相当于componentDidMount；
>   3. 当第二个参数接收具体的响应式数据对象（包含 props），相当于 watch 监听数据；
>   4. 在 useEffect 中返回一个函数，函数中为组件即将卸载前要做的操作



### 5、谈谈 React 响应式对象的更新方法和更新机制？

> 响应式对象的更新方法：
>
> 1. 在类组件中，可以使用 this 的 setState 方法来修改定义的响应式数据对象；
>
> 2. 而在 Hooks 编程中，没有 this 对象，可以使用 useState 来创建响应式数据对象与修改；
>
> 3. 这两种方法的共同点是：
>
>    ① 响应式数据对象的数据都是异步的；
>
>    ② 对响应式数据进行多次修改时只有最后一次生效，其原因是会先进行采集依赖，再根据diff算法进行修改；
>
>    ③ 都可以通过回调函数来实现多次修改的效果，其原因是回调会写进队列，在异步更新的时候根据队列进行排队调用。
>
> 更新机制：React 框架在接收到用户状态改变通知后，会根据当前渲染树，结合最新的状态改变，通过 Diff 算法进行更新。
>
> 1. 通过 Diff 算法计算 DOM 树变化的部分，只更新变化的部分，从而避免整棵 DOM 树重构，提高性能；
> 2. 对数据和状态所做的任何改动，都会被自动且高效的同步到虚拟DOM然后进行采集依赖，将最后一次需要修改的数据同步到真实DOM中，而不是每次改变都去操作一下DOM。



### 6、谈谈你对 React 合成事件的理解？

> React 事件处理使用小驼峰方式，名字虽然和原生DOM长的很像，但是 React 进行了自己的封装操作处理，拿onClick点击事件来举例。
>
> 1. 当用户在为onClick添加函数时，React并没有将 Click 事件绑定在DOM上面；
> 2. 而是在document处监听所有支持的事件；发生并冒泡至document处时，React将事件内容封装交给中间层SyntheticEvent（负责所有事件合成）;
> 3. 所以当事件触发的时候，对使用统一的分发函数dispatchEvent将指定函数执行。
>
> 但由于class的方法默认不会绑定this，如果没有绑定this，调用this的时候为underfined，解决方法为：
>
> 1. 在构造函数 constructor 使用 bind 绑定 this；
> 2. 调用的时候使用 bind 绑定 this（不推荐，每次渲染都会重新绑定）；
> 3. 使用箭头函数。



### 7、React 中 useEffect、useLayoutEffect、useCallback、useMemo 都有什么区别？一般都是如何使用的？

> - useCallback 和 useMemo  都是性能优化的手段，他们的参数跟 useEffect 一致 ，类似于类组件中的shouldComponentUpdate ，但 useEffect 会用于处理副作用，而前两个hooks不能；
> - useCallback 和 useMemo 都会在组件第一次渲染的时候执行，之后会在其依赖的变量发生改变时再次执行，类似 vue 中的计算属性；
>   1. useCallback优化针对于子组件渲染，返回值是函数。
>   2. useMemo优化针对于当前组件高开销的计算。返回值是缓存的变量。
>
> - useEffect 和 useLayoutEffect 都是官方拿来替代 componentDidMount / componentDidUpdate / componentWillUnmount 这三个生命周期函数的 hook。
>   1. useEffect 是在浏览器重绘之后才异步执行的；而 useLayoutEffect 是在浏览器重绘之前同步执行的。
>   2.  useEffect 不会阻塞浏览器重绘，因此平时业务中都是使用 useEffect 来进行处理，性能上会表现的更好一些。
>
> 使用场景：
>
> 1. 使用 useCallback 缓存 render 中定义的方法；
> 2. 使用 useMemo 缓存 render 中定义的一些变量或者计算后的变量；
> 3. 使用 useEffect 来模拟生命周期。



### 8、谈谈在 React 中你对 Ref 的理解和使用？

> - 我们在使用 React 开发时对 Ref 的使用比较少，因为我们很少直接操作底层DOM元素，而是通过在 render 里编写我们的页面结构，再由React来组织DOM元素的更新。
> - 而在少数情况中，有些业务需求要求我们对页面的真实DOM进行直接操作，而使用 Ref 直接访问到 DOM 节点。若 Ref 绑定在组件上，可获取到当前组件的实例化。
>
> 使用方法：① 字符串形式调用；② 回调形式调用；③使用 React.createRef() 创建，并通过ref属性附加到React元素。
>
> - 而在实际场景中我们使用 Ref 直接绑定组件，获取最高权限，显然不太合适。
> - 针对有些场景，我们需要在父组件中，拿到子组件的 DOM 节点，可以使用 Ref分发 forwardRef 来进行获取。
> - 但仍然获取的是 DOM 的最高权限，遇上 react 推出了 useimperativehandle 透传 ref的方法来解决ref获取 DOM 最高权限的问题。



### 9、谈谈 React 类组件 Component 和 Purecomponent 区别？函数组件同理的优化策略是？

> 区别：
>
> 1. 两者最大的区别是 PureComponent 会自动执行 shouldComponentUpdate 函数，通过对 props 和 state 进行浅比较，实现react的性能优化；而 Component 必须要通过自己去调用生命周期函数 shouldComponentUpdate 来实现react组件的优化。
> 2. PureComponent不仅会影响本身，而且会影响子组件，所以PureComponent最佳情况是展示组件；
> 3. 如果是复杂数据类型，使用 PureComponent 页面不会重新渲染；而使用Component可以正确显示。
>
> 函数组件同理的优化策略：
>
> 1. 使用 useCallback 可以避免子组件的重复渲染
> 2. 使用 React.memo 包裹子组件也可以避免造成不必要的重复渲染；
> 3. useMemo 用于计算结果缓存。



### 10、谈谈你对 React 中组件之间的通信的理解？

> - 基于父子组件通信，父组件通过 props 向子组件通信，子组件通过回调函数事件将子组件的值传回父组件；
> - 基于跨级组件通信，常用的方式有两种，逐层传值与跨层传值：
>   1. 逐层传值：就是父子通信的基础上在加上一个中间层，数据通过 props 自上而下进行传递，再通过回调上传，比较繁琐。
>   2. 跨层传值：使用 Context 传值共享数据。本质上来讲，Context 是数据提供者和数据消费者的概念。
>
> - 基于兄弟组件通信，我们可以使用 event、pubsub-js等第三方插件来实现通信功能。
> - 当然，我们也可以使用 localStorage 本地储存数据的方法，来实现通信。



### 11、谈谈你对 Redux 实现原理的理解？什么是 Redux 三个原则？

> redux其实就是一个单独的状态管理工具，可以进行存储公共数据并进行修改。其核心内容有：
>
> 1. store是 redux 的核心内容，整个 redux 的仓库，所有的应用方法都是由 store 提供；
> 2. createStore 用来创建 store ；
> 3. reducer 函数相当于数据加工厂，初始的数据，修改数据的逻辑都统统的在这里完成；
> 4. state 存放公共数据，action 存放修改公共数据的方法，然后通过 store.getState().count 读取值，用 store.dispatch() 操作方法。
>
> Redux 三个原则：
>
> - 单一数据源
>   - 整个应用的`state`（这个state不是组件中的state）被储存在一棵对象结构树中，并且这个对象结构只存在于唯一一个store中
> - State是只读的
>   - 唯一改变state的方法就是触发action，action是一个用于描述已发生事件的**普通对象**。
> - 使用**纯函数**（一个函数的返回结果只受到其形参的影响，则其就是纯函数）来执行修改
>   - 为了描述action如何改变state tree ，我们需要编写reducer，reducer必须是纯函数，它接收先前的state和action，并返回新的state
>   - 纯函数是函数式编程的概念，必须遵守以下一些约束。
>     - 不得改写参数
>     - 不能调用系统 I/O 的API
>     - 不能调用Date.now()或者Math.random()等不纯的方法，因为每次会得到不一样的结果



### 12、useEffect 的依赖为引用类型的时候，如何确保有效的监听？

> 因为当使用 useEffect 进行监听时，是通过依赖的值变化才会重新调用执行函数；而当依赖为引用类型时，比较的是两者的地址而非值，则会引发无限渲染的问题。
>
> 1. 使用 useMemo 这个 hook 方法，能帮助我们缓存数据；
> 2. 使用 useCallback 这个 hook 方法，能帮我们缓存一个函数。当某个依赖改变时，返回一个函数，在依赖没有改变时，useCallback 会缓存第一次返回的函数；
> 3. 若数据是对象，可以监听对象里面的值，能够避免监听引用类型。



### 13、谈谈你对 React Render 渲染机制的理解？

> - React整个的渲染机制就是React会调用[render](https://so.csdn.net/so/search?q=render&spm=1001.2101.3001.7020)()函数构建一棵Dom树；
>
> - 在state/props发生改变的时候，render()函数会被再次调用渲染出另外一棵树，重新渲染所有的节点，构造出新的虚拟Dom tree跟原来的Dom tree用Diff算法进行比较，找到需要更新的地方批量改动，再渲染到真实的DOM上，由于这样做就减少了对Dom的频繁操作，从而提升的性能；
>
> 我们在使用react渲染的时候，render()函数会有以下作用：
>
> 1. 将虚拟DOM树渲染到真实的DOM树上；
> 2. 将任务交给浏览器；
> 3. 进行layout（布局）和paint（绘制）等操作；



### 14、谈谈你对 React Key 的理解？项目中，你都是如何使用 key 的？

> - React 利用 key 来识别组件，他是一种身份标识，当数据发生增删改查的时候可以通过该标识快速精准的找到所需修改的DOM节点。能减少页面重绘和回流的频率，进而提高页面更新的效率；
>
> - 在我们 react 的开发中，对一些遍历出来的组件没有添加 key 值时，会在控制台报出 warning 警告，提示我们需要添加 key 属性。
> - 要注意的一点是，在 react 中不建议使用 index 作为 key ，原因是：
>   1. 若对数据进行逆序添加、逆序删除等破坏顺序操作时，会产生没有必要的真实DOM更新；
>   2. 当使用多个输入框的时，使用 index 作为输入框的 key 值时，会产生错误 DOM 更新。



### 15、谈谈什么是 React createPortal？你都是如何使用的？

> - Protals 则提供了一种将组件直接挂载到直接父组件 DOM 层次之外的一类方式；
>
> - 因此 Portals 适合脱离文档流(out of flow) 的组件，特别是 position: absolute 与 position: fixed的组件。比如模态框，通知，警告，goTop 等；
> - createPortal函数主要有三个参数，分别是children（需要渲染的组件）、container（需要渲染到的指定节点）、key。开发中我们主要关注children和container即可。
>
> 使用场景：平时开发中，使用 createPortal 主要用来创造遮罩层，也可以使用来做弹窗 Modal 组件。



### 16、谈谈你对 React Fiber 虚拟  DOM 实现原理理解？Diff 算法实现的理解？

> -  Fiber是react执行渲染时的一种新的调度策略，是虚拟DOM的一种实现方式，帮助我们管理DOM更新的优先级。
> - fiber是个链表，有type，props，return，child 和 sibing 属性，指向第一个子节点和相邻的兄弟节点，从而构成 fiber tree；return 属性指向其父节点。
>
> - 因为JavaScript是单线程的，一旦组件开始更新，主线程就一直被React控制，当js在处理大型计算的时候会导致页面出现卡帧的现象，更严重的会出现页面“假死”；而 Fiber 能将大型的计算拆分成一个个小型计算，然后按照执行顺序异步调用，这样就不会长时间霸占线程，UI也能在俩次小型计算的执行间隙进行更新，从而给与用户及时的反馈。
>
> Diff 算法实现：
>
> 1. Diff 算法是一种对比算法，通过对比新旧虚拟 DOM 从而找出需要修改的 DOM 节点，并且能实现快速精准的更新 DOM 从而提高效率；
>
> 2. Dif算法的特点：
>
>    ① Diff 算法在进行比较时，会根据树形结构从上往下一层一层进行查找，当发现需要更新的数据，从而进行局部更新。
>
>    ​	举个例子，在我们开发中常常会在组件中添加key属性，是为了添加一个标识，并且能快速精准的找到所需修改的DOM节点。能减少页面重绘和回流的频率，进而提高页面更新的效率。
>
>    ② 当对同一个响应式数据对象进行多次修改时，DOM不会立即更新而是将若干次的更新保存到 js 对象中，进行采集依赖，将最后一次修改的数据渲染到DOM树上。



### 17、谈谈你在 React 中如何使用 CSS？类似 Vue scoped 模块化样式实现原理？

> 在 React 中使用css：
>
> 1. 直接在组件中书写。需注意：有连字符的属性需转化成小驼峰写法，css 的值不需要引号。
> 2. 单独写入 css 文件中，在页面中导入。
> 3. 安装 sass 编译环境，将样式写入 scss 文件中，然后在页面中导入。
> 4. 在组件中引入 .module.css 样式文件。
>
> 模块化样式实现：
>
> 1. 在 React 中若想实现 Vue 的scoped 模块化样式，可以使用 webpack 的一个插件 css-loader，并配合 style-loader 一起使用。
> 2. 可以把样式单独封装成一个模块，css 文件命名使用 .module.css的方式，并且用 className 赋予类名，可以避免类名冲突。



### 18、谈谈 React 如何实现类似 Vue 插槽、具名插槽的效果？

> - 在 React 中可以使用 props.children 来实现类似 Vue 插槽的效果，props.children 可以获取组件实例的innerHTML，还可以为其传入参数，以控制内部标签；
> - 可以通过 props.children 的下标去访问从而实现类似 Vue 具名插槽的效果。



### 19、谈谈 React 中路由传递参数的方法？

> 1. 使用动态路由 params 传参，使用 / 的方式在地址 URL 上拼接参数只能传字符串，但不能传递对象，参数多会导致 URL 过长;
>
> 2. 使用 search 传参，
>
>    ① 基于声明式导航，以键值对的方式在地址 URL 上使用 ？的方式进行拼接；
>
>    ② 基于编程式导航直接使用 search 进行传值；
>
>    ③ HashRouter 模式刷新地址栏，参数丢失，BrowserRouter 模式不会丢失参数；
>
> 3. 也可以基于编程式导航进行隐式传参，此方法在 Vue 中刷新页面传入的参数会丢失，但在 React 中刷新页面不会丢失；
>
> 4. 使用 query 进行传参，但刷新页面时会参数丢失，所以不推荐使用；
>
> 可以使用 navigate(-1) 跳转回上一步



### 20、日常工作中 React 常用的优化手段有哪些？

> 1. 路由懒加载；
> 2. 列表渲染的时候加 key；
> 3. 在函数组件中使用 useCallback 和 useMemo 来进行组件优化，依赖没有变化的话，不重复执行；
> 4. 在事件绑定中，使用 constructor 中 bind 事件与定义阶段使用箭头函数绑定这两种形式只会生成一个方法实例，性能方面会有所改善；
> 5. 使用 React Fragments 空标签避免额外标记；
> 6. 避免使用内联函数（每次调用 render 函数时都会创建一个新的函数实例）



### 21、谈谈你对 RTK 状态管理过程的使用理解？

> 1. 首先初始化创建状态管理器；
> 2. 在最顶层的页面 index.tsx 中使用<Provider>组件包裹 App，并传递 store；
> 3. 使用 createSlice 创建切片，包含 name 标识切片、初始 state 以及存放修改初始 state 数据方法的 reducer 函数。
> 4. 不同的模块创建不同的切片，并且放入状态管理器 reducer 中进行统一管理；
> 5. 在 React 组件中使用 useSelector 钩子函数从store 中读取数据，使用 useDispatch 钩子函数获取 dispatch 函数进行修改数据。



### 22、谈谈你对 双向数据绑定 和 单向数据流的理解？

> - React 遵循从上到下的数据流向，即为单向数据流；
> - 而提到双向数据绑定，大家想到的可能会是 Vue 框架，因为 Vue 使用的是 MVVM 设计模式，其核心思想就是使用 Viewmodel 把 view 和 model 结合起来的一个双向数据绑定；
> - 和 Vue 相比 React 并没有提供向 v-model 这样的指令来实现文本框的数据流双向绑定，因为react的设计思路就是单向数据流，所以我们需要借助 onChange 和 setState 来实现一个双向的数据流，当数据发生变化的时候，用户界面发生相应的变化，双向数据绑定，带来双向数据流；
> - 虽然 Vue 有双向绑定 v-model，但是 Vue 和 React 父子组件直接传递数据，仍然还是遵循单向数据流的，父组件可以向子组件传递 props，但是子组件不能修改父组件传递过来的 props，子组件只能通过事件在父组件进行数据更改。



### 23、redux-thunk  是干什么的，为什么要用这个？具体怎么实现？

> - redux-thunk 是一个中间件，专门用来解决 action 中的异步处理；
> - 使用 redux-thunk，我们需要手动去调用 dispatch，这样我们可以在执行异步修改 state 的时候能让 state 的值变得可控。不使用 redux-thunk，我们就不需要手动去调用dispatch，而是自动调用 dispatch 来修改 state ，不适合异步修改 state；
> - 实现过程：
>   1. 安装 thunk
>   2. 创建store的时候将引入中间件 thunk ，放在 applyMiddleware 方法之中
>   3. 上述步骤完成了对 store.dispatch() 的功能增强，即可以在reducer中进行一些异步的操作



### 24、使用过 Context 上下文不？在 React Redux 中是如何使用的？

> - 使用过，Context 主要是用来实现不同组件的通信，无需一层一层传递 props 的方式来进行数据传递。
>
> 使用 useReducerContext 模拟实现轻型的 Redux：
>
> 1. 创建初始化数据
> 2. 创建 reducer，存放修改数据的方法
> 3. 使用 createContext 初始化上下文
> 4. 基于 useReducer 管理用来消费的数据
> 5. 基于 context 上下文，父组件提供数据消费，数据是 useReducer Hook 后的 state, dispatch
> 6. 在子组件中基于 context 上下文，获取数据消费



### 25、谈谈什么是 类组件？什么是函数组件？二者有什么区别？

> 类组件：react 创建类组件的方式就是使用类的继承，ES6的语法支持使用 class 来定义一个类，并使用了 ES6 标准语法构建类组件。
>
> 函数组件：使用 javascript 函数构建函数组件，且组件的首字母必须大写，必须返回 jsx 代码。
>
> 二者区别：
>
> - 组件的定义方式不同。
> - 生命周期不同：类组件有；函数式组件没有，但是引入 hook 后可以使用副作用函数 useEffect 来模拟生命周期函数。
> - state的定义、读取、修改方式不同：函数组件用 hook 的 useState，类组件可以直接定义并且使用 this.setState 的方法修改数据。
> - this： class组件有，函数式组件没有。
> - 实例： class组件有，函数时组件没有。
> - ref使用不同：类组件可以获取子组件实例，函数式组件不可以，因为函数式组件没有实例。



### 26、在你之前的项目中使用过 Ant Design UI 框架了没？如何自定义修改样式？

> 使用过。
>
> 1. 直接在标签中使用内联样式进行修改
> 2. 在样式的外部添加 :global，然后在global中修改 ant-design 组件的样式
> 3. 自定义定制主题



### 27、你怎样理解 “在React中，一切都是组件 ” 这句话的？

> 在使用 React 开发中，我们可以将整个 UI 分成小的独立并可复用的组件，并且每个组件彼此独立互不干扰。
>
> - React采用组件化的思想，最小的组件单位就是原生HTML元素，采用JSX的语法声明组件的调用
> - React的虚拟DOM，就是一个大的组件树，从父组件层到子组件，在render函数中层层堆叠
> - 从react-router 版本4.0开始，路由本身也是组件
> - 各个库提供的 hoc 返回的也是组件，如 useMemo、connect
> - React中的基础数据state props的传递也是以组件为基础



### 28、谈谈你对 React Router 使用流程的理解？Router6 都有哪些变化？

> 在React 中使用 react-router ，专门用来实现 SPA 应用的，通过切换组件来展示不同的页面内容，且不会刷新页面，当点击路由时只会做页面的局部更新。
>
> - Router类型分为 HashRouter 和 BrowserRouter 模式
>
> - Route 用于路径的匹配，然后进行组件的渲染，对应的属性有：
>
>   ① path：匹配的路径
>
>   ② element：展示的组件
>
> - Link/NavLink组件：通常路径的跳转是使用 Link 组件，最终会被渲染成 a 元素，其中属性 to 代替 a 标签的 href 属性
>
> Router 6的变化：
>
> 1. 内置组件的变化: 
>
>    ① 移除了 <Switch /> ，新增 <Routes />来替代
>
>    ② 删除了<redirect> ，使用<Navigate>来重定向
>
> 2. 语法的变化：Route 展示组件使用 element 属性，不再是component
>
> 3. 新增多个hook:  useParams，useNavigate 等；
>
> 4. 明确推荐使用函数式组件
