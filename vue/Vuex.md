# vuex 学习



## 下载挂载到项目内

- 下载 

  ```shell
  $ npm install vuex
  ```

- 在项目内创建一个 `存储库`

  - 新建一个文件夹叫做 `store`
  - 新建一个文件叫做 `index.js`

- 在 `index.js` 文件内开始进行 `vuex` 的书写



## 创建一个仓库

- 需要用到的方法是 `Vuex.createStore({ /* 配置项 */ })`

  ```js
  // 导入 vuex 第三方
  import { createStore } from 'vuex'
  
  // 创建存储库
  const store = createStore({
      /* 配置项 */
  })
  
  // 导出存储库
  // 为什么要导出 ? 因为我要把 存储库 挂载到 vue 身上
  //   才可以每一个组件都使用这个存储库内的数据
  export default store
  ```



## 把存储库挂载到 Vue 身上

- 来到 `main.js` 文件

- 导入我们写好的 存储库

  ```js
  import store from '@/store'
  
  // ...
  
  // 挂载到 app 身上
  app.use(store)
  ```

- 当你将仓库挂载到 `Vue` 的实例身上以后

- 会向全局添加一个属性叫做 `$store`

  - 在任何一个位置都可以直接使用
  - 拿到的就是你设置的存储库



## state

- 在你创建的 `vuex` 存储库中, 可以添加一个 `state` 配置项

- 表示你要存储的共享的数据

  > 等价于我们组件内的 data
  >
  > 语法:
  >
  > ​	state () {
  >
  > ​		return {
  >
  > ​			// ...
  >
  > ​		}
  >
  > ​	}

- 在组件内如果想拿到 `state` 中的数据

  ```js
  this.$store.state.xxx
  ```

- 需要把这个 `state` 内的数据, 拿到我当前组件内使用

  - 方式1: 直接给 `data` 内的数据赋值

    ```js
    data () {
        return {
            str: this.$store.state.xxx
        }
    }
    ```

    - **不推荐** 这样使用 : 当 `state` 内的数据变化的时候, 组件内的数据不会跟随变化

  - 方式2 : 利用 `computed` 的方式获取数据

    - 特点: 当被计算项发生变化的时候, 会从新获取数据的值, 计算出最新的值

    ```js
    computed: {
        str () { return this.$store.state.xxx }
    }
    ```

    

## mutations

- 也是 `vuex` 内的一个配置项, 这里存储的都是方法

- 方法是专门用来修改 `state` 内数据的方法

  ```js
  {
      state () { /* ... */ },
      mutations: {
          方法1 () { /* ... */ }
      }
  }
  ```

  - `mutations` 内每一个方法, 接受两个参数
  - 参数1: 就是当前的 `state`
  - 参数2: 调用 `commit` 的时候, 给 `mutations` 内对应方法传递的参数
  - 注意: `commit` 只能接受两个参数, 如果你想给函数多个信息, 需要把多个信息包裹在一个数据结构内传递

- 当你需要修改 `state` 内的数据的时候

  - 只要想办法去调用 `mutations` 内的 方法 即可

- 组件内如何调用 `mutations` 内的方法

  - 语法 : `this.$store.commit('方法名')`
  - 相当于把 `mutations` 内的方法调用了一下

- 问题 : 当我们进行异步修改的时候, 会导致 `vuex` 记录出现问题( 记录的永远是上一次 )

  > `mutations` 内方法都做了什么事情
  >
  > 	1. 当你 `commit` 这个方法的时候, 接受形参
  > 	1. 把你书写在方法内的代码执行掉
  >		
  > 	3. 把当前的数据状态记录下来( 就是记录现在的数据是什么 )
  >
  > 当你在 `mutations` 内的方法内, 执行一个异步代码的时候
  >
  > 	1. 当你 `commit` 这个方法的时候, 接受形参
  > 	2. 把你书写在方法内的代码执行掉
  > 	- 这里执行了一个异步代码
  > 	- 会把代码放在队列池内等待
  > 	- 继续向下执行同步代码
  > 	3. 把当前的数据状态记录下来( 就是记录现在的数据是什么 )
  > 	4. 至此, 所有同步代码完毕, 然后拿出来执行异步代码
  > 	- 此时才把数据修改

- 注意 : 使用 `mutations` 的时候, **不要书写异步代码**



## actions

- 也是 `vuex` 内的一个配置项, 这里存储的都是方法

