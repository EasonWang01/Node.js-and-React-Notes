```
sudo service mongod start
sudo service mongod stop
```

```
mkdir ./data/db1  
mongod --dbpath ./data/db1
mongod --dbpath ./data/db1 --shutdown
```

mongo預設使用27017port，所以如果要使用robomongo連線EC2上的mongo要先開aws 的port然後記得要在mongo加上auth不然大家都可以連線，很不安全

加上auth 帳號密碼的方式如下

[https://medium.com/@matteocontrini/how-to-setup-auth-in-mongodb-3-0-properly-86b60aeef7e8](https://medium.com/@matteocontrini/how-to-setup-auth-in-mongodb-3-0-properly-86b60aeef7e8)

&gt;如果出現[admin user not authorized](https://stackoverflow.com/questions/23943651/mongodb-admin-user-not-authorized)\(\)

[https://stackoverflow.com/questions/35507182/creating-first-user-in-mongodb-3-2](https://stackoverflow.com/questions/35507182/creating-first-user-in-mongodb-3-2)

用service的方式啟動無法用--dbpath指定資料夾路徑



\#創建使用者步驟

依序輸入以下指令

```
1.mongod --dbpath ./data/db1 &
2.mongo
3.use admin
4.db.createUser({ user: "admin", pwd: "adminpassword", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
5.db.auth("admin", "adminpassword")


```



