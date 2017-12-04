> linebot的api server 必須用https的，所以測試建議可以部署在heroku或是使用ngrok

不錯的文章：[http://www.oxxostudio.tw/articles/201701/line-bot.html](http://www.oxxostudio.tw/articles/201701/line-bot.html)

# 申請機器人

> 注意：一個是free方案一個是developer trial後面的這個才可以用push API （選好後無法兩者切換）

[https://business.line.me/zh-hant/services/lineat](https://business.line.me/zh-hant/services/lineat)

# 管理機器人

[https://admin-official.line.me/8417327/account/autoanswer/\#/?backUrl=%2F%3Fpage%3D1](https://admin-official.line.me/8417327/account/autoanswer/#/?backUrl=%2F%3Fpage%3D1)

> 包含把自動回覆訊息修改或關閉



# 開發者文件

https://developers.line.me/en/docs/messaging-api



## PUSH

> 如果client沒有主動送訊息給bot過，則可能會出現傳送失敗的錯誤
>
> userID可以在

```js

const accessToken = "L2MidTusPwSDFpA8dCsPohcNMkdAHnZCdt541+sqUPxY8ONMspuGqFv9Rrv6mTrBUjvTV+afZ4oOE/PKJjOiV4pfCYvjY1Bi47oOLCbFxEuW2Rk/9efdc05e0ciQirzCrfIyNZmJLrJeBSo/mQ+yLwdB04t89/1O/w1cDnyilFU=";
const channelSecret = "80f29885a979d63ec23ec629a5c67ba5";
const userId = "U678157be02a660805cb61798ce5c4f7d"


const line = require('@line/bot-sdk');

const client = new line.Client({
  channelAccessToken: accessToken
});

const message = {
  type: 'text',
  text: '測試!'
};

client.pushMessage(userId, message)
  .then(() => {
    console.log('okok')
  })
  .catch((err) => {
    console.log(err)
  });
```



