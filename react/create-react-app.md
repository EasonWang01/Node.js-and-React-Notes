# Create-react-app

> 為一個快速建立React 模板的工具



## 更改預設PORT

1.新增 `.env `

```
PORT=8002
```

2.之後啟動後會自動執行在8002 PORT

3.如果要讓Node.js 檔案也可以讀.env檔案可以使用以下模組

```
yarn add dotenv
```

然後

```
require('dotenv').load();
console.log(process.env)
```



