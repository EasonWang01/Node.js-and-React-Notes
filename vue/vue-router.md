# Vue router

1. `yarn add vue-router`
2. 記得在 index.html 加上如下

```javascript
    <div id="app">
      <router-view></router-view>
    </div>
```

3. main.js

```javascript
import Vue from 'vue'
import App from '@/App.vue'
import Test from "@/components/Test.vue";
import VueRouter from 'vue-router'

Vue.use(VueRouter)
Vue.config.productionTip = false

const routes = [
  { path: '/', component: App },
  { path: '/foo', component: Test },
]
const router = new VueRouter({
  mode: 'history',
  routes 
})
new Vue({
  router,
}).$mount('#app')

```

