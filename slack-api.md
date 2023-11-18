# Slack API

## 讓 Bot 傳送訊息到 Private Channel

首先加入權限

<figure><img src=".gitbook/assets/截圖 2023-11-18 下午6.46.15.png" alt=""><figcaption></figcaption></figure>

之後 install 到 workplace 後會產生 OAuth token

<figure><img src=".gitbook/assets/截圖 2023-11-18 下午6.47.08.png" alt=""><figcaption></figcaption></figure>

之後要把 Bot 手動加入到 Private channel 中 (點選新增應用程式)

<figure><img src=".gitbook/assets/截圖 2023-11-18 下午6.41.49.png" alt=""><figcaption></figcaption></figure>

之後在 channel 資訊左下方找到 channel ID，並且紀錄下剛才的 Bot User OAuth Token

執行以下程式：

```javascript
const { WebClient } = require("@slack/web-api");

const options = {};
const web = new WebClient(
  "Bot User OAuth Token",
  options
);

const sendSlackMessage = async (message, channel = null) => {
  const channelId = "Channel Id";

  return new Promise(async (resolve, reject) => {
    try {
      const resp = await web.chat.postMessage({
        text: message,
        channel: channelId,
      });
      return resolve(true);
    } catch (error) {
      return resolve(true);
    }
  });
};

const sendMessage = async (message) => {
  const resp = await sendSlackMessage(message);
  console.log(resp); // true
};
sendMessage("Test App 123");
```
