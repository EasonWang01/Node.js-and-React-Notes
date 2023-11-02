---
description: JSON Web Token 一般用來做用戶驗證使用
---

# JWT Token

> JWT token 的內容經過 base64 編碼，包含 header, payload, signature，使用 . 分隔，例如：
>
> eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV\_adQssw5c

所以任何人拿到 token 後都可以 decode 出原本 payload 內容，但因為 encode jwt token 時會使用 private key，所以只有對應的公鑰持有人可以驗證（非對稱加密驗證）。

## Encode 創建 RSA JWT Token 範例

```javascript
const fs = require('fs');
const jwt = require('jsonwebtoken');

// Replace these paths with your RSA private and public key files
const privateKeyPath = 'path/to/private-key.pem';
const publicKeyPath = 'path/to/public-key.pem';

// Load the RSA private and public keys
const privateKey = fs.readFileSync(privateKeyPath, 'utf8');
const publicKey = fs.readFileSync(publicKeyPath, 'utf8');

// Sample payload
const payload = {
  sub: '1234567890',
  name: 'Eason Wang',
  iat: Math.floor(Date.now() / 1000),
};

// Encode (Sign) a JWT Token
const token = jwt.sign(payload, privateKey, { algorithm: 'RS256' });
console.log('Encoded Token:', token);

// Decode (Verify) a JWT Token
jwt.verify(token, publicKey, { algorithms: ['RS256'] }, (err, decoded) => {
  if (err) {
    console.error('JWT verification failed:', err.message);
  } else {
    console.log('Decoded Token:', decoded);
  }
});

```

## Decode 範例

```javascript
function decodeJwt(token) {
  // Split the token into header, payload, and signature
  const [headerB64, payloadB64] = token.split('.');

  // Decode the base64-encoded header and payload
  const header = JSON.parse(atob(headerB64));
  const payload = JSON.parse(atob(payloadB64));

  return {
    header,
    payload,
  };
}

// Your JWT token
const token = 'YOUR_JWT_TOKEN_HERE';

// Decode the JWT token
const decoded = decodeJwt(token);

// Access the header and payload
console.log('Decoded Header:', decoded.header);
console.log('Decoded Payload:', decoded.payload);

```

## JWT Token

Server可使用此模組

`https://github.com/auth0/node-jsonwebtoken`

但Client需要使用

`https://github.com/auth0/jwt-decode`

> 因為React-native 沒有stream模組，不能用`node-jsonwebtoken`
>
> 的verify功能。

## 加密方法：ES256、HS256等

專門給JSON Web Algorithms (JWA) 所使用。

[http://self-issued.info/docs/draft-ietf-jose-json-web-algorithms-00.html](http://self-issued.info/docs/draft-ietf-jose-json-web-algorithms-00.html)

## 驗證方法：

一般使用HMAC的話直接在client和server都給密碼即可解密，但這樣可能不安全。

所以可以用ECDSA或RSA等非對稱方式，產生公鑰與私鑰。

流程如下：

> 有時會有一個專門驗證Token的Server先記錄所有project對應使用的公鑰與私鑰對。

1. Server接到client的登入資訊後，Server使用私鑰對資訊簽名，產生JWT Token傳給client
2. Client擁有public key，使用public key確認JWT Token，驗證是從正確的Server 私鑰簽發的。
3. Client發送請求時，帶上JWT Token，Server接到後使用對應client 的 公鑰驗證 Token。

## JWT 的內容是沒有加密的

只有單純用 base64URL 編碼，最後面的字段才是加密過的，但只是用來驗證，包含 encode 時的 secret&#x20;

可以用如下解析 token：

```javascript
        function parseJwt (token) {
            var base64Url = token.split('.')[1];
            var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            var jsonPayload = decodeURIComponent(atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            return JSON.parse(jsonPayload);
        };
```
