# Crypto加密



# #簡單字串加密

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
>TypeError: error:0606506D:digital envelope routines:EVP_DecryptFinal_ex:wrong final block length

可把`'hex'`改為`'binary'`試試