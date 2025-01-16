---
title: "vue2,3的学习笔记"
date: 2024-05-27 14:33:00
tags: [ "vue","html" ]
---

## vue的使用步骤

1. 准备容器 (Vue所管理的范围)
2. 引包 (开发版本包 / 生产版本包) 官网
3. 创建实例
4. 添加配置项 =>完成渲染

## 常用函数

### filter 函数

filter是一个操作函数，用于筛选数组中的元素，返回剩下的元素，它会遍历筛选集合中的元素，再根据表达式的值为true或者false来选择是否保留
例如：

```vue
this.taskList = this.taskList.filter(item => item.id !== id)
```

上面代码的作用是筛选除了taskList集合中元素 "item"的id 不为 "id"的所有元素，作用和删除指定元素效果一致

### reduce函数

用于求和的函数，用于数组和集合，会使用回调函数将一个数组或集合中的值循环取出遍历

```vue
let total = this.list.reduce((sum, item) => sum + item.num, 0)
```

上面代码的作用，设置求和的初始值为0.将sum与item中的num相加，每次相加的值赋给sum，最终达到求和的效果

## 易错点

### computed和methods方法

computed侧重于数据的计算与调用，在不同的地方多次使用只会调用一次，具有缓存机制，如果使用methods方法在多个地方使用则会调用多次。

### v-for

v-for是vue2中的一个指令，用于遍历数组，集合或列表的，它包含有一个 :key 的属性，如果设置唯一性的key则将元素和标签进行了唯一性绑定，元素的变化会带动标签的样式变化。

### route与router

1. route,router的区分

route 是用于路由跳转的，router 是用于路由操作的，一个用于跳转一个用于传递参数，传递参数使用router

2. 关于push和replace的使用场景

push 用于路由跳转，每次跳转都会像栈一样往历史记录中添加路由，replace是通过删除后再添加，相当于替换的形式
push用于各种的没有任何需求的页面转换和跳转，replace是在业务需求，需要跳转后再回跳到原页面不希望上一跳是上一个页面而希望是上上个页面的情况

### 打包时的路由懒加载

源代码
```vue
import List from '@/views/search/list.vue'
```
使用懒加载
```vue
const List = ()=> import('@/views/search/list.vue')
```
使用后打包时就可以将原本打包生成的js文件分解为更多的小的js文件，在需要时再进行加载

## vue3

### setup函数

生命周期早于beforeCreate，所以在setup中无法获取this

### 父传子 子传父

```vue
const props = defineProps({ 属性名: 类型 })
```

```vue
const emit = defineEmits(['方法名'])
```

### defineExpose()

```vue
defineExpose({
    想要暴露的属性名
})
```
使用这个宏函数之后就可以在父组件中获取子组件的属性和方法，否则子组件的属性与方法是默认不开放给父组件的

### 跨层级传递数据

```vue
// 创建，响应式数据也可以传递
provider('键名',值)
// 接收
inject('键名需要对应')
// 数据修改使用创建provider提供的函数进行修改
inject('函数名')
```

### watch监听器

```vue
watch(['监听的属性'],(新值,旧值)=>{})
```

### pinia

用于vuex的替代
```vue
// 通过pinia获取数据时不能使用{} 结构的方式，如果使用就会丢失数据的响应式
const { count,msg } = useCounterStore()
// 正确的使用方法是通过特定的函数获取Store然后再结构
const counterStore = useCounterStore()
const { count,msg } = storeToRefs(counterStore)
```

### 路由配置项

```vue
// createWebHistory历史模式不带# ,createWebHashHistory 带#
import { createRouter, createWebHistory } from 'vue-router'
// 配置项，在vite.config中进行配置,
import.meta.env.BASE_URL
// 如添加base: '/' 后，所有的路由都会加上默认的'/'地址
export default defineConfig({
plugins: [vue()],
base: '/',
resolve: {
alias: {
'@': fileURLToPath(new URL('./src', import.meta.url))
}
}
})
```
