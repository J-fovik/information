

# 一 . vue3-Composition-api

## 过渡阶段的一种写法:

setup 选项式函数

ref () 响应式函数得到使用 

ref 语法原理使用的时候 vue2 中的object.defineProperty

```js
<template>
  <div>
    <!--name这个ref 引用对象在使用时不需要加value,vue3会自动帮你加value,所以可以拿到值-->
    <p>{{ name }}--{{ age }}</p>
    <p>{{ job.type }}--{{job.salary}}</p>
    <p @click="change">说话</p>
  </div>
</template>
<script>
import { ref } from "vue"; // 引入响应式函数ref
export default {
  name: "App",
  setup() {
    let name = ref("张三"); //返回一个 ref 引用对象
    let age = ref(20);
    console.log(name)
    // 当ref传的参数为对象时
    let job = ref({
      type: "前端工程师",
      salary: "20k",
    });
    function change() {
      name.value = "李四"; // ref对象.value 修改其值
      age.value = 30;
      job.value.type = "后端开发工程师";
      job.value.salary = "30k";
    }
    return {
      name,
      age,
      change,
      job
    };
  },
};
</script>
```

reactive 响应式函数的使用

实现响应式的原理是根据  vue3中的proxy 实现的响应式

```js
<template>
  <p>{{ job.type }} --{{ job.salary }}--{{ job.like.a.b }}--{{job.content}}</p>
  <p v-for="(item, index) in foodArr" :key="index">{{ item }}</p>
  <button @click="changeFn">changeFn</button>
</template>

<script>
import { reactive } from "vue";
export default {
  name: "App",
  components: {},
  setup() {
    //定义对象
    let job = reactive({
      type: "前端工程师",
      salary: "20k",
      like:{
          a:{
              b:'不告诉你'
          }
      }
    });
    console.log(job);
    //定义数组
    let foodArr = reactive(["刷抖音", "敲代码"]);
    console.log(foodArr);
    // 定义方法
    function changeFn() {
      job.type = "后端开发工程师";
      job.salary = "30k";
      job.content = "写代码"; // 给对象添加属性
      foodArr[0]='买包包' // 通过下标修改数组
    }
    return {
      //setup函数返回值为一个对象
      job,
      changeFn,
      foodArr
    };
  },
};
</script>
```



组件传参:

```js
<!--父组件  -->
<template>
  <p></p>
  <Testvue3 :tit="schoolName">
    <span>123</span>
  </Testvue3>
</template>

<script>
import Testvue3 from "@/components/Testvue3";
export default {
  name: "App",
  components: { Testvue3 },
  setup() {
    let schoolName = "千锋"; // 定义要传给子组件的数据  
    return {
      schoolName, // 要使用的变量抛出去，这样就可以在页面模板中使用该变量
    };
  },
};
</script>
```

```js
<!-- 子组件 -->
<template>
  <p>{{ tit }}</p>
  <button>点击事件,子传父</button>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: ["tit"],
  setup(props) { 
    // 参数props即为父组件传过来的参数
     console.log(props)
    return {
      //setup函数返回值为一个对象
    };
  },
};
</script>
```

子传父:

```
<!-- 子组件 -->
<template>
  <p>{{ tit }}</p>
  <button @click="emit">点击事件,子传父</button>
</template>
<script>
import { reactive } from "vue";
export default {
  data() {
    return {};
  },
  emits: ["transfer"], //在子组件中使用emits配置项,接收父组件给我绑定的自定义事件，用法类似于props,						// 不加该配置项，控制台会提示警告
  setup(props, context) {
    console.log(11, props);
    console.log(22, context);
    // 定义方法
    function emit() {
      // 子传父 此处不用this,使用context上下文对象
      context.emit("transfer", 666);
    }
    return {
      //setup函数返回值为一个对象
      emit,
    };
  },
};
</script>
```







## 1、toRef  与 toRefs

定义：toRef  创建一个ref 响应数据

语法：let name = toRef(person，'name')  将响应对象person中的name属性单独拿出来变成响应属性。

应用：一般用于将响应对象中的某个属性单独提供给外部使用

- **如下是使用toRef 前的代码**： 插值表达式模板中的 person 有点繁琐 

