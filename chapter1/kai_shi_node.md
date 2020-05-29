# 開始Node

## 開始Node

### 安裝

windows

```text
官網下載安裝包即可
```

Linux

方法1.\(推薦\)

```text
sudo apt-get install npm

sudo npm cache clean -f
sudo npm install -g n
sudo n stable

sudo ln -sf /usr/local/n/versions/node/<VERSION(只有數字沒有v)>/bin/node /usr/bin/node
```

方法2.

```text
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
```

然後

```text
sudo apt install -y nodejs
```

方法3.

```text
wget https://nodejs.org/dist/v4.4.7/node-v4.4.7-linux-x86.tar.xz
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

## 在linux看到版本是0.xx.xx

```text
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```

使用n套件來更新

參考以下兩篇文章，有關nodejs更新

[http://theholmesoffice.com/node-js-fundamentals-how-to-upgrade-the-node-js-version/](http://theholmesoffice.com/node-js-fundamentals-how-to-upgrade-the-node-js-version/)

[http://askubuntu.com/questions/594656/how-to-install-the-latest-versions-of-nodejs-and-npm-for-ubuntu-14-04-lts](http://askubuntu.com/questions/594656/how-to-install-the-latest-versions-of-nodejs-and-npm-for-ubuntu-14-04-lts)

### 開始第一個專案

mkdir class1

npm init

發現資料夾多了一個package.json

**注意:不要將主目錄名稱的開頭取名為和任何你要安裝的package名稱相同，安裝時會出問題**

## 使用scripts標籤下加入名稱，可使用npm run +名稱

package.json

```text
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

