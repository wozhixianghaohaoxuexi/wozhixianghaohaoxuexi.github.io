---
title: vue3新特性
date: 2022-03-03 11:53:47
categories:
  - vue
tags:
  - vue3
---

### 一、常用 Composition API

#### 1. setup

setup 是 Vue3.0 中一个新的配置项，值为一个函数，是所有 Composition API 的入口

**两个注意点**：

* 1.setup 执行的时机：在 beforeCreate 之前执行一次，this 是 undefined
* 2.setup 的参数：
    * props：值为对象，包含组件外部传递过来，且组件内部声明接收了的属性
    * content：上下文对象
        * attrs：值为对象，包含组件外部传递过来，但没有在 props 配置中声明的属性，相当于 `this.$attrs`
        * slots：收到的插槽内容，相当于 `this.$slots`
        * emit：分发自定义事件的函数，相当于 `this.$emit`

#### 2. ref 和 reactive

##### 2.1 ref 和 reactive 使用

* ref 用于定义一个响应式的数据
    * 定义基本数据类型：响应式是靠 Object.defineProperty() 的 get 和 set 完成的
    * 定义对象数据类型：内部实际使用 reactive 函数
* reactive 用于定义一个**对象类型**的响应式数据

```html
<template>
    <div>num: {{ num }}</div>
    <div>obj.name: {{ obj.name }}</div>
    <div>obj.age: {{ obj.age }}</div>
</template>
```

```js
import { ref, reactive } from 'vue'
export default {
  setup() {
    const num = ref(1)
    const obj = reactive({
      name: '张三',
      age: 18
    })
    setTimeout(() => {
      num.value ++;
      obj.age ++;
    }, 3000);
    return {
      num,
      obj
    }
  }
}
```

##### 2.2 ref 与 reactive 对比

* 从定义数据角度对比：
    * ref用来定义：基本类型数据。
    * reactive用来定义：对象（或数组）类型数据。
    * 备注：ref也可以用来定义对象（或数组）类型数据, 它内部会自动通过reactive转为代理对象。
* 从原理角度对比：
    * ref通过Object.defineProperty()的get与set来实现响应式（数据劫持）。
    * reactive通过使用Proxy来实现响应式（数据劫持）, 并通过Reflect操作源对象内部的数据。
* 从使用角度对比：
    * ref定义的数据：操作数据需要.value，读取数据时模板中直接读取不需要.value。
    * reactive定义的数据：操作数据与读取数据：均不需要.value。

##### 2.3 vue2 和 vue3 响应式对比

