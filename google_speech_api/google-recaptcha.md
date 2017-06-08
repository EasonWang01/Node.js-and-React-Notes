# 簡介:給人類方便給bot困難

\(類似於csrf token的功能\)

# \#1.  安裝

[https://www.google.com/recaptcha](https://www.google.com/recaptcha)

如果用react.js可以使用[https://github.com/appleboy/react-recaptcha](https://github.com/appleboy/react-recaptcha模組)克服dom沒出現但recaptcha執行造成無法顯示的情況



# \#2.使用

文件:https://developers.google.com/recaptcha/docs/verify

[https://github.c](https://www.gitbook.com/book/easonwang01/web_advance/edit#)使用套件的範例

```
<Recaptcha
  verifyCallback={(r) => console.log(r)}
  theme="light"
  sitekey="6LejmSQUAAAAACsss6nydz-73jHrJBqlH5xa2WR0"
  onloadCallback={() => { }}
/>
```



> 將recapcha置中
>
> ```
>   .g-recaptcha > div { 
>       margin: auto !important;
>   }
> ```



