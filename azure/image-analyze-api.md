# Image Analyze API

這篇文章要使用 Azure Computer Vision API 來對圖片進行相關的影像分析。

## 1.申請服務

先到以下網址申請服務。

[https://portal.azure.com/\#create/Microsoft.CognitiveServicesComputerVision](https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)

選擇相關方案：  
![https://ithelp.ithome.com.tw/upload/images/20181017/20112426hRVqY1m5CR.png](https://ithelp.ithome.com.tw/upload/images/20181017/20112426hRVqY1m5CR.png)

## 2.取得金鑰

![https://ithelp.ithome.com.tw/upload/images/20181017/20112426sMLUZYtBK0.png](https://ithelp.ithome.com.tw/upload/images/20181017/20112426sMLUZYtBK0.png)

## 3.使用API

可以先到以下網站：  
[https://eastasia.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa/console](https://eastasia.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa/console)

看到其提供的 API 種類：  
![https://ithelp.ithome.com.tw/upload/images/20181017/201124260mulQBPiz8.png](https://ithelp.ithome.com.tw/upload/images/20181017/201124260mulQBPiz8.png)

## Analyze Image API

接著我們先使用第一個 Analyze Image API

1.先填上請求參數：  
![https://ithelp.ithome.com.tw/upload/images/20181017/20112426a67XVarT52.png](https://ithelp.ithome.com.tw/upload/images/20181017/20112426a67XVarT52.png)

visualFeatures 可分析的特徵有如下：

```text
Categories
Tags
Description
Faces
ImageType
Color
Adult
```

2.放上要分析的圖片URL：  
![https://ithelp.ithome.com.tw/upload/images/20181017/20112426YNb9Rycyu2.png](https://ithelp.ithome.com.tw/upload/images/20181017/20112426YNb9Rycyu2.png)

![https://desk-fd.zol-img.com.cn/t\_s960x600c5/g5/M00/0B/05/ChMkJ1aCHsGIF\_JuABN-S6sntcUAAGskAMO8d4AE35j534.jpg](https://desk-fd.zol-img.com.cn/t_s960x600c5/g5/M00/0B/05/ChMkJ1aCHsGIF_JuABN-S6sntcUAAGskAMO8d4AE35j534.jpg)

3.按下送出後即會回傳相關圖片分析後的數據：

![https://ithelp.ithome.com.tw/upload/images/20181017/20112426Rd4ptSjc6s.png](https://ithelp.ithome.com.tw/upload/images/20181017/20112426Rd4ptSjc6s.png)

程式範例如下：

```javascript
const https = require("https");

const param =
  "?visualFeatures=Categories,Faces,Tags,Description,ImageType,Color,Adult&language=en";

const options = {
  host: "eastasia.api.cognitive.microsoft.com",
  port: 443,
  path: `/vision/v1.0/analyze${param}`,
  method: "POST",
  headers: {
    "Ocp-Apim-Subscription-Key": "填上金鑰"
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
    url: "https://desk-fd.zol-img.com.cn/t_s960x600c5/g5/M00/0B/05/ChMkJ1aCHsGIF_JuABN-S6sntcUAAGskAMO8d4AE35j534.jpg"})
);
req.end();
```

