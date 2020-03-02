# 結合Lambda

## 使用Lambda結合DynamoDB

1.先於IAM創建一個Role。

2.在Lambda 建立Function 並在下方的 Execution role 選擇剛才創建的Role。

3.加入如下程式

> function index 對應到index.js相同檔案名稱

```javascript
var AWS = require('aws-sdk'),
    uuid = require('uuid'),
    documentClient = new AWS.DynamoDB.DocumentClient(); 

exports.handler = function index(event, context, callback){
    var params = {
        Item : {
            "id" : uuid.v1(),
            "Name" : event.name
        },
        TableName : event.tableName
    };
    documentClient.put(params, function(err, data){
        callback(err, data);
    });
}
```

> 注意，搜尋的回傳值會要用
>
> ```text
> data.Item 取出
> ```

4.進行測試

5.加入API gateway即可

## API gateway授權

當我們要確認用戶有權限可以打API 時可以先發一隻 AUTH TOKEN 請求

[http://akuma1.pixnet.net/blog/post/321461539-（十一）​​​​​​​api-gateway-custom-authoriz](http://akuma1.pixnet.net/blog/post/321461539-（十一）​​​​​​​api-gateway-custom-authoriz)

