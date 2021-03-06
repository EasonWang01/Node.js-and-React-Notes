# JWT



## JWT Token

Server可使用此模組

`https://github.com/auth0/node-jsonwebtoken`

但Client需要使用

`https://github.com/auth0/jwt-decode`

> 因為React-native 沒有stream模組，不能用`node-jsonwebtoken`
>
> 的verify功能。

## 加密方法：ES256、HS256等

專門給JSON Web Algorithms \(JWA\) 所使用。

![](/assets/Screen%20Shot%202018-10-05%20at%2011.10.04%20AM.png)

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

只有單純用 base64URL 編碼，最後面的字段才是加密過的，但只是用來驗證，包含 encode 時的 secret 

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

