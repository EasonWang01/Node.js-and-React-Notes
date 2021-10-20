# Twilio

美國上市公司，提供 SMS 與 Voice phone call服務，你可以在上面申請實體美國電話號碼。

## &#x20;建立 Trial account

在 twilio 建立帳號後綁定一個你的手機號碼之後，可以申請一個trial number

[https://www.twilio.com/docs/usage/tutorials/how-to-use-your-free-trial-account#trial-account-restrictions-and-limitations](https://www.twilio.com/docs/usage/tutorials/how-to-use-your-free-trial-account#trial-account-restrictions-and-limitations)

> If you don’t use your phone number for more than 30 days, we’ll remove it from your trial project. If you decide to return, you can always pick a new phone number.
>
> 30 天內沒使用trial account 內的號碼會被清除

之後你可以用 trial number 發送簡訊或打電話(使用API)，或是你也可以發送簡訊到這個number，可以從console裡面去查看log。

![](<../.gitbook/assets/螢幕快照 2020-04-17 下午12.10.19.png>)



## 連結

voice console: [https://www.twilio.com/console/voice/dashboard](https://www.twilio.com/console/voice/dashboard)

sms doc: [https://www.twilio.com/docs/sms](https://www.twilio.com/docs/sms)



## 使用

#### 取得 access token

![](<../.gitbook/assets/螢幕快照 2020-04-17 下午12.13.55.png>)

#### 撥打電話

curl

```
curl -X POST https://api.twilio.com/2010-04-01/Accounts/AC1ef3aaf7dd7bfd622fd53b8c2bd5b8de/Calls.json \
--data-urlencode "Url=http://demo.twilio.com/docs/voice.xml" \
--data-urlencode "To=+886911928157" \
--data-urlencode "From=+14157411853" \
-u AC1ef3aaf7dd7bfd622fd53b8c2bd5b8de:<your_auth_token>
```

#### 發送簡訊

curl

```
EXCLAMATION_MARK='!'
curl -X POST https://api.twilio.com/2010-04-01/Accounts/AC1ef3aaf7dd7bfd622fd53b8c2bd5b8de/Messages.json \
--data-urlencode "Body=Hi there$EXCLAMATION_MARK" \
--data-urlencode "From=+14157411853" \
--data-urlencode "To=+886911928157" \
-u AC1ef3aaf7dd7bfd622fd53b8c2bd5b8de:<your_auth_token>
```
