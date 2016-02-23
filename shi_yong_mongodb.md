# 使用MongoDB

使用MongoLab 當練習範例

```
https://mongolab.com/
```

1.先註冊帳號

2.創建資料庫，選地區時記得選擇有免費的

3.創建database內的user

4.創建database內的collection

5.在collection內加入document

##如何測試剛創好的資料庫?

使用Robomongo  https://robomongo.org/

點選Download，選最下面的免費選項下載

1.下載完後開啟，先點選左上方的create 

2.設定一下相關database url、port、username、password(database的，不是你帳戶的)

3.點選左下Test，如成功即可點選save，並進行connect

可參考http://www.codedata.com.tw/database/mongodb-tutorial-1-setting-up-cloud-env/

#如何用Node.js連線到MongoLab?

先使用npm 安裝
```
npm install mongodb
```