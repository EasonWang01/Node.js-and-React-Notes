# 瀏覽器快取與緩存（Expires, Last-modified）

# Expires

我們一樣使用 Node.js server，然後把回應改為以下：

```js
res.setHeader("Expires", new Date("2025-02-10").toUTCString())
res.end('123');
```

只要 Expires 後面的參數是未來的時間都會進行緩存。（disk cache）

![](/assets/螢幕快照 2019-07-03 下午6.19.27.png)

# Last-modified

與 Expires 相反，只要是日期是過去式都會被快取。

```js
res.setHeader("Last-modified", new Date("2005-02-10").toUTCString())
```



