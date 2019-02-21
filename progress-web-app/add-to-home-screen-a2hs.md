# A2HS

必須要先註冊 Service Worker

> 可以如下確認：
>
> ![](/assets/Screen Shot 2019-02-21 at 4.30.00 PM.png)

如果成功的話會自動觸發beforeinstallprompt

```js
    window.addEventListener('beforeinstallprompt', e => {
      e.preventDefault();
      e.prompt();
    });
```

之後就會跳出是否要加入桌面的確認框。

https://developer.mozilla.org/en-US/docs/Web/Progressive\_web\_apps/Add\_to\_home\_screen

