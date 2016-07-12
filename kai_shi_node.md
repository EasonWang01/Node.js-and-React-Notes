# 開始Node



##安裝
windows
```
官網下載安裝包即可
```
Linux
```
1. wget https://nodejs.org/dist/v4.4.7/node-v4.4.7-linux-x86.tar.xz
2.xz -d node-v4.4.7-linux-x86.tar.xz
3.tar xvf node-v4.4.7-linux-x86.tar
4.export PATH=$PATH:/home/IXD/node-v4.4.7-linux-x86
或是直接讓每次開機加入路徑
echo 'PATH=$PATH:/opt/nodejs/bin' >> ~/.bashrc


5.之後輸入nodejs即可(非node)

安裝NPM

sudo apt-get install npm

之後輸入npm即可
```


##開始第一個專案
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



