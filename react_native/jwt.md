# JWT Token

Server可使用此模組

`https://github.com/auth0/node-jsonwebtoken`

但Client需要使用

`https://github.com/auth0/jwt-decode`

> 因為React-native 沒有stream模組，不能用`node-jsonwebtoken`
>
> 的verify功能。



# 加密方法：ES256、HS256等

專門給JSON Web Algorithms \(JWA\) 所使用。

![](/assets/Screen Shot 2018-10-05 at 11.10.04 AM.png)

http://self-issued.info/docs/draft-ietf-jose-json-web-algorithms-00.html



