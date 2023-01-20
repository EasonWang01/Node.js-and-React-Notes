---
description: TOPT ( Time-based One-Time Passwords) 2FA
---

# Google authenticator

### 定義在 RFC-6238

{% embed url="https://datatracker.ietf.org/doc/html/rfc6238" %}

{% embed url="https://www.twilio.com/docs/glossary/totp" %}

{% embed url="https://github.com/google/google-authenticator/wiki/Key-Uri-Format#issuer" %}

{% embed url="https://rootprojects.org/authenticator/" %}

{% embed url="https://git.coolaj86.com/coolaj86/browser-authenticator.js" %}

第三方 Node.js 模組：

{% embed url="https://github.com/speakeasyjs/speakeasy" %}

{% embed url="https://github.com/yeojz/otplib" %}

{% embed url="https://authenticatorapi.com/" %}

## 程式範例

以下使用 `speakeasy` 模組，產生 secret 與 qrcode，之後 secret 要記住在後端

```javascript
const speakeasy = require("speakeasy");
const qrcode = require("qrcode");

const secret = speakeasy.generateSecret({ name: "eason@gmail.com" });

console.log(secret)

qrcode.toDataURL(secret.otpauth_url, function (err, url) {
  console.log(url);
});
```

驗證

```javascript
const speakeasy = require("speakeasy");

const verifyResult = speakeasy.totp.verify({
  secret: "<剛才的 secret>",
  encoding: "ascii",
  token: "<手機 google authenticator app 掃碼 qrcode 後產生的六個號碼>"
})

console.log('verifyResult', verifyResult)
```

{% embed url="https://github.com/speakeasyjs/speakeasy#generateSecret" %}

{% embed url="https://www.youtube.com/watch?v=6mxA9Zp8600" %}

## 演算法：

[https://github.com/bellstrand/totp-generator/blob/master/index.js](https://github.com/bellstrand/totp-generator/blob/master/index.js)
