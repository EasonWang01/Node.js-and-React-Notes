註冊使用者

```js
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