- 专门用来调配 `mutations` 内方法的方法

  ```js
  {
      state () { /* ... */ },
      mutations: {
          方法1 () { /* ... */ }
      },
      actions: {
          方法1 () { /* .... */ }
      }
  }
  ```

  - `actions` 内每一个方法, 接受两个参数
  - 参数1: 就是当前的 `store` 存储库
  - 参数2: 调用 `dispatch` 的时候, 给 `mutations` 内对应方法传递的参数
  - 注意: `dispatch` 只能接受两个参数, 如果你想给函数多个信息, 需要把多个信息包裹在一个数据结构内传递

- 在组件内调用 `actions` 内的方法
  - 语法 : `this.$store.dispatch('方法名', '参数')`
- 结论: 
  - 如果你想同步修改数据, 直接用 `mutations` 内的方法, 在组件内用 `commit` 触发
  - 如果你想异步修改数据, 直接用 `actions` 内的方法, 在组件内用 `dispatch` 触发



## getters

- `vuex` 内提供的一个配置项

- 就是对 `state` 内的数据进行处理的

  > 等价于我们组件内的 computed

- 书写的语法, 也是和 `computed` 内的语法一样

- 在组件内使用 `getters` 内的属性方式和 `state` 语法一模一样



## modules

- 也是 `vuex` 内的一个配置项

- 就是把数据分门别类做成一个一个的小仓库

- `modules` 内可以书写的配置项, 和我们初始仓库书写的一模一样

- 在初始仓库内用 `modules` 配置项, 把小仓库引入进来

  ```js
  {
      ...,
      modules: {
        名字: { /* 小仓库配置项 */ }  
      }
  }
  ```

- 特点1: ( 仅限于 state )

  - 会把你总仓库 和 所有小仓库 的 `state` 的内容全部拿出来, 进行组装

    ```js
    {
        message: '总',
        users: {
            message: 'users'
        },
        goods: {
            message: 'goods'
        }
    }
    ```

- 特点2

  - 当你的 `mutations` 或者 `actions` 内有重名方法的时候
  - 一旦调用大家都会被执行

- 模块的独立命名空间

  - 当你给每一个模块开启的独立命名空间以后
  - 每一个小仓库内的方法就是独立方法了, 重名也无所谓
  - 添加 `namespaced` 属性, 值为 `true`

- 如何调用命名空间内的方法

  - 语法 : `this.$store.commit('命名空间/方法名')`
  - 没有命名空间是不能使用这个 `命名空间/方法名` 



## 映射语法

- 说明 : 

  - `state` 和 `getters` 的映射语法是一样的
  - `mutations` 和 `actions` 的映射语法是一样的

- 使用的内容

  - `state` => `mapState`
  - `getters` => `mapGetters`
  - `mutations` => `mapMutations`
  - `actions` => `mapActions`
  - 以上这些映射方法都是从 `vuex` 内解构出来

- `mapState` 和 `mapGetters`

  - 需要从 `vuex` 内解构出内容来使用

  - 作用: 专门用来映射 `state` 内的所有数据的

  - 使用语法1: `mapState([ '数据1', '数据2' ])`

    > 参数 : 数组内的每一个名字, 就是 state 内的数据
    >
    > 返回值 : 是一个对象, 里面是一个一个的函数, 分别对应每一个数据的映射函数
    >
    > 例子 : 
    >
    > ​	const res = mapState([ 'str' ])
    >
    > ​	res === {
    >
    > ​		str () { return this.$store.state.str }
    >
    > ​	}
    >
    > 只要把返回值内的内容一个一个的添加到 computed 内, 就可以直接使用了

  - 使用语法2 : `mapState({ 别名: '数据1', 别名: '数据2' })`

    > 从 state 内映射出来的数据, 起一个别名
    >
    > 为了避免和自己组件内的数据冲突

  - 映射带有命名空间的数据

    ```js
    // 方式一:
    // {
    //     ...mapState({
    //         名字: state => state.命名空间.xxx
    //     })
    // }
    ```

    ```js
    // 方式二:
    // {
    //     ...mapState('命名空间', {
    //         名字: '数据'
    //     })
    // }
    // {
    //     ...mapState('命名空间', [ '数据1' ])
    // }
    ```

- `mapMutations`

  - 需要从 `vuex` 内解构出内容来使用

  - 作用: 专门用来映射 `state` 内的所有数据的

  - 注意: 映射出来的内容是存储在 `methods` 内

  - 使用语法 : `mapMutations([ '方法名' ])`

    > 例子 : 
    >
    > ​	const res = mapMutations(['changeStr'])
    >
    > ​    res === {
    >
    > ​		changeStr (state) { state.str = 'xxxx' } /* 其实拿到的是 store 内对应这个方法的 引用 */
    >
    > ​	}
