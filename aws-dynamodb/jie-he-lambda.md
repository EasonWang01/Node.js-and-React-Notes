# 使用Lambda結合DynamoDB

1.先於IAM創建一個Role。

2.加入如下程式

```js
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



