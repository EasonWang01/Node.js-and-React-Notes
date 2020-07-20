# ES6 Proxy

可以改變原本預設的行為

```javascript
const person = {
  name: "Test"
};

const proxy = new Proxy(person, {
  get: function(target, propKey) {
    if (propKey in target) {
      return target[propKey];
    } else {
      throw new ReferenceError(`${propKey} do not exist.`);
    }
  }
});

proxy.name // "Test"
proxy.age // 出現自定義的錯誤，如果沒用 proxy 只會出現 undefined
```

