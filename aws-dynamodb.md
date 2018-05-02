# AWS DynamoDB

>

步驟:

1.先設定好AMI，然後參考此文章，將AMI的key設定到電腦檔案中。

[https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html)

2.安裝aws-sdk

```
yarn add aws-sdk
```

3.對照下面表格記下自己的AWS服務位置

```
Code    Name
ap-northeast-1    Asia Pacific (Tokyo)
ap-southeast-1    Asia Pacific (Singapore)
ap-southeast-2    Asia Pacific (Sydney)
eu-central-1    EU (Frankfurt)
eu-west-1    EU (Ireland)
sa-east-1    South America (Sao Paulo)
us-east-1    US East (N. Virginia)
us-west-1    US West (N. California)
us-west-2    US West (Oregon)
```

4.建立一個Dynamo Table

# 新增

```js
var AWS = require('aws-sdk');
AWS.config.update({
  region: 'us-west-1'
});

var docClient = new AWS.DynamoDB.DocumentClient();

var params = {
  TableName: 'market_item',
  Item: {
    id : '0x01',
    test: 123
  }
};

// Call DynamoDB to add the item to the table
docClient.put(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

> 參考官方範例:
>
> [https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html)
>
> 會看到 putItem 而非 put，兩者的差別可參考:
>
> [https://stackoverflow.com/questions/33942945/error-invalidparametertype-expected-params-itempid-to-be-a-structure-in-dyn?utm\_medium=organic&utm\_source=google\_rich\_qa&utm\_campaign=google\_rich\_qa](https://stackoverflow.com/questions/33942945/error-invalidparametertype-expected-params-itempid-to-be-a-structure-in-dyn?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

# 查詢

```js
var AWS = require('aws-sdk');
AWS.config.update({
  region: 'us-west-1'
});


var docClient = new AWS.DynamoDB.DocumentClient();

var params = {
  TableName: 'market_item',
  Key: {
    id : '0x01',
  }
};

// Call DynamoDB to add the item to the table
docClient.get(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

> docClient.put改成 docClient.get 即可，並且把params的Item改為Key

# 更新

需要加上幾個欄位在參數: UpdateExpression, ExpressionAttributeNames, ExpressionAttributeValues, ReturnValues

```
UpdateExpression: 'set #m=:n' 並在後續ExpressionAttributeNames, ExpressionAttributeValues寫上m跟n代表的值
#後面的值到錶要被設定的KEY   
=:後面的值代表要設定的Value
```

ReturnValues代表:

```
ALL_OLD：回傳整個 item 被更新前的資料。
ALL_NEW：回傳整個 item 被更新後的資料。
UPDATED_OLD：僅回傳 item 這次更新的欄位被修改前的資料。
UPDATED_NEW：僅回傳 item 這次更新的欄位資料。
```

範例:

```js
var AWS = require('aws-sdk');
AWS.config.update({
  region: 'us-west-1'
});


var docClient = new AWS.DynamoDB.DocumentClient();

var params = {
  TableName: 'market_item',
  Key: {
      id: '0x01',
  },
  UpdateExpression: 'set #m=:n',
  ExpressionAttributeNames: {
      '#m': 'test'
  },
  ExpressionAttributeValues: {
      ':n': 456
  },
  ReturnValues: 'UPDATED_NEW'
};

// Call DynamoDB to add the item to the table
docClient.update(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

# 刪除

參數與新增相同

```js
var AWS = require('aws-sdk');
AWS.config.update({
  region: 'us-west-1'
});


var docClient = new AWS.DynamoDB.DocumentClient();

var params = {
  TableName: 'market_item',
  Key: {
      id: '0x01',
  }
};

// Call DynamoDB to add the item to the table
docClient.delete(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

不錯文章:

[https://medium.com/@kinvilhsiao/dynamodb-使用-nodejs-4a24c99ea751](https://medium.com/@kinvilhsiao/dynamodb-使用-nodejs-4a24c99ea751)

