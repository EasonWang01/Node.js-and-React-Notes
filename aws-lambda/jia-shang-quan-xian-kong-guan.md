



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



