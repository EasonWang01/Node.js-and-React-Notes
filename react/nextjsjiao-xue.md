# Next.js 教學

https://nextjs.org/

1.開一個新資料夾，然後cd進去後輸入以下指令。

```
npm init -y
npm install --save react react-dom next
```

2.在package.json的script內加入以下

```
{
  "scripts": {
    "dev": "next"
  }
}
```

3.

```
npm run dev
```

4.之後應該會顯示一個404畫面，這時我們新增一下pages 資料夾，裡面放入一個index.js，內容放入以下。

```js
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```

5.新增其他route時只要在pages內加入檔案即可。

> /pages/about.js

```js
export default () => (
  <div>
    <p>This is the about page</p>
  </div>
)
```



