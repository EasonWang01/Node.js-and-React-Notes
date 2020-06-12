# Vuex

類似於 Redux，處理資料流。

```text
yarn add vuex
```

main.js

```javascript
import Vuex from 'vuex'

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state, num) {
      state.count =+ num;
    }
  }
})

new Vue({
  router,
  store,
}).$mount('#app')
```

App.vue

使用以下即可存取 state

```javascript
this.$store.state.count
```

發送 action 更改 state 如下

> 第一個參數為 mutations 對應的名稱，第二個參數為額外給的

```javascript
this.$store.commit('increment', 2)
```

