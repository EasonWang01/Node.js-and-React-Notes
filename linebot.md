> linebot的api server 必須用https的，所以測試建議可以部署在heroku或是使用ngrok

不錯的文章：[http://www.oxxostudio.tw/articles/201701/line-bot.html](http://www.oxxostudio.tw/articles/201701/line-bot.html)

[https://goo.gl/ePtTRw](https://goo.gl/ePtTRw)

# 申請機器人

> 注意：一個是free方案一個是developer trial後面的這個才可以用push API （選好後無法兩者切換）

[https://business.line.me/zh-hant/services/lineat](https://business.line.me/zh-hant/services/lineat)

# 管理機器人

[https://admin-official.line.me/8417327/account/autoanswer/\#/?backUrl=%2F%3Fpage%3D1](https://admin-official.line.me/8417327/account/autoanswer/#/?backUrl=%2F%3Fpage%3D1)

> 包含把自動回覆訊息修改或關閉

# 開發者設定

[https://developers.line.me/console/channel/1550169976/basic/](https://developers.line.me/console/channel/1550169976/basic/)

# 開發者文件

[https://developers.line.me/en/docs/messaging-api](https://developers.line.me/en/docs/messaging-api)

[https://developers.line.me/en/docs/messaging-api/reference/](https://developers.line.me/en/docs/messaging-api/reference/)

# \# API 範例

## WebHook

> 先在developer.line.me設定

![](/assets/螢幕快照 2017-12-04 下午10.38.13.png)

```js
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
app.use(bodyParser.json());

app.post('/webhook/', (req, res, next) => {
  // get content from request body
  console.log(req.body.events[0].source)
  console.log(req.body.events[0].message)
})

app.listen(process.env.PORT || 3000, () => {
  console.log('Listening on port 3000!')
})
```

## PUSH

> 如果client沒有主動送訊息給bot過，則可能會出現傳送失敗的錯誤
>
> userID可以在開發者設定網頁最下方看到

```js
const accessToken = "L1MidTusPwSDFpA8dCsPohcNMkdAHnZCdt541+sqUPxY8ONMspuGqFv9Rrv6mTrBUjvTV+afZ4oOE/PKJjOiV4pfCYvjY1Bi47oOLCbFxEuW2Rk/9efdc05e0ciQirzCrfIyNZmJLrJeBSo/mQ+yLwdB04t89/1O/w1cDnyilFU=";
const channelSecret = "80f29885a979d63ec23ec629a5c67ba3";
const userId = "U678157be02a660805cb61798ce5c4f2d"


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

# 回覆訊息

```js
const line = require('@line/bot-sdk');
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
app.use(bodyParser.json());


const client = new line.Client({
  channelAccessToken: 'LMidTusPwSDFpA8dCsPohcNMkdAHnZCdt541+sqUPxY8ONMspuGqFv9Rrv6mTrBUjvTV+afZ4oOE/PKJjOiV4pfCYvjY1Bi47oOLCbFxEuW2Rk/9efdc05e0ciQirzCrfIyNZmJLrJeBSo/mQ+yLwdB04t89/1O/w1cDnyilFU='
});

app.post('/webhook/', (req, res, next) => {
  console.log(req.body)
  // get content from request body
  console.log(req.body.events[0].source)
  console.log(req.body.events[0].message)
  client.replyMessage(req.body.events[0].replyToken, {
    type: 'text',
    text: '您好，請稍等..'
  })
    .then(() => {
      console.log('message replied!')
    })
    .catch((err) => {
      console.log(err)
    });

})

app.listen(process.env.PORT || 3000, () => {
  console.log('Listening on port 3000!')
})
```

# 回覆圖卡訊息

```js

const line = require('@line/bot-sdk');
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
app.use(bodyParser.json());


const client = new line.Client({
  channelAccessToken: 'LMidTusPwSDFpA8dCsPohcNMkdAHnZCdt541+sqUPxY8ONMspuGqFv9Rrv6mTrBUjvTV+afZ4oOE/PKJjOiV4pfCYvjY1Bi47oOLCbFxEuW2Rk/9efdc05e0ciQirzCrfIyNZmJLrJeBSo/mQ+yLwdB04t89/1O/w1cDnyilFU='
});

app.post('/webhook/', (req, res, next) => {
  if(req.body.events[0].type === "postback") {
    postBackLogic(req)
    return
  }
  client.replyMessage(req.body.events[0].replyToken, msg)
    .then(() => {
      console.log('message card replied!')
    })
    .catch((err) => {
      console.log(err)
    });

})

app.listen(process.env.PORT || 3000, () => {
  console.log('Listening on port 3000!')
})



const msg = {
  "type": "template",
  "altText": "請選擇查詢項目",
  "template": {
      "type": "buttons",
      "thumbnailImageUrl": "https://i.ytimg.com/vi/b1-Fj-C4OmE/maxresdefault.jpg",
      "imageAspectRatio": "rectangle",
      "imageSize": "cover",
      "imageBackgroundColor": "#FFFFFF",
      "title": "加密星球",
      "text": "請選擇查詢項目",
      "actions": [
          {
            "type": "postback",
            "label": "Buy",
            "data": JSON.stringify({
              action: "bitcoin"
            }),
          },
          {
            "type": "postback",
            "label": "Add to cart",
            "data": "action=add&itemid=123"
          },
          {
            "type": "uri",
            "label": "View detail",
            "uri": "http://example.com/page/123"
          }
      ]
  }
}


const postBackLogic = (req) => {
  console.log(req.body.events[0].source)
  console.log(JSON.parse(req.body.events[0].postback.data).action)
};
```



