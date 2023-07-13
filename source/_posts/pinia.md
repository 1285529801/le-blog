---
title: pinia学习
---
是一个集中式状态管理器，类似于vuex

**安装pinia**

```shell
npm install pinia
```

## 1. 基本使用

1. **创建大仓库并调用**

    ```tsx
    // main.ts 中
    import { createApp } from 'vue'
    import './style.css'
    import App from './App.vue'
    +++ import { createPinia } from 'pinia'
    
    +++ const pinia = createPinia() //  使用createPinia创建大仓库
    createApp(App).use(pinia).mount('#app')
    ```

2. **创建一个小仓库**

   ```tsx
   import { defineStore } from 'pinia'
   
   export const useMainStore = defineStore('main', {
     // state 用的是箭头函数的返回值
     state: () => {
       return {
         money: 100
       }
     },
     getters: {
         
     },
     actions: {
         
     },
   })
   ```

3. **在组件中使用**

   ```vue
   <script setup lang="ts">
     import { useMainStore } from '../store/index'
     let mainStore = useMainStore()
     console.log(mainStore);
   </script>
   
   <template>
     <h1>{{ mainStore.money }}</h1>
   </template>
   ```

## 2. 对state进行解构

对拿到的state进行解构，使用数据更方便

```vue
<script setup lang="ts">
import { useMainStore } from '../store/index'
// 引入 storeToRefs
import { storeToRefs } from 'pinia'
let mainStore = useMainStore()
// 使用该API进行解构，否则直接解构会丢失数据的响应式
const { money } = storeToRefs(mainStore)
console.log(mainStore);
</script>

<template>
<h1>{{ money }}</h1>
</template>
```

## 3. 对state的修改

1. **简单修改**

   ```vue
   <script setup lang="ts">
   import { useMainStore } from '../store/index'
   let mainStore = useMainStore()
   
   mainStore.money += 100
   mainStore.money = 'lxl'    
   </script>
   ```

2. **使用 `$patch` 批量修改**

   ```vue
   <script setup lang="ts">
   import { useMainStore } from '../store/index'
   let mainStore = useMainStore()
   // $pacth内部为一个函数，参数为state，在里面可以对多个state一起修改
   mainStore.$patch(state => {
       state.money +=100
   })    
   </script>
   ```

3. **配置使用Action**

   ```ts
   import { defineStore } from 'pinia'
   
   export const useMainStore = defineStore('main', {
     // state 用的是箭头函数的返回值
     state: () => {
       return {
         money: 100
       }
     },
     getters: {
         
     },
     actions: {
         // 方法不能使用箭头函数，因为没有this
         changeMoney () {
             this.money +=100
         }
     },
   })
   ```

   ```vue
   <script setup lang="ts">
   import { useMainStore } from '../store/index'
   let mainStore = useMainStore()
   // 直接调用 Action 中的方法
   mainStore.changeMoney()
   </script>
   ```

## 4. Getters

> 计算属性，跟vuex中一样

```tsx
import { defineStore } from 'pinia'

export const useMainStore = defineStore('main', {
  // state 用的是箭头函数的返回值
  state: () => {
    return {
      money: 100
    }
  },
  getters: {
      // 第一种写法
      counAdd():number{
          // 使用this来计算需要指定函数返回值类型
          return this.money +=10 
      }
      // 第二种写法
      counAdd(state){
    	  // 可选参数state，通过state.xxx 来计算
          return state.money +=10 
      }
  },
  actions: {
      
  },
})
```

