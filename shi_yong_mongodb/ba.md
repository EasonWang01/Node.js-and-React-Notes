# MongoDB 常用指令

# 基本啟動與終止

```
sudo service mongod start
```

```
sudo service mongod stop
```

```
mkdir ./data/db1  
mongod --dbpath ./data/db1
mongod --dbpath ./data/db1 --shutdown
```

mongo預設使用27017port，所以如果要使用robomongo連線EC2上的mongo要先開aws 的port然後記得要在mongo加上auth不然大家都可以連線，很不安全

# \# 創建使用者步驟

依序輸入以下指令

```
1.mongod --dbpath ./data/db1 &
2.mongo
3.use admin
4.db.createUser({ user: "admin", pwd: "輸入密碼", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
5.db.auth("admin", "adminpassword")
```

重新啟動並開啟認證登入

&gt;使用--auth後要記得到admin並用db.auth後才可執行其他操作

```
1.mongod --dbpath ./data/db1 --shutdown
2.mongod --dbpath ./data/db1 --auth  &
3.mongo
4.use admin   // 選擇使用的資料庫
5.db.auth('admin','密碼')  // 登入資料庫使用者
```

新增一個資料庫並且新增使用者\(此時因為剛有登入admin所以可以新增使用者，但新增前無法添加資料\)

```
1.use <要新增的資料庫名稱>
2.db.createUser(
  {
    user: "yicheng",
    pwd: "密碼",
    roles: [ { role: "readWrite", db: "此資料庫名稱" }]
  }
)
```

新增資料至資料庫

```
 db.資料庫名稱.insert({"name":"tutorials point"})
```

存入資料後可看到db被新增進去了

```
show dbs
```

查看現在所在的db名稱

```
db
```

查詢資料

```
db.users.find()
```

顯示所有collection

```
show collections
```

之後連線字串為mongodb://youruser:yourpassword@localhost/yourdatabase

ex:

```
mongoose.connect('mongodb://帳號:密碼@ec2-52-193-141-232.ap-northeast-1.compute.amazonaws.com:27017/資料庫名稱',function(err){
    if(err){throw err};
});
```

# Dump 將 DB 資料導出備份

```
mongodump --authenticationDatabase admin -u admin -p <admin密碼>
```

# Restore 將備份檔案導入DB

```
mongorestore -d <取DB名稱> <要導入的資料夾路徑> --authenticationDatabase admin -u admin -p <admin密碼>
```

# \# 指定可連線的IP

到config檔案設定

```
bind_ip=可連線的IP
```

# \#自訂mongod.conf之config檔案

\(因為使用mongod時不會和使用service啟動一樣自動使用/etc/mongod.conf的設定，需要手動加入--才會去使用config檔案\)

```
https://docs.mongodb.com/manual/reference/configuration-options/#use-the-configuration-file
```

參考至:

[https://medium.com/@matteocontrini/how-to-setup-auth-in-mongodb-3-0-properly-86b60aeef7e8](https://medium.com/@matteocontrini/how-to-setup-auth-in-mongodb-3-0-properly-86b60aeef7e8)

&gt;如果出現[admin user not authorized](https://stackoverflow.com/questions/23943651/mongodb-admin-user-not-authorized)\(\)

[https://stackoverflow.com/questions/35507182/creating-first-user-in-mongodb-3-2](https://stackoverflow.com/questions/35507182/creating-first-user-in-mongodb-3-2)

P.S

1.用service的方式啟動無法用--dbpath指定資料夾路徑

2.用service無法輸入--auth需要打開/etc/mongod.conf並更改下列為如下

```
security:
    authorization: enabled
```

3.admin使用者預設可以連到所有db，如果連不進去通常是因為db上還沒有資料所以db還沒完全建立好，只要先使用admin新建資料庫\(use\)後在資料庫新增使用者\(createUser\)後使用該user登入\(db.auth\)然後新增資料，之後admin也可在該資料庫新增資料了

# 遠端連線mongoDB

[https://ianlondon.github.io/blog/mongodb-auth/](https://ianlondon.github.io/blog/mongodb-auth/)

# \# 讓進程保持--fork

\(記得要指定logpath\)

```
mongod --auth --dbpath ./data/db2 --fork --logpath=./mongodb.log
```

[http://chenzhou123520.iteye.com/blog/1634676](http://chenzhou123520.iteye.com/blog/1634676)

# 注意事項

如果在第一次創建 user 使用

```
db.createUser( { user: "admin", pwd: "<Enter a secure password>", roles: [ { role: "readWriteAnyDatabase", db: "admin" }, { role: "userAdminAnyDatabase", db: "admin" } ] } )
```

> 這樣會讓admin無法刪除DB，因為沒有權限

可以如下修改

```
mongo -u admin -p 密碼 --authenticationDatabase=admin
use admin
db.grantRolesToUser('admin',[{ role: "root", db: "admin" }])
```



