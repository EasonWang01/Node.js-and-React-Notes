# 範例

> 使用event.headers 可取得 Header
>
> event.body 可取得body

## 註冊使用者

```javascript
    const AWS = require('aws-sdk');
    const uuid = require('uuid');
    const crypto = require('crypto');
    const documentClient = new AWS.DynamoDB.DocumentClient();


    /*
    @param {string} account
    @param {string} password
    */

    exports.handler = function index(event, context, callback) {

      var account = event.account; // 必須為email
      var password = event.password;
      var hashed_password = HMAC_sha256(password.toString());

      function validateEmail(email) {
        var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
        return re.test(String(email).toLowerCase());
      }

      function HMAC_sha256(text) {
        const secret = 'yicheng';
        const hash = crypto.createHmac('sha256', secret)
          .update(text)
          .digest('hex');
        return hash;
      }
      if (!validateEmail(account)) {
        return {
          code: 1,
          text: 'Wrong Email(account) Format'
        }
      }

      // 確認使用者不存在 
      var params = {
        TableName: "market_user",
        Key: {
          account
        }
      };

      documentClient.get(params, function(err, data) {
        if (err) {
          console.log("Error", err);
        } else {
          if (typeof data.Item !== "undefined") {
            context.succeed({ code: 1, result: "User exist" });
            return
          } 

            var params = {
              Item: {
                "id": uuid.v1(),
                "account": account,
                "hashed_password": hashed_password,
                "createDate": Date.now(),
              },
              TableName: "market_user"
            };

            documentClient.put(params, function(err, data) {
              if (err) {
                context.fail(err);
              } else {
                context.succeed({
                  code: 0,
                  result: "Success Create User"
                })
              }
            });

        }
      });

    };
```

## 登入使用者

> 這邊使用到第三方模組，所以要先在本地安裝好後打包成ZIP檔案上傳Lambda。

```javascript
const AWS = require('aws-sdk');
const crypto = require('crypto');
const documentClient = new AWS.DynamoDB.DocumentClient();
const jwt = require('jsonwebtoken');
const jwtPass = "yicheng";

exports.handler = function index(event, context) {
    function HMAC_sha256(text) {
        const secret = 'yicheng';
        const hash = crypto.createHmac('sha256', secret)
            .update(text)
            .digest('hex');
        return hash;
    }

    var account = event.account;
    var hashed_password = HMAC_sha256(event.password.toString());

    var params = {
        TableName: "market_user",
        Key: {
            account
        }
    };
    documentClient.get(params, function(err, data) {
    if (err) {
        console.log("Error", err);
    } else {
        var db_password = data.Item.hashed_password;
        if(db_password === hashed_password) {
            var token = jwt.sign({ account, timestamp: Date.now() }, jwtPass);
            context.succeed({
                code: 0,
                result: 'Success Login',
                token
            })
        } else {
            context.succeed({
                code: 1,
                result: 'Password not match'
            })
        }
    }});
};
```

