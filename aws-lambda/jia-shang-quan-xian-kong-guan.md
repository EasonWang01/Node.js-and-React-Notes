# 文件與文章

https://docs.aws.amazon.com/zh\_cn/apigateway/latest/developerguide/api-gateway-custom-authorizer-output.html

http://akuma1.pixnet.net/blog/post/321461539-%EF%BC%88%E5%8D%81%E4%B8%80%EF%BC%89%E2%80%8B%E2%80%8B%E2%80%8B%E2%80%8B%E2%80%8B%E2%80%8B%E2%80%8Bapi-gateway-custom-authoriz

https://auth0.com/docs/integrations/aws-api-gateway/custom-authorizers/part-3

> Auth0的文章最詳細。

# 流程

當我們發出Request後給API gateway會想要讓認證過的使用者才能繼續執行程式。

API Gateway的Authorizers可以讓該API gateway接收請求時也接受一個Token，並且執行一個寫在Lambda的程式，該程式如下，會驗證Token，驗證成功會回傳固定格式的權限參數。

> 此處使用jsonwebtoken模組產生一個token

我們先在AWS Lambda創建如下函式

Lambda 範例:

```js
const AWS = require('aws-sdk');
const jwt = require('jsonwebtoken');
const jwtPass = "yicheng";

exports.handler = function index(event, context, callback) {
  const token = event.headers.token;
  jwt.verify(token, jwtPass, function(err, decoded) {
    if(err) {
      callback("Error: Invalid token"); 
      return
    }
    if(typeof decoded.account !== "undefined") {
      console.log(decoded)
      callback(null, generatePolicy('user', 'Allow'));
    } 
  });
};


var generatePolicy = function(principalId, effect, resource) {
    var authResponse = {};

    authResponse.principalId = principalId;
    if (effect) {
        var policyDocument = {};
        policyDocument.Version = '2012-10-17'; 
        policyDocument.Statement = [];
        var statementOne = {};
        statementOne.Action = 'execute-api:Invoke'; 
        statementOne.Effect = effect;
        statementOne.Resource = ["arn:aws:execute-api:us-west-1:795263033835:heeruzit3m/*/POST/"];
        policyDocument.Statement[0] = statementOne;
        authResponse.policyDocument = policyDocument;
    }

    // Optional output with custom properties of the String, Number or Boolean type.
    authResponse.context = {
        "stringKey": "stringval",
        "numberKey": 123,
        "booleanKey": true
    };
    return authResponse;
}
```

之後我們到API Gateway選擇Authorizers新增一個認證，並且選擇剛才寫的Lambda Function

> 下面範例在之後會接受一個名為token的Header

![](/assets/004.png)

