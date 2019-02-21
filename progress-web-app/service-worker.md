# Service worker

#### 1. 首先要先加入 Manifest，參考如下步驟

https://developer.mozilla.org/en-US/docs/Web/Manifest

之後可以確認：

![](/assets/Screen Shot 2019-02-21 at 4.33.38 PM.png)

#### 2. 

在App.js之類的地方加入：

```js
import * as serviceWorker from './sw.js';
serviceWorker.register();
```

#### 3.

sw.js

```js
export function register(config) {
  if ('serviceWorker' in navigator) {

    window.addEventListener('load', () => {
      const swUrl = `/service-worker.js`;
      registerValidSW(swUrl, config);
    });
  }
}

function registerValidSW(swUrl, config) {
  navigator.serviceWorker
    .register(swUrl, {
      scope: '.',
    })
    .then(registration => {
      registration.onupdatefound = () => {
        const installingWorker = registration.installing;
        if (installingWorker == null) {
          return;
        }
        installingWorker.onstatechange = () => {
          if (installingWorker.state === 'installed') {
            if (navigator.serviceWorker.controller) {
              // Execute callback
              if (config && config.onUpdate) {
                config.onUpdate(registration);
              }
            } else {
              console.log('Content is cached for offline use.');
              // Execute callback
              if (config && config.onSuccess) {
                config.onSuccess(registration);
              }
            }
          }
        };
      };
    })
    .catch(error => {
      console.error('Error during service worker registration:', error);
    });
}
```

#### 4.

上面我們有寫 /service-worker.js; 所以我們要把他加在 /public 資料夾中，讓網域可以直接存取到。

```js
self.addEventListener('install', () => self.skipWaiting());

self.addEventListener('activate', () => {
  self.clients.matchAll({ type: 'window' }).then(windowClients => {
    for (let windowClient of windowClients) {
      windowClient.navigate(windowClient.url);
    }
  });
});

self.addEventListener('fetch', event => {});
```



---

其他文章: [https://developer.mozilla.org/zh-CN/docs/Web/API/Service\_Worker\_API/Using\_Service\_Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API/Using_Service_Workers)

