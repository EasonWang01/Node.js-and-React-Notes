# Google-recaptcha

## 簡介:給人類方便給bot困難

\(類似於csrf token的功能\)

## \#1.  安裝

[https://www.google.com/recaptcha](https://www.google.com/recaptcha)

如果用react.js可以使用[https://github.com/appleboy/react-recaptcha](https://github.com/appleboy/react-recaptcha模組)克服dom沒出現但recaptcha執行造成無法顯示的情況

## \#2.使用

文件:[https://developers.google.com/recaptcha/docs/verify](https://developers.google.com/recaptcha/docs/verify)

使用react-recaptcha套件的範例

```text
<Recaptcha
  verifyCallback={(r) => console.log(r)}
  theme="light"
  sitekey="6LejmSQUAAAAACsss6nydz-73jHrJBqlH5xa2WR0"
  onloadCallback={() => { }}
/>
```

> 將recapcha置中
>
> ```text
>   .g-recaptcha > div { 
>       margin: auto !important;
>   }
> ```

### \#3.在server端接收到後傳給google驗證

把verifyCallback拿到的值傳

```text
URL: https://www.google.com/recaptcha/api/siteverify

METHOD: POST

POST Parameter    Description
secret       (你的API secret)     Required. The shared key between your site and reCAPTCHA.
response   (點選後接到的verifycallback值)     Required. The user response token provided by reCAPTCHA, verifying the user on your site.
remoteip    (可選)Optional. The user's IP address.   ()
```

Node.js example

\(記得要用POST querystring方式傳\)

```text
  app.post('/checkHuman', (req, res) => {
    axios.post('https://www.google.com/recaptcha/api/siteverify',querystring.stringify({
      secret: '6LejmSQUAAAAAEccpZkMXhZjCwD8Z6kroMPijxmM',
      response: req.body.code 
    })).then(d => {
      console.log(d.data)
    })
  })
```

成功會回傳如下

```text
{ success: true,
  challenge_ts: '2017-06-08T08:56:12Z',
  hostname: 'localhost' }
```

