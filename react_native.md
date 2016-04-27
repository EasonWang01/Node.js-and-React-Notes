# React Native
----

###1.windows環境建置
>需求安裝:
>
>1.JDK   
>
>2.Android Studio 
>
>3.Genymotion
>
>4.Node.js


1.npm install -g react-native-cli    `安裝`

2.react-native init  AwesomeProject   `react native創建資料夾`

3.安裝JDK與android studio後，創建一個新專案

4.進到專案資料夾把local.properties 複製到AwesomeProject 的android資料夾內   `產生環境變數`

5.開啟android studio 點選上方圖案(SDK manger)，之後再點Launch Standalone SDK manger
  勾選build tool後安裝

6.在cmd輸入react-native run-android(記得開好genymotion)，正常跑完會出現紅色底字，這時因為我們還沒啟動react-native的server

7.於cmd輸入 react-native start ，之後於genymotino點選Reload JS即可

8.正常啟動後，可點選index.android.js修改，之後點genymotion的右側menu，設定hot reload   `儲存後即立刻更改畫面`



#2.React native專案檔案介紹

####1.index.android.js

我們一般會在該檔案，寫為如下
```
'use strict';

import React, { AppRegistry } from 'react-native';
import App from './app/containers/app.js';

AppRegistry.registerComponent('AwesomeProject', () => App); 

```
特別要注意的地方為

`AppRegistry.registerComponent('AwesomeProject', () => App); `

`registerComponent`第一個參數要跟資料夾名稱相同，第二個參數要跟你的component enrty相同

####2.app目錄

我們假設你已經學過React跟redux的基礎，以下我們稱react-native簡稱為native

在native中，和一般node專案一樣使用npm安裝套件，接著再跟目錄創建app資料夾，將專案相關文件都寫於裡面

#3.implement Redux

範例連結:
https://github.com/alinz/example-react-native-redux

下載後

1.將app資料夾放入你的project

2.`npm install redux react-redux redux-thunk --save`

3..將`index.android.js`改為如下即可運行
```
'use strict';

import React, { AppRegistry } from 'react-native';
import App from './app/containers/app.js';

AppRegistry.registerComponent('AwesomeProject', () => App);

```