```vue
<!-- 子组件 -->
<template>
  <div>
    <p>
      {{ person.name }} -- {{ person.age }} -- {{ person.job.type }} --
      {{ person.job.salary }}
    </p>
  </div>
</template>

<script setup>
import { reactive } from "vue";

    let person = reactive({
      name: "张三",
      age: 20,
      job: {
        type: "web前端开发",
        salary: 30,
      },
    });

</script>
```

- 如下是使用toRef 后 的代码，

```vue
<!-- 子组件 -->
<template>
  <div>
    <p>
     <!-- 在模板中直接使用name,age,type,salary -->
      {{ name }} -- {{ age }} -- {{ type }} --
      {{ salary }}
    </p>
    <p>
      <button @click="name += '-'">修改name</button>
      <button @click="salary++">修改薪水</button>
    </p>
  </div>
</template>

<script>
import { reactive, toRef } from "vue";
    let person = reactive({
      name: "张三",
      age: 20,
      job: {
        type: "web前端开发",
        salary: 30,
      },
    });
      
	// 将person 中的name 属性转换成ref 响应式数据，这样就可以直接在模板中使用了,以此类推
    let name = toRef(person, "name"); 
    let age = toRef(person, "age");
    let type = toRef(person.job, "type");
    let salary = toRef(person.job, "salary");

};
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
```

- 使用toRefs  可以将对象中的多个属性转换成响应数据，代码如下：

```vue
<!-- 子组件 -->
<template>
  <div>
    <p>
      {{ name }} -- {{ age }} -- {{ job.type }} --
      {{ job.salary }}
    </p>
    <p>
      <button @click="name += '-'">修改name</button>
      <button @click="job.salary++">修改薪水</button>
    </p>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";
    let person = reactive({
      name: "张三",
      age: 20,
      job: {
        type: "web前端开发",
        salary: 30,
      },
    });
    //toRefs返回一个响应对象，该对象中每个属性都变成了响应属性了。这样就可以直接拿来在模板插值表达式中使用了
    let person1 = toRefs(person);''
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
```



## 2、setup 语法糖

