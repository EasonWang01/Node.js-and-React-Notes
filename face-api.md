# Face API

先到此處申請服務：[https://portal.azure.com/\#create/Microsoft.CognitiveServicesFace](https://portal.azure.com/#create/Microsoft.CognitiveServicesFace)![](/assets/螢幕快照 2018-10-13 下午1.31.31.png)

然後取得存取金鑰：

![](/assets/螢幕快照 2018-10-13 下午4.52.38.png)



之後可以到此網站線上測試此API:

[https://eastasia.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236/console](https://eastasia.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236/console)

填入相關參數與圖片的URL即可：

> returnFaceAttributes 裡面填入想辨識的資料，使用逗點分隔，前面不可有空白。

![](/assets/螢幕快照 2018-10-13 下午4.47.03.png)

下面為回傳的資訊：

![](/assets/螢幕快照 2018-10-13 下午4.47.13.png)

## 也可以使用程式的方法來互叫

```js
const https = require("https");

const param =
  "?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=emotion,age,gender,exposure,headPose,hair,makeup,accessories";

var options = {
  host: "eastasia.api.cognitive.microsoft.com",
  port: 443,
  path: `/face/v1.0/detect${param}`,
  method: "POST",
  headers: {
    "Ocp-Apim-Subscription-Key": "42a6efee6bec415a967ff73c5e89bae3"
  }
};

const req = https.request(options, res => {
  res.on("data", function(data) {
    console.log(data.toString());
  });
});

req.on("error", e => {
  console.error(e);
});

req.write(
  JSON.stringify({
    url: "https://i.ytimg.com/vi/Lx5j_QhUULM/maxresdefault.jpg"
  })
);
req.end();
```



