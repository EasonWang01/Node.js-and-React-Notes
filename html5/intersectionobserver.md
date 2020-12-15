# IntersectionObserver

可以用來偵測目前畫面是否捲動到特定元素，可用來作 infinite scroll 等功能。

```javascript
let options = {
  root: document.querySelector('#scrollArea'),
  rootMargin: '0px',
  threshold: 1.0
}

let observer = new IntersectionObserver(callback, options);
```

[https://developer.mozilla.org/en-US/docs/Web/API/Intersection\_Observer\_API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