通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [`setup`](https://cn.vuejs.org/api/sfc-script-setup.html) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`` 中的导入和顶层变量/函数都能够在模板中直接使用。 

**注意: 组合式api 中 没有this .** 

```vue
<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>

<script setup>
import { ref, onMounted,reactive } from 'vue';

// 响应式状态
const count = ref(0); // 返回的是一个响应式的对象

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

```

 组合式 API 的核心思想是直接在函数作用域内定义响应式状态变量，并将从多个函数中得到的状态组合起来处理复杂问题。这种形式更加自由，也需要你对 Vue 的响应式系统有更深的理解才能高效使用。相应的，它的灵活性也使得组织和重用逻辑的模式变得更加强大。 



## 3. setup 语法糖的优点

01: 更好的逻辑复用

02: 更灵活的代码组织

03: 更好的类型推导 对ts 支持更加友好

04: 更小的生产包体积



## 4. setup 响应式数据

**示例代码1 :--reactive**

```vue
<template>
  <ul>
    <li v-for="item in userinfoArr">
      <span v-for="(value, key) in item">{{ key }}--{{ value }}</span> <br />
    </li>
  </ul>
</template>

<script setup>

import { reactive, ref } from 'vue';
const userobj1 = reactive({
  id: 1,
  name: "张三",
  age: 20
})
const userobj2 = reactive({
  id: 2,
  name: "李四",
  age: 30
})

const userinfoArr = reactive([userobj1, userobj2])

</script>
```



### DOM 更新时机

```js
<template>
  // 当 ref 在模板中作为顶层属性被访问时，它们会被自动“解包”，所以不需要使用 .value
  <p id="count" @click="increment">{{ count }}</p>

</template>

<script setup>

import { reactive, ref, nextTick } from 'vue';
let count = ref(0)


function increment() {

  count.value++
  console.log(document.getElementById('count').innerHTML);
  nextTick(() => {
    //   // 访问更新后的 DOM
    console.log(document.getElementById('count').innerHTML);
  })
}
</script>

```



**示例代码2--ref**

```vue
<template>
  // 使用时自动解包
  <p id="count" @click="increment">{{ count }}</p>
  // 使用时自定解包
  <p>
    {{ userobj.age }}
  </p>
</template>

<script setup>

import { ref } from 'vue';
 // 使用ref 处理基本数据类型响应式
let count = ref(0)

// 使用ref 处理复杂数据类型响应式
let userobj = ref({
  name: "李四",
  age: 20
})

console.log(userobj);
function increment() {
  count.value++
  userobj.value.age = 30

}


</script>
```



## 5. 计算 属性 computed

 `computed()` 方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**。和其他一般的 ref 类似 

计算属性的getter 函数式写法:

```js
<template>
  <p id="count" @click="increment">{{ count }} </p>
  <p>计算属性: {{ doubleCount }} </p>
</template>

<script setup>

import { ref, computed } from 'vue';
let count = ref(0)
function increment() {

  count.value++
  userobj.value.age = 30

}

const doubleCount = computed(() => {
  return count.value * 2
})

</script>
```

计算属性的setter 写法:

```vue
<template>
  <p>
    <input type="text" v-model="firstName">
  </p>
  <p>
    <input type="text" v-model="lastName">
  </p>
  <p>
    <input type="text" v-model="fullName">
  </p>
</template>

<script setup>
import { ref, computed } from 'vue'

const firstName = ref('')
const lastName = ref('')

const fullName = computed({
  // getter
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // setter
  set(newValue) {
    // 注意：我们这里使用的是解构赋值语法
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})

</script>
```



## 6. 组件的生命周期钩子函数

注意: 组件的生命周期钩子函数 在setup语法中可以被使用多次,在 选项式api 中只能使用一次,因为会覆盖



```js
<template>
  <div ref="el"></div>
</template>
<script setup>
import { ref, onMounted } from 'vue'

const el = ref()

onMounted(() => {
  console.log('第一次执行')
})
onMounted(() => {
  console.log('第二次执行');
})


</script>
```



## 7. watch 监听器  

```js
<template>
  <p>{{ count }}</p>
  <p><button @click="addcount">addcount</button></p>
</template>

<script setup>
import { ref, watch } from 'vue'

const count = ref(0)

function addcount() {
  count.value++
}

// 可以直接侦听一个 ref
watch(count, (newvalue, oldvalue) => {
  console.log(newvalue, oldvalue);
})
</script>
```



`watch` 的第一个参数可以是不同形式的“数据源”：它可以是一个 ref (包括计算属性)、一个响应式对象、一个 getter 函数、或多个数据源组成的数组： 

```vue
<template>
  <p>x:{{ x }}</p>
  <p>y:{{ y }}</p>
  <p><button @click="x++">x++</button><button @click="y++">y++</button></p>
</template>

<script setup>
import { ref, watch } from 'vue';
const x = ref(0)
const y = ref(0)


// getter 函数 相当于计算属性,根据已有的值衍生出新值
watch(
  () => x.value + y.value,
  (sum, oldsum) => {
    console.log(sum, oldsum)
  }
)

// 多个来源组成的数组
watch([x, y], ([newXY, oldXY]) => {
  console.log(newXY, oldXY)
})
</script>
```



#### 深层侦听器:

直接给 `watch()` 传入一个响应式对象，会隐式地创建一个深层侦听器——该回调函数在所有嵌套的属性变更时都会被触发：  **默认就是深层监听**

```vue
<template>
  <p>{{ userobj.username }}--{{ userobj.age }}</p>
  <p><button @click="userobj.age++">userobj.age++</button></p>
</template>

<script setup>
import { ref, watch, reactive } from 'vue';

let userobj = reactive({
  username: "张三",
  age: 20
})

watch(userobj, (newvalue, oldvalue) => {
  console.log(newvalue, oldvalue);
})
</script>
```



#### **强制转成深度监听器**:

```vue
<template>
  <p>{{ userobj.username }}--{{ userobj.age }}</p>
  <p><button @click="userobj.age++">userobj.age++</button></p>
</template>

<script setup>
import { ref, watch, reactive } from 'vue';

let userobj = reactive({
  username: "张三",
  age: 20
})

watch(() => userobj, (newvalue, oldvalue) => {
  console.log(newvalue, oldvalue);
}, { deep: true })
</script>

```

#### watchEffect()

 `watch()` 是懒执行的：仅当数据源变化时，才会执行回调。但在某些场景中，我们希望在创建侦听器时，立即执行一遍回调 .

watchEffect 不需要去设置要将监听的数据, 因为写在该watchEffect 中的数据就是要监听的数据,

**注意 :** watchEffect 参数函数中依赖什么属性, 那么当该属性变化的时候, watchEffect 函数会被自动触发

 追踪任何在回调中访问到的东西 

```vue
<template>
  <p>{{ userobj.username }}--{{ userobj.age }}</p>
  <p><button @click="userobj.age++">userobj.age++</button></p>
</template>

<script setup>
import { reactive, watchEffect } from 'vue';

let userobj = reactive({
  username: "张三",
  age: 20
})

// watchEffect 监听该方法中箭头函数的中的变量,当变量修改,箭头函数会触发执行.
watchEffect(() => {
  console.log(userobj);
  console.log(userobj.age)
})

</script>
```



#### 监听数据变化后,获取更新后的dom

默认情况下，用户创建的侦听器回调，都会在 Vue 组件更新**之前**被调用。这意味着你在侦听器回调中访问的 DOM 将是被 Vue 更新之前的状态。

如果想在侦听器回调中能访问被 Vue 更新**之后**的DOM，你需要指明 `flush: 'post'` 选项：

```js
watch(source, callback, {
  flush: 'post'
})

watchEffect(()=>{
   //获取最新的dom(数据更新后的dom) 
    
}, {
  flush: 'post'
})
```



## 8. 组件的注册使用

全局组件的注册使用和之前的一样,没有变化

在main.js 中引入引入要全局注册的组件,使用app.component(组件名,组件的实现) 注册组件

```js
// 在main.js中实现
import MyComponent from './App.vue'

app.component('MyComponent', MyComponent)

//也可以链式使用
app
  .component('ComponentA', ComponentA)
  .component('ComponentB', ComponentB)
  .component('ComponentC', ComponentC)
```



局部组件的注册使用:

 在使用 `` 的单文件组件中，导入的组件可以直接在模板中使用，无需注册 

```js
<template>
  <ComponentA />
</template>

<script setup>
import ComponentA from './ComponentA.vue'
</script>


```



全局组件和局部组件相比,优缺点:

```
全局注册虽然很方便，但有以下几个问题：

全局注册，但并没有被使用的组件无法在生产打包时被自动移除 (也叫“tree-shaking”)。如果你全局注册了一个组件，即使它并没有被实际使用，它仍然会出现在打包后的 JS 文件中。

全局注册在大型项目中使项目的依赖关系变得不那么明确。在父组件中使用子组件时，不太容易定位子组件的实现。和使用过多的全局变量一样，这可能会影响应用长期的可维护性
```



#### 父传子:

```vue
// 在子组件中
<template>
    <p>
        我是子组件 --{{ tit }}
    </p>
</template>

<script setup>
// 使用 defineProps 获取参数对象
import { defineProps } from 'vue';
    
// 方式1: 使用字符串数组的形式接受属性
const props = defineProps(['tit']);

// 方式2: 使用对应的形式接受属性
defineProps({
  title: String, // 属性1 及其数据类型
  likes: Number  // 属性2 及其数据类型
})


</script>
<style scoped>

</style>
```



#### 子传父:

方式1: 
子组件通过 $emit 触发自定义事件, 父组件接收自定义事件

**子组件:**

```vue
// 子组件
<template>
    <div>
        <p>
            我是子组件 --{{ tit }}
        </p>
        <p><button @click="$emit('transfer', id)">字传父</button></p>
    </div>
</template>

<script setup>
import { defineProps, ref } from 'vue'
const props = defineProps(['tit']);
    
let id = ref(100)

</script>
<style scoped>

</style>
```

 **父组件:**

```vue
// 父组件
<template>
  <Child tit="100" @transfer="getvalue"></Child>
</template>

<script setup>
import Child from '../components/Child.vue';

const getvalue = (n) => {
  console.log(n);
}

</script>
```



方式2: 使用  defineEmits ()

**子组件:** 

```vue
<template>
    <div>
        <p>
            我是子组件 --{{ tit }}
        </p>
        <p><button @click="emitFn">字传父</button></p>
    </div>

</template>
<script setup>
import { defineProps, defineEmits, ref } from 'vue';
    
const props = defineProps(['tit'])

const emit = defineEmits(['transfer'])

const emitFn = () => {
    emit('transfer', '我是子组件中的数据')
}

let id = ref(100)

</script>
<style scoped>

</style>
```

**父组件: **

```vue
<template>

  <Child tit="100" @transfer="getvalue"></Child>


</template>
<script setup>
import Child from '../components/Child.vue';

const getvalue = (n) => {
  console.log(n);
}


</script>

```



## 9. 依赖注入

`provide` 和 `inject` 可以帮助我们解决这一问题。 [[1\]](https://cn.vuejs.org/guide/components/provide-inject.html#footnote-1) 一个父组件相对于其所有的后代组件，会作为**依赖提供者**。任何后代的组件树，无论层级有多深，都可以**注入**由父组件提供给整条链路的依赖。 



`provide()` 函数接收两个参数。第一个参数被称为**注入名**，可以是一个字符串或是一个 `Symbol`。后代组件会用注入名来查找期望注入的值。一个组件可以多次调用 `provide()`，使用不同的注入名，注入不同的依赖值。

第二个参数是提供的值，值可以是任意类型，包括响应式的状态，比如一个 ref：

**父组件或祖先组件:**

```js
import { ref, provide } from 'vue'

const count = ref(0) 
// provide(key, value) key 为注入的依赖名, value 为依赖名的值
provide('key', count)
```



**应用层的provide**

 除了在一个组件中提供依赖，我们还可以在整个应用层面提供依赖：  在应用级别提供的数据在该应用内的所有组件中都可以注入。这在你编写[插件](https://cn.vuejs.org/guide/reusability/plugins.html)时会特别有用，因为插件一般都不会使用组件形式来提供值。 

```js
import { createApp } from 'vue'

const app = createApp({})

app.provide(key,value)
```



子组件或后代组件 中注入: Inject (注入); 

 如果提供的值是一个 ref，注入进来的会是该 ref 对象，而**不会**自动解包为其内部的值。这使得注入方组件能够通过 ref 对象保持了和供给方的响应性链接。

 要注入上层组件提供的数据，需使用 [`inject()`](https://cn.vuejs.org/api/composition-api-dependency-injection.html#inject) 函数： 

```js
<script setup>
import { inject } from 'vue'

const message = inject('key')

</script>
```

 

## 10. 自定义指令

**局部自定义指令:**

```vue
<script setup>
// 在模板中启用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <input v-focus />
</template>
```



**全局自定义指令:**

```js
const app = createApp({})

// 使 v-focus 在所有组件中都可用
app.directive('focus', {
  /* ... */
})
```



## 11. vue中的自定义hooks(组合式函数)

自定义hooks 就是将页面之间的相同的公共逻辑功能代码抽离处理,放在一个单独js 文件中,以函数的形式导出公共的逻辑代码, 当页面组件需要改部分公共逻辑代码, 秩序导入该函数,然后在该页面中调用使用即可

案例: 实现一个移动鼠标显示当前坐标的demo

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const x = ref(0)
const y = ref(0)

function update(event) {
  x.value = event.pageX
  y.value = event.pageY
}

onMounted(() => window.addEventListener('mousemove', update))
onUnmounted(() => window.removeEventListener('mousemove', update))
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```



如果页面中多个地方使用该功能, 那么可以将该部分功能抽离处理, 在src/ hooks/ usemouse.js 中

```js
import { ref, onMounted, onUnmounted } from 'vue'
export function usemouse() {


    const x = ref(0)
    const y = ref(0)

    function update(event) {
        x.value = event.pageX
        y.value = event.pageY
    }

    onMounted(() => window.addEventListener('mousemove', update))
    onUnmounted(() => window.removeEventListener('mousemove', update))

    return { x, y }
}
```

在页面中使用

```vue
<template>
    <div>
        购物车组件
        <p>Mouse position is at: {{ x }}, {{ y }}</p>
    </div>
</template>

<script setup>
// 到如hooks 函数
import { usemouse } from '@/hooks/mouse.js';
// 调用并使用该hooks函数
const { x, y } = usemouse()
</script>
<style scoped>

</style>
```



## 11. 路由router

 要在 `setup` 函数中访问路由，请调用 `useRouter` 或 `useRoute` 函数,我们需要两个函数 userRouter 和useRoute, 因为我们在 `setup` 里面没有访问 `this`，所以我们不能再直接访问 `this.$router` 或 `this.$route`。作为替代，我们使用 `useRouter` 函数

```vue
<script lang='ts' setup>
import { useRouter, useRoute } from 'vue-router';

    const router = useRouter()
    const route = useRoute()

    function pushWithQuery(query) {
      router.push({
        name: 'search',
        query: {
          ...route.query,
        },
      })
    }
}
</script>
```



## 12、pinia 

 Pinia 是 Vue 的专属状态管理库，它允许你跨组件或页面共享状态。 和之前vuex 的功能是一样的, 可以理解为pinia 为 下一代的vuex工具

 **Pinia API 与 Vuex(<=4) 区别:**

-  *mutation* 已被弃用。它们经常被认为是**极其冗余的** 
-  不再有嵌套结构的**模块** ,也就是说没有了modules,也就没有了 子模块中的命名空间

```shell
yarn add pinia
# 或者使用 npm
npm install pinia
```

第一步: 创建一个 store 对象,并将其挂在到根实例上

```js
// 引入createpinia 创建pinia
import { createPinia } from 'pinia';
const Store = createPinia()
export default Store

//在 mian.js 文件中
// 引入创建好的pinia对象
import Store from '@/store/index'
app.use(Store)
```



第二步: 新建store 文件, 然后在store 文件中新建 useStore.js

```js
// 引入defineStore,使用该函数来定义store
import { defineStore } from 'pinia';
import { reactive, ref, computed } from 'vue';
// 你可以对 `defineStore()` 的返回值进行任意命名，但最好使用 store 的名字，同时以 `use` 开头且以 `Store` 结尾。(比如 `useUserStore`，`useCartStore`，`useProductStore`)
// 第一个参数是你的应用中 Store 的唯一 ID。
// 如下使用的是setup 组合式api
export const useUserStore = defineStore('user', () => {
    const userinfo = reactive({ name: "张三", age: 20 })
    const count = ref(0)
    const addage = () => { // 修改store中的userinfo 数据的action 方法
        userinfo.age++
    }
    const doubleCount = computed(() => { // 相当于getters 计算属性
        return count.value * 2
    })
    // 将属性和方法暴露出去,这样 这样在页面中才能访问到该数据和方法
    return { userinfo, count, addage, doubleCount } 
})

```

在 *Setup Store* 中：

- `ref()` 就是 `state` 属性
- `computed()` 就是 `getters`
- `function()` 就是 `actions`



第三步:在页面中使用

```vue
<template>
    <div>
        我的页面组件
        <p>{{ userinfo.name }}--{{ userinfo.age }}</p>
        <p @click="count++">count: {{ count }}</p>
        <p>计算属性doubleCount: {{ doubleCount }}</p>
        <p><button @click="addage">用户年龄++</button></p>
    </div>
</template>

<script setup>
//  引入userStore 中对应的数据
import { useUserStore } from '@/store/userStore.js'
// 注意: 解构时, 从useStore.js 中的数据会丢掉响应式, 
// 如果直接从pinia中解构数据，会丢失响应式， 使用storeToRefs可以保证解构出来的数据也是响应式的
import { storeToRefs } from 'pinia';
const useStore = useUserStore()

const { userinfo, doubleCount, count } = storeToRefs(useUserStore())
// 非数据的数据如下边的函数,不是响应式的数据,响应式数据针对值,而不会函数表达式,所以如下函数不需要使用storeToRefs处理
const { addage } = useUserStore()
// console.log(userinfo, doubleCount, count);
</script>
<style scoped>
</style>
```



**pinia 数据持久化:**

安装  pinia-persistedstate-plugin

```shell
npm install pinia-persistedstate-plugin
```

在 store目录下 新建 index.js 文件, 配置store 的数据的持久化

```js
import { createPinia } from "pinia";

import { createPersistedState } from "pinia-persistedstate-plugin";
const store = createPinia();
store.use(createPersistedState());

export default store
```

在main.js文件中引入使用

```js
import store from '@/stores'

app.use(store)
```



## 13、新的组件

### 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

### 2.Teleport

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。

  ```vue
  <teleport to="移动位置">
  	<div v-if="isShow" class="mask">
  		<div class="dialog">
  			<h3>我是一个弹窗</h3>
  			<button @click="isShow = false">关闭弹窗</button>
  		</div>
  	</div>
  </teleport>
  ```

### 3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```js
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用```Suspense```包裹组件，并配置好```default``` 与 ```fallback```

    ```vue
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```





## 14.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理



## 15.  vue3 中使用 ts 

官方文档: https://cn.vuejs.org/guide/typescript/composition-api.html; 



01: 首先需要使用脚手架安装一个vue+ts 的项目结构

02: 实现父传子的属性props 接收,对props 的接收的参数的类型进行校验

#### 1- 组件的 props 标注类型

```vue
// 父组件:

<template>
  <child :title="title" :count="count"></child>
  <button @click="add">addcount</button>
</template>

<script setup lang="ts">
import child from '@/components/Child.vue';

import { ref } from 'vue';
const title = ref('父组件的数据');
let count = ref(0);

const add = () => {
  count.value++
  console.log(count);

}

</script>
```



```vue
// 子组件
<template>
    <div>
        child
        <p>{{ title }}--{{ count }}</p>
    </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';
// 如下是以前 js 的组合式的写法
//const props = defineProps(['title', 'count'])
//const { count, title } = props

// 如下是ts 的写法1
const props = defineProps<{
   	title: string
    count?: number
}>();

// 如下是ts 的写法2, 定义一个接口类型
interface props {
    title: string
    count?: number
}
const props = defineProps<props>();
console.log(props)
    
// 写法3 报错   
// 将使用interface 定义的接口写在其他文件中然后引入使用
// 注意: 接口或对象字面类型可以包含从其他文件导入的类型引用，但是，传递给 defineProps 的泛型参数本身不能是一个导入的类型：
// import type { props } from '@/types/interface';
// const props = defineProps<props>();
// console.log(props.title, props.count)   
    
    
//写法4:  设置props 参数的默认值
interface props {
    title: string
    count?: number
}

const props = withDefaults(defineProps<props>(), {
    title: 'hello', // 默认值为 hello
    count: () => 10  // 10 为count 的默认值
})

</script>
<style scoped>

</style>
```



#### 2-为组件的 emits 标注类型

emits 定义在父传子组件传参过程的方法的

```vue
// 子组件:
<template>
    <div>
        child
        <p>{{ title }}--{{ count }}</p>
        <button @click="myFn">子组件的点击事件</button>
    </div>
</template>

<script setup lang="ts">
import {defineEmits} from 'vue';
    
const emit = defineEmits<{
  //定义在子组件中定义父组件传递给子组件的自定事件方法, 如下形事件参1: 表示自定义事件, 需要传递的参数可选
    (e: 'transfer', id: number): void 
}>();
    
// 定义点击事件,点击后执行emit 方法,并且写事件的啊
const myFn = () => {
    emit('transfer', 100)
}

</script>
<style scoped>

</style>
```

```vue
//父组件中:
<template>

  <child :title="title" :count="count" @transfer="getvalue"></child>
  <button @click="add">addcount</button>
</template>

<script setup lang="ts">
import child from '@/components/Child.vue';

import { ref } from 'vue';
const title = ref('父组件的数据');
let count = ref(0);

const add = () => {
  count.value++
  console.log(count);

}

// 自定义事件对应的执行函数
const getvalue = (m: number) => {
  console.log(11, m);

}

</script>
```



#### 3- 为 `ref()` 标注类型

使用方式1   ref 会根据初始化时的值推导其类型： 

```ts
import { ref } from 'vue'

// 推导出的类型：Ref<number>
const year = ref(2020)

// => TS Error: Type 'string' is not assignable to type 'number'.
year.value = '2020'
```



使用方式2:   在调用 `ref()` 时传入一个泛型参数，来覆盖默认的推导行为： 

```vue
// 得到的类型：Ref<string | number>
const year = ref<string | number>('2020')

year.value = 2020 // 成功！
```



#### 4- reactive 标注类型

```vue
<template>
    <div>
        <p>{{ ((book.obj) as itemType).name }}</p>
    </div>
</template>

<script lang='ts' setup>
import { reactive } from 'vue';
// 定义obj 的数据类型
interface itemType { name: string, price?: number };
// 定义book 的数据类型
interface Book {
    obj: itemType | null
}
// 给book 初始化赋值
let book = reactive<Book>({ obj: null });
// 修改obj 的值
book.obj = { name: '水浒转' }

console.log(book)
//  修改obj 的值
book.obj.name = '三国'
</script>
```



#### 5 - 计算属性中设置类型

```
import { ref, computed } from 'vue';

const count = ref(0);

// 推导得到的类型：ComputedRef<number>
const double = computed(() => count.value * 2);

// => TS Error: Property 'split' does not exist on type 'number'
const result = double.value.split('')
```

```vue
const double = computed<number>(() => {
  // 若返回值不是 number 类型则会报错
})
```



#### 6- 为事件处理函数标注类型

```vue
<template>
  <input type="text" @change="handleChange" />
</template>
<script setup lang="ts">


// `event` 如果不设置类型的话,隐式地标注为 `any` 类型,vscode里将event标记为 Event类型
// 当然 Event 还可以具体的分为 mouseEvent 等等..
function handleChange(event: Event) {
   // 强制将event.target 转为 HTMLInputElement
  console.log((event.target as HTMLInputElement).value)
}
</script>
```



#### 7.为 provide / inject 标注类型

在祖先组件或父组件中提供provide 一个变量

```vue
<template>
  <div class="about">
    我是about 页面
    <Son></Son>
  </div>
</template>
<script lang="ts" setup>
import Son from '@/components/Son.vue';
import { provide } from 'vue';

import { key } from '@/types/interface';    
provide(key, '我是爷爷'); // 若提供的是非字符串值会导致错误

</script>
<style>

</style>
```

在src 下的 目录中创建types 文件夹,然后定义需要导出的数据类型

```ts
import type { InjectionKey } from 'vue';
export const key = Symbol() as InjectionKey<string>;
```

在后代的组件中注入祖先组件提供的变量

```vue
<template>
    <div>
        孙子组件
    </div>
</template>

<script setup lang="ts">
import { inject } from 'vue';
import { key } from '@/types/interface'
    
// foo 就是获取到的依赖的值
const foo = inject<string>(key) 
// 也可以设置默认值  假设祖先组件中没有提供依赖,则默认为 'grandfather'
const foo = inject<string>(key, 'grandfather') as string

</script>
<style scoped>
    
</style>
```



#### 8 . 为模板引用标注类型

给元素设置ref 属性

```vue
<template>
  <input ref="el" />
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
// 直到组件被挂载前，这个 ref 的值都是初始的 null
// el和ref的值必须保持一致
const el = ref<HTMLInputElement | null>(null)

onMounted(() => {
  // 使用可选链 es6 以后新增的语法
  el.value?.focus()
})
</script>


```



#### 9.  为组件模板引用标注类型

 有时，你可能需要为一个子组件添加一个模板引用，以便调用它公开的方法。 

给子组件设置ref 属性 , 则获取的就是子组件的实例对象

```vue
// 父组件
<template>
    <div>
        分类
    </div>
    <hr />
    <categorychildVue ref="model"></categorychildVue>
    <button @click="showmodel">点击显示模态框</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

 //引入子组件
import categorychildVue from '@/components/categorychild.vue';
// 获取子组件的实例
const model = ref<InstanceType<typeof categorychildVue> | null>(null)
// 点击父组件中定义的方法
const showmodel = () => {
    // 调用子组件中定义的方法实例上的方法
    console.log(model.value?.open());
}
</script>
<style scoped>

</style>
```



```vue
// 子组件
<template>
    <div v-show="isShow">
        模态框
    </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
const isShow = ref(false)
const open = () => (isShow.value = true);

// 将该子组件中的方法抛出,这样父组件中才能获取该方法
defineExpose({
    open
})

</script>
<style scoped>

</style>
```

