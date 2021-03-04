---
description: 類似於 React.js 或 Angular 之前端框架
---

# Vue

## 建立專案

```text
yarn global add @vue/cli
vue create my-project
```

## 基本結構

App.vue

```javascript
<template>
  ...html
</template>

<script>
export default {
  name: "App", // component 的名稱
  methods: {
    //onclick 之類 dom 觸發的方法都寫這
  },
  data: () => ({
    //類似於 react state
  })
};
</script>

<style>
  #app {
    // style 寫這 
  }
</style>
```

main.js 寫一些 初始化的行為

```javascript
import Vue from 'vue'
import App from '@/App.vue'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import VueRouter from 'vue-router'

Vue.use(VueRouter)
Vue.config.productionTip = false
Vue.use(ElementUI);

const routes = [
  { path: '/', component: App },
]
const router = new VueRouter({
  mode: 'history',
  routes 
})
new Vue({
  router,
}).$mount('#app')

```

## Ref

存取某個元素時使用

```javascript
<input ref="input">
```

之後用以下存取

```javascript
this.$refs.input
```

> 也可以在 child compoent 加上 ref，之後用 this.$refs 存取到後可改變 child 內的 state 或 call child function

## 綁定 dom 觸發事件

> v-on:click 可改為 @click

```javascript
<button v-on:click="addTodo">addTodo</button>

<script>
export default {
  name: "test",
  methods: {
    addTodo() {
      ...
    }
  }
};
</script>
```

## Render List \(類似 react map render\)

```javascript
<div v-for="item in items" :key="item.id">{{ item.message }}</div>

<script>
export default {
  name: "App",
  data: () => ({
    items: [{ message: "Foo" }, { message: "Bar" }]
  })
};
</script>
```

## Watch prop

```javascript
    props: ['project_id'],
    watch: { 
      project_id: function(newVal, oldVal) {
        console.log(newVal, oldVal)
      }
    },
```

## Template

類似於 react 的

```javascript
<> 
 ...
</>
```

可以當作空的 html tag

[https://vuejs.org/v2/guide/list.html\#v-for-on-a-lt-template-gt](https://vuejs.org/v2/guide/list.html#v-for-on-a-lt-template-gt)

