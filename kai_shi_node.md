# 開始Node

mkdir class1

npm init

發現資料夾多了一個package.json


## 

**注意:不要將主目錄名稱的開頭取名為和任何你要安裝的package名稱相同，安裝時會出問題**

## 


#使用scripts標籤下加入名稱，可使用npm run +名稱

package.json
```
{
  "name": "class1webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  },
  "author": "Eason",
  "license": "ISC",
  "dependencies": {
    "webpack": "^1.12.13",
    "webpack-dev-server": "^1.14.1"
  }
}
```



