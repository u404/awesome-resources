# vue3 基础理解

## 应用入口

```ts
import { createApp } from 'vue'
import App from './App.vue'  // 根组件

const app = createApp(App)  // 创建应用

app.mount('#app')  // 挂载应用
```

## 响应式

```ts
import { reactive, ref } from "vue"   // 配套函数：isRef、unref

// reactive可以用来创建响应式对象 - 原理Proxy -----------------------------
const state = reactive({ name: "aaa", count: 123, child: { name: "abc" } })  
// 更新 - 可以直接更新：state.count++
// 访问 - 可以直接在模板中，访问state.name、state.count
// 深层响应性 - 默认具有深层响应性，使用 shallowReactive 可以创建浅层响应对象
// 失去响应性 - 将对象解构时、将属性赋值给变量时、将属性直接作为参数传入函数时。相应的接收变量，无法获得响应性

// ref可以用来创建响应式变量 ------------------------------------------
const count = ref(0);  // ref会定义为：count = { value: 0 }
// 更新 - js块内必须使用.value的方式更新： count.value++ 
// 访问 - js块内使用count.value的方式访问，而模板代码块内可以自动解包，因此可以直接使用count
// 深层响应性 - 具备深层响应性，即当ref()参数值包装的是一个对象时，会内部调用reactive包装对象
//              即：const o = ref({ name: "aaa" }); 会定义为：o = { value: reactive({ name: "aaa" }) }
// 自动解包 - 在模板代码块内，ref创建的变量或属性，作为顶级属性访问时，自动解包。
//          - 在js块内，若ref被嵌套（即赋值）给一个响应式对象，会自动解包。
//          - 但将ref嵌入数组或Map时，不会自动解包。
```

## 计算属性

```ts
import { computed } from "vue"

const totalCount = computed(() => {
    return state.count + count
})

// 创建可赋值的计算属性
const totalCount = computed({
    get() {
      return state.count + count      
    },
    set(newValue) {
      state.count = 0
      count.value = newValue
    }
})
```

## watch

```ts
import { ref, watch } from "vue" 

const question = ref("")

watch(question, async (v, ov) => {
    console.log(v, ov)
})

// watch 的第一个参数，支持多种数据源：ref、计算属性、响应式对象、getter函数、多个数据源组成的数组
watch(() => x.value + y.value, (sum) => {  // watch 一个getter函数
    console.log(sum) 
})
watch([x, () => y.value], ([newX, newY]) => {   // watch 多个数据源
    console.log(newX, newY)    
})

// 注意，不能直接侦听响应式对象的属性值
watch(obj.count, ...)  // 这是错误的！
// 可以改用getter函数
watch(() => obj.count, ...)  // 正确

// 深层侦听器 - 给watch传入一个响应式对象，会创建深层的侦听器。内部所有嵌套变更时都会触发
watch(obj, (v, ov) => {
    // v 和 ov 是相等的，是同一个对象！
    // 属性也完全相同
})

// 浅层侦听
// 无法直接监听一个顶层响应对象本身的变化，但可以将其赋值给另一个响应对象的方式来实现
watch(() => outter.obj, ...)  // 这种情况下，是浅层侦听的！
watch(() => outter.obj, callback, { deep: true })  // 可以将其强制转化为深层监听

// watchEffect() - 立即执行，并侦听所有内部引用的响应数据的后续变化
watchEffect(async () => {
    const response = await fetch(url.value)  // 注意，仅监听同步执行时的依赖，也就是所有的await之前的
    data.value = await response.json() 
})

// 回调触发时机 - 默认都是在组件更新之前，此时访问DOM将是原状态。
// 指定 flush: 'post' 选项，可以将触发时机指定到组件更新后
watch(source, callback, {
  flush: 'post'
})

watchEffect(callback, {
  flush: 'post'
})
// watchEffect post模式的别名
import { watchPostEffect } from 'vue'
watchPostEffect(() => {
  /* 在 Vue 更新后执行 */
})

// 停止侦听器
// 所有同步执行时创建的侦听器，都会在组件卸载时自动停止。
// 异步创建的侦听器，需要手动停止，否则会内存泄漏。
const unwatch = watchEffect(callback);  // watch 和 watchEffect 都会返回停止函数
// 在必要时卸载
unwatch()
```

## 生命周期钩子

```ts
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
```

## 模板引用ref

```ts
// 为了获得对组件或Dom的引用，可以声明一个ref变量，然后ref属性指定该变量
import { ref, onMounted } from 'vue'

const input = ref(null)   // 声明一个ref变量

onMounted(() => {
    input.value.focus()   // 组件加载后，即可通过ref的value来获取组件或Dom的引用
})

<template>
    <input ref="input" />  <!-- ref属性指定该变量 -->
</template>

// 在 v-for 语句中 ref属性会将所有组件或Dom的引用存入数组，但不能保证数组元素的顺序
const itemRefs = ref([])  // 定义一个数组ref变量来接收

onMounted(() => console.log(itemRefs.value))
</script>

<template>
  <ul>
    <li v-for="item in list" ref="itemRefs">
      {{ item }}
    </li>
  </ul>
</template>

// ref属性可以绑定为一个函数，每次更新时都会调用（卸载时也会调用一次，传入的el为null）
<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }" />
<input :ref="func" /> 

// 组件上的ref
// 使用选项式api或没有使用<script setup>定义的子组件，被父组件引用时，其 ref = 子组件的this
// 但使用了<script setup>时，组件内容是私有的。
// 只有显式使用 defineExpose({ a, b... })暴露的内容，才能被父组件访问到。


```

## script setup 专用api - 宏命令（不需要引入）

```ts
const props = defineProps(['title'])  // 定义props
// js块中使用props.title，而template中直接使用title

const emit = defineEmits(['change'])  // 定义事件
// 使用emit('change')  来触发事件

// 对比非script setup的模式 -------------
export default {
    props: ['title'],
    emits: ['change'],
    setup(props, ctx) {
        ctx.emit('change')    
    }
}

```

## 事件

```ts
// 在自定义组件上支持v-model指令，
// value - props.modelValue，
// input - update:modelValue

// 定义props - modelValue
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) }
})

// 触发value变更事件
defineEmits(['update:modelValue'])
```