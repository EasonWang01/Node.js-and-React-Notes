# HTTPS

發送 request

```javascript
var https = require('https');

var options = {
  host: 'www.google.com',
  port: 443,
  path: '/upload',
  method: 'POST'
};

var req = https.request(options, function(res) {
  console.log('STATUS: ' + res.statusCode);
  console.log('HEADERS: ' + JSON.stringify(res.headers));
  res.setEncoding('utf8');
  res.on('data', function (chunk) {
    console.log('BODY: ' + chunk);
  });
});

req.on('error', function(e) {
  console.log('problem with request: ' + e.message);
});

// write data to request body
req.write('data\n');
req.write('data\n');
req.end();
```

## OpenSSL

開放原始碼的 SSL library。

[https://www.openssl.org/](https://www.openssl.org/)

## BoringSSL

google 開發的 SSL library 用來取代 OpenSSL。

[https://boringssl.googlesource.com/boringssl/](https://boringssl.googlesource.com/boringssl/)

