# rethinkdb



# RethinkDB(使用ReQL 為query language)

https://www.rethinkdb.com/docs/quickstart/

```
brew update && brew install rethinkdb
```
windows直接下載後點擊exe，在前往
```
http://localhost:8080/
```

使用Node.js

```
npm install rethinkdb
```

```
r = require('rethinkdb');

var connection = null;
r.connect( {host: 'localhost', port: 28015}, function(err, conn) {
    if (err) throw err;
    connection = conn;
    console.log(conn);
})
```
建立Table
```
r = require('rethinkdb');

r.connect( {host: 'localhost', port: 28015}, function(err, connection) {
    if (err) throw err;
    

    r.db('test').tableCreate('authors').run(connection, function(err, result) {
    if (err) throw err;
    console.log(JSON.stringify(result, null, 2));
});

});

```
```
generated_keys為主鍵的意思
```
#取得資料
```
r = require('rethinkdb');

r.connect( {host: 'localhost', port: 28015}, function(err, connection) {
    if (err) throw err;
    

r.table('authors').run(connection, function(err, cursor) {
    if (err) throw err;
    cursor.toArray(function(err, result) {
        if (err) throw err;
        console.log(JSON.stringify(result, null, 2));
    });
});

});



```
#體驗rethinkDB的realtime
```
r = require('rethinkdb');

r.connect( {host: 'localhost', port: 28015}, function(err, connection) {
    if (err) throw err;
    
r.table('authors').changes().run(connection, function(err, cursor) {
    if (err) throw err;
    cursor.each(function(err, row) {
        if (err) throw err;
        console.log(JSON.stringify(row, null, 2));
    });
});

});



```
執行後再開另一個CMD


檔案改成
```
r = require('rethinkdb');

r.connect( {host: 'localhost', port: 28015}, function(err, connection) {
    if (err) throw err;
    
r.table('authors').update({type: "fictional"}).
    run(connection, function(err, result) {
        if (err) throw err;
        console.log(JSON.stringify(result, null, 2));
    });

});



```
在另一個CMD執行server，發現兩個CMD都有console出現


參考至:https://www.rethinkdb.com/docs/guide/javascript/

https://www.rethinkdb.com/docs/cookbook/javascript/