* vue2实现原理：
    * 对象类型：通过 Object.defineProperty() 对属性的读取、修改进行拦截（数据劫持）
    * 数组类型：通过重写更新数组的一系列方法来实现拦截（对数组的变更方法进行了包裹）
    * 为什么数组类型不通过 Object.defineProperty() 对数组属性进行监听？[（Object.defineProperty 可以对数组元素监听，但是性能代价和获得的用户体验收益不成正比）](https://github.com/vuejs/vue/issues/8562)

```js
Object.defineProperty(data, 'count', {
    get () {}, 
    set () {}
})
```

* vue3实现原理：
    * 通过 Proxy（代理）：拦截对象中任意属性的变化，包括属性值的读写、属性的添加、属性的删除等
    * 通过 Reflect（反射）：对源对象的属性进行操作

```js
const proxy = new Proxy(data, {
	// 拦截读取属性值
    get (target, prop) {
    	return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
    	return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
    	return Reflect.deleteProperty(target, prop)
    }
})
proxy.name = 'tom'
```

#### 3. 计算属性与监视

##### 3.1 computed

与 vue2 中 computed 配置功能一致

```js
import { computed, reactive } from 'vue'
export default {
  setup() {
    const person = reactive{
      firstName: '张',
      lastName: '三'
    }
    // const fullName = computed(() => {
    //   return `${person.firstName}-${person.lastName}`
    // })
    const fullName = computed({
      get() {
        return `${person.firstName}-${person.lastName}`
      },
      set(value) {
        const nameArr = value.split('-')
        person.firstName = nameArr[0]
        person.lastName = nameArr[1]
      }
    })
    return {
      person,
      fullName
    }
  }
}
```

##### 3.2 watch

与 vue2 中 watch 配置功能一致

**注意点：**
1. 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
2. 监视reactive定义的响应式数据中某个属性时：deep配置有效。

```js
//情况一：监视ref定义的响应式数据
watch(sum, (newValue, oldValue)=>{
	console.log('sum变化了', newValue, oldValue)
}, {immediate: true})

//情况二：监视多个ref定义的响应式数据
watch([sum, msg], (newValue, oldValue)=>{
	console.log('sum或msg变化了', newValue, oldValue)
}) 

/* 情况三：监视reactive定义的响应式数据
			若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
			若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
*/
watch(person, (newValue, oldValue)=>{
	console.log('person变化了', newValue, oldValue)
}, {immediate: true, deep: false}) //此处的deep配置不再奏效

//情况四：监视reactive定义的响应式数据中的某个属性
watch(()=>person.job, (newValue, oldValue)=>{
	console.log('person的job变化了', newValue, oldValue)
}, {immediate: true, deep: true}) 

//情况五：监视reactive定义的响应式数据中的某些属性
watch([()=>person.job, ()=>person.name], (newValue, oldValue)=>{
	console.log('person的job变化了', newValue, oldValue)
}, {immediate: true, deep: true})

//特殊情况
watch(()=>person.job, (newValue, oldValue)=>{
    console.log('person的job变化了', newValue, oldValue)
}, {deep: true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
```

##### 3.3 watchEffect

* watchEffect 与 watch 区别：watch 既要指明监视的属性，也要指明监视的回调。而 watchEffect 不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

```js
// watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
watchEffect(()=>{
    const x1 = sum.value
    const x2 = person.age
    console.log('watchEffect配置的回调执行了')
})
```

#### 4. 生命周期
![vue3生命周期](https://gitee.com/huqian025/my-images/raw/master/vue/vue3新特性/vue3生命周期.png)

* vue3 中可以继续使用 vue2 中的生命周期钩子，但是有两个被更名
    * beforeDestroy 更名为 beforeUnmount
    * destroyed 更名为 unmounted

* vue3 也提供了 Composition API 形式的生命周期钩子，与 vue2 中钩子对应关系如下：
    * beforeCreate ===> setup
    * created =======> setup
    * beforeMount ===> onBeforeMount
    * mounted ======> onMounted
    * beforeUpdate ===> onBeforeUpdate
    * updated =======> onUpdated
    * beforeDestroy ==> onBeforeUnmount
    * destroyed =====> onUnmounted
    * onRenderTracked（新增，调试用）
    * onRenderTriggered（新增，调试用）

![vue2和vue3生命周期钩子对比](https://gitee.com/huqian025/my-images/raw/master/vue/vue3新特性/vue2和vue3生命周期钩子对比.png)


#### 5. 自定义 hook 函数

* hook 本质是使用 vue3 的组合 API 封装的可复用的功能函数

* 示例1：收集用户鼠标点击的页面坐标

```ts
import { ref, onMounted, onUnmounted } from 'vue'

export default function useMousePosition () {
  // 初始化坐标数据
  const x = ref(-1)
  const y = ref(-1)

  // 用于收集点击事件坐标的函数
  const updatePosition = (e: MouseEvent) => {
    x.value = e.pageX
    y.value = e.pageY
  }

  // 挂载后绑定点击监听
  onMounted(() => {
    document.addEventListener('click', updatePosition)
  })

  // 卸载前解绑点击监听
  onUnmounted(() => {
    document.removeEventListener('click', updatePosition)
  })

  return {x, y}
}
```

* 示例2：使用axios发送异步ajax请求

```ts
import { ref } from 'vue'import axios from 'axios'

export default function useUrlLoader<T>(url: string) {

  const result = ref<T | null>(null)
  const loading = ref(true)
  const errorMsg = ref(null)

  axios.get(url)
    .then(response => {
      loading.value = false
      result.value = response.data
    })
    .catch(e => {
      loading.value = false
      errorMsg.value = e.message || '未知错误'
    })

  return {
    loading,
    result,
    errorMsg,
  }
}
```

#### 6. toRef 和 toRefs

* toRef 作用：为 reactive 对象上的属性创建 ref。创建的 ref 与源属性同步，修改源属性将更新 ref，修改 ref 也将更新源属性

```js
const state = reactive({
  foo: 1,
  bar: 2
})
const fooRef = toRef(state, 'foo')

// 改变 ref 值，源属性值发生变化
fooRef.value++
console.log(state.foo) // 2

// 修改源属性值，ref 值发生变化
state.foo++
console.log(fooRef.value) // 3

// 错误示范，fooRef 不会与 state.foo 同步，因为 ref() 接收的是一个纯字符串值
// const fooRef = ref(state.foo)
```

* toRefs 作用：将 reactive 对象转换为普通对象，普通对象的每个属性都是指向原始对象相应属性的 ref。每个单独的 ref 都是使用 toRef 创建的

```js
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2
  })
 
  // 返回时转化为 refs
  return toRefs(state)
}
// 对象解构不会丢失响应式
const { foo, bar } = useFeatureX()
```

### 二、其他 Composition API
#### 1. shallowReactive 与 shallowRef

* shallowReactive：只处理对象最外层属性的响应式（浅响应式）
* shallowRef：只处理基本数据类型的响应式，不进行对象的响应式处理
* 什么时候使用：
    * 如果一个对象结构比较深，但只是外层属性发生变化，则使用 shallowReactive
    * 如果一个对象，后续不会修改其属性，而是用新的对象来替换，则使用 shallowRef

#### 2. readonly 与 shallowReadonly

* readonly：让一个响应式数据变为只读的（深只读）
* shallowReadonly：让一个响应式数据变为只读的（浅只读）
* 使用场景：不希望数据被修改时

#### 3. toRaw 与 markRaw

* toRaw：将一个由 reactive 生成的响应式对象转为普通对象
* 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新

* markRaw：标记一个对象，使其永远不会成为响应式对象
* 使用场景：
    * 有些值不应被设置为响应式，例如复杂的第三方类库等
    * 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能

#### 4. customRef

* 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制

```js
import { ref, customRef } from 'vue'
export default {
    name:'Demo',
    setup(){
        // let keyword = ref('hello') //使用 Vue 准备好的内置 ref
        // 自定义一个 myRef
        function myRef(value, delay){
            let timer
            // 通过 customRef 去实现自定义
            return customRef((track, trigger)=>{
                return{
                    get(){
                        track() // 告诉 vue 这个 value 值是需要被“追踪”的
                        return value
                    },
                    set(newValue){
                        clearTimeout(timer)
                        timer = setTimeout(()=>{
                            value = newValue
                            trigger() // 告诉 vue 去更新界面
                        },delay)
                    }
                }
            })
        }
        // 使用自定义的ref
        let keyword = myRef('hello', 500) 
        return {
            keyword
        }
    }
}
```

#### 5. provide 与 inject

* 作用：实现祖与后代组件之间的通信。父组件有一个 provide 选项来提供数据，后代组件有一个 inject 选项来使用这些数据

![provide和inject](https://gitee.com/huqian025/my-images/raw/master/vue/vue3新特性/provide和inject.png)

```js
// 祖组件
setup() {
    let car = reactive({name: '奔驰', price: '40万'})
    provide('car', car)
}

// 后代组件
setup() {
    const car = inject('car')
    return { car }
}
```

#### 6. 响应式数据的判断

* isRef：检查一个值是否为 ref 对象
* isReactive：检查一个对象是否是由 reactive 创建的响应式代理
* isReadonly：检查一个对象是否是由 readonly 创建的只读代理
* isProxy：检查一个对象是否是由 reactive 或者 readonly 方法创建的代理

### 三、新组件
#### 1. Fragment（片段）

* 在 vue2 中：组件必须有一个根标签
* 在 vue3 中：组件可以没有根标签，内部会将多个标签包含在一个 Fragment 虚拟元素中
* 好处：减少标签层级，减小内存占用

#### 2. Teleport（瞬移）

* Teleport 是一种能够将组件的 html 结构移动到指定位置的技术

```html
<!-- to 目标需要一个 CSS 选择器字符串或一个实际的 DOM 节点 -->
<teleport to="body">
	<div v-if="isShow" class="mask">
		<div class="dialog">
			<h3>我是一个弹窗</h3>
			<button @click="isShow = false">关闭弹窗</button>
		</div>
	</div>
</teleport>
```

#### 3. Suspense（不确定的）

* 作用：等待异步组件时渲染一些额外内容，让应用有更好的用户体验
* 使用步骤：
1. 异步引入组件

```js
import { defineAsyncComponent } from 'vue'
const Child = defineAsyncComponent(() => import('./components/Child.vue'))
```

2. 使用 Suspense 包裹组件，并配置好 default 和 fallback

```html
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

### 四、其他
#### 1. 全局 API 转移

* vue3 中将全局 API，即 Vue.xxx 调整到应用实例 app 上

| 2.x 全局 API（Vue）      | 3.x 实例 API (app)          |
| ------------------------ | --------------------------- |
| Vue.config.xxxx          | app.config.xxxx             |
| Vue.config.productionTip | 移除                        |
| Vue.component            | app.component               |
| Vue.directive            | app.directive               |
| Vue.mixin                | app.mixin                   |
| Vue.use                  | app.use                     |
| Vue.prototype            | app.config.globalProperties |

#### 2. 自定义指令

vue2 自定义指令钩子：

* bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
* inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
* update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新。
* componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
* unbind：只调用一次，指令与元素解绑时调用。

**vue2和vue3自定义指令钩子对比：**
![vue2和vue3自定义指令钩子对比](https://gitee.com/huqian025/my-images/raw/master/vue/vue3新特性/vue2和vue3自定义指令钩子对比.png)

```js
// vue2 注册一个全局自定义指令 v-focus
Vue.directive('focus', { 
    // 当被绑定的元素插入到 DOM 中时
    inserted: function (el) { 
        // 聚焦元素 
        el.focus() 
    } 
})

// vue3 注册一个全局自定义指令 v-focus
const { createApp } from "vue"
const app = createApp({}) 
app.directive('focus', { 
    mounted(el) { 
        el.focus()
    } 
})
```

#### 3. v-model

* 非兼容变更：用于自定义组件时，v-model 的 prop 和事件默认名称已更改
    * prop: value -> modelValue
    * 事件: input -> update:modelValue
* 非兼容变更：v-bind 的 .sync 修饰符和组件的 model 选项已移除，可在 v-model 上加一个参数代替
* 新增：可以在同一组件上使用多个 v-model 绑定
* 新增：可以自定义 v-model 修饰符

##### 3.1 vue2 语法

vue2 中，在组件上使用 v-model 相当于绑定 value prop 并触发 input 事件

```html
<ChildComponent v-model="pageTitle" />
<!-- 等价于 -->
<ChildComponent :value="pageTitle" @input="pageTitle = $event" />
```

###### 3.1.1 model 选项配置修改双向绑定

如果想要更改 prop 或事件名称，则需要在 ChildComponent 组件中添加 model 选项

```html
<!-- ParentComponent.vue -->
<ChildComponent v-model="pageTitle" />
<!-- 等价于 -->
<ChildComponent :title="pageTitle" @change="pageTitle = $event" />
```

```js
// ChildComponent.vue
export default {
  model: {
    prop: 'title',
    event: 'change'
  },
  props: {
    // 这将允许 `value` 属性用于其他用途
    value: String,
    // 使用 `title` 代替 `value` 作为 model 的 prop
    title: {
      type: String,
      default: 'Default title'
    }
  }
}
```

###### 3.1.2 v-bind.sync 修改双向绑定（vue3 中已移除）

在某些情况下，我们可能需要对某一个 prop 进行“双向绑定”，可以在子组件中抛出 update:myPropName 事件，父组件使用 :myPropName.sync 实现对 myPropName 属性的双向绑定

```js
// ChildComponent.vue
this.$emit('update:title', newValue)
```

```html
<!-- ParentComponent.vue -->
<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
<!-- 等价于 -->
<ChildComponent :title.sync="pageTitle" />
```

##### 3.2 vue3 语法

在 vue3 中，自定义组件上的 v-model 相当于传递了 modelValue prop 并接收抛出的 update:modelValue 事件

```html
<ChildComponent v-model="pageTitle" />
<!-- 等价于 -->
<ChildComponent :modelValue="pageTitle" @update:modelValue="pageTitle = $event"/>
```

如果需要更改 model 名称，可以为 v-model 传递一个参数，作为组件内的 model 选项代替

```html
<ChildComponent v-model:title="pageTitle" />
<!-- 等价于 -->
<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
```

这也可以作为 .sync 修饰符的替代，而且允许我们在自定义组件上使用多个 v-model

```html
<ChildComponent v-model:title="pageTitle" v-model:content="pageContent" />
<!-- 等价于 -->
<ChildComponent 
    :title="pageTitle" @update:title="pageTitle = $event"
    :content="pageContent" @update:content="pageContent = $event" />
```

#### 4. 异步组件

Vue3 中 使用 defineAsyncComponent 定义异步组件，配置选项 component 替换为 loader，loader 函数本身不再接收 resolve 和 reject 参数，且必须返回一个 Promise，用法如下：

```html
<template> 
    <!-- 异步组件的使用 --> 
    <AsyncPage /> 
    <AsyncPageWithOptions />
</tempate>
```

```js
import { defineAsyncComponent } from "vue"; 
export default { 
    components: {
        // 无配置项异步组件 
        AsyncPage: defineAsyncComponent(() => import("./NextPage.vue")),
        
        // 有配置项异步组件 
        AsyncPageWithOptions: defineAsyncComponent({ 
            loader: () => import("./NextPage.vue"), 
            delay: 200, 
            timeout: 3000, 
            errorComponent: () => import("./ErrorComponent.vue"), 
            loadingComponent: () => import("./LoadingComponent.vue")
        }) 
    }
}
```

#### 5. 其他改变

* data 选项应始终被声明为一个函数
* 过渡类名更改：

```css
/* vue2 写法 */
.v-enter,
.v-leave-to {
    opacity: 0;
}
.v-leave,
.v-enter-to {
    opacity: 1;
}

/* vue3 写法 */
.v-enter-from,
.v-leave-to {
    opacity: 0;
}
.v-leave-from,
.v-enter-to {
    opacity: 1;
}
```

* 移除 keyCode 作为 v-on 的修饰符，同时也不再支持 config.keyCodes
* 移除 v-on.native 修饰符
* 移除过滤器（filter）
* v-if 优先 v-for 解析

>参考文档：
>[掘金：Vue3.0 新特性以及使用经验总结](https://juejin.cn/post/6940454764421316644)
>[Vue3 API toRef](https://vuejs.org/api/reactivity-utilities.html#toref)
>[Vue2 迁移 Vue3 文档 v-model](https://v3.cn.vuejs.org/guide/migration/v-model.html)
