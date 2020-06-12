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

4. 點擊換頁面

```javascript
<template id="test">
  <div>
    <button v-on:click="gotoHome">test</button>
  </div>
</template>

<script>
export default {
  name: "test",
  mounted: () => {
    console.log("test mounted");
  },
  methods: {
    gotoHome() { // 不能用 arrow function
      this.$router.push("/");
    }
  }
};
</script>
```

