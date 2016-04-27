# React Native
----

###windows環境建置
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