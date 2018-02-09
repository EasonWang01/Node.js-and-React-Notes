# 打包應用程式

## pkg

[https://github.com/zeit/pkg](https://github.com/zeit/pkg)

我們可以使用 pkg 這個模組來打包應用程式為可執行檔，之後可直接執行，不用安裝Node.js環境。

#### 安裝

```
npm install pkg -g
```

#### 使用

```
pkg ./test.js --output ./test.exe
```

#### 更換ICON

需要使用node-rcedit 模組

[https://github.com/electron/node-rcedit](https://github.com/electron/node-rcedit)

```js
var rcedit = require('rcedit')

rcedit('./pkttest-win.exe' , {
  icon: './img.ico'
},(err) => {
  console.log(err)
})
```

> 指定exe檔案位置與ico位置後即可更換可執行檔的icon
>
> 如果執行成功後圖片沒變，把檔案移動到別的資料夾即可生效

只能使用.ico檔案，無法使用png或jpg，可用此轉換:[http://icoconvert.com/](http://icoconvert.com/)

