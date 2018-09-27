# Crypto加密

# 簡單字串加密

```
let cipher = crypto.createCipher('aes-256-cbc','testkey');
let crypted = cipher.update('要加密的東西','utf8','hex');
crypted += cipher.final('hex');
```

解密

```
let decipher = crypto.createDecipher('aes-256-cbc','testkey');
let dec = decipher.update(crypted,'hex','utf8');
dec += decipher.final('utf8');
console.log('解密的文本：'+dec);
```

注意：如果產生錯誤

> TypeError: error:0606506D:digital envelope routines:EVP\_DecryptFinal\_ex:wrong final block length

可把`'hex'`改為`'binary'`試試

# JWT Token產生

[https://github.com/auth0/node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)

Server

> 以下為預設的HMAC加密，第二個參數放入secret，可以放入客戶password

```js
// Generate JWT token
const token = jwt.sign(result.rows[0], result.rows[0].password);
console.log(token)
```

Client

> 登入時驗證password正確，token從localstorage等地方取出。

```js
const decoded = jwt.verify(token, result.rows[0].password);
console.log(decoded) // bar
```

如果客戶端已經有Token則直接使用密碼與verify解密即可驗證，不用再次去DB query。

