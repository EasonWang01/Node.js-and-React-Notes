# AWS DynamoDB

步驟:

1.先設定好AMI，然後參考此文章，將AMI的key設定到電腦檔案中。

[https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html)

2.安裝aws-sdk

```
yarn add aws-sdk
```

3.對照下面表格記下自己的AWS服務位置

```
Code	Name
ap-northeast-1	Asia Pacific (Tokyo)
ap-southeast-1	Asia Pacific (Singapore)
ap-southeast-2	Asia Pacific (Sydney)
eu-central-1	EU (Frankfurt)
eu-west-1	EU (Ireland)
sa-east-1	South America (Sao Paulo)
us-east-1	US East (N. Virginia)
us-west-1	US West (N. California)
us-west-2	US West (Oregon)
```

4.建立一個Dynamo Table

# 寫入

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
> https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html
>
> 會看到 putItem 而非 put，兩者的差別可參考:
>
> https://stackoverflow.com/questions/33942945/error-invalidparametertype-expected-params-itempid-to-be-a-structure-in-dyn?utm\_medium=organic&utm\_source=google\_rich\_qa&utm\_campaign=google\_rich\_qa



