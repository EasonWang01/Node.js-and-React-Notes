# vision api

## 影像辨識

以下為圖片上傳網站範例

[https://cloud.google.com/vision/](https://cloud.google.com/vision/)

須先點選以下建立憑證，服務帳戶金鑰

[https://console.developers.google.com/apis/credentials](https://console.developers.google.com/apis/credentials)

以下為範例程式碼

`./01.json`為金鑰檔案

```text
const Vision = require('@google-cloud/vision');

// Instantiates a client
const vision = Vision({
  languageHints: [ "en", "zh-TW", "zh-CN" ],  //要辨識的語言
  keyFilename:'./01.json'
});

// The path to the local image file, e.g. "/path/to/image.png"
// const fileName = '/path/to/image.png';

// Performs text detection on the local file
vision.detectText('./0.jpg')
  .then((results) => {
    const detections = results[0];

    console.log('Text:');
    detections.forEach((text) => console.log(text));
});
```

## Open Source可用模組

[https://github.com/desmondmorris/node-tesseract](https://github.com/desmondmorris/node-tesseract) [https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract)

上面的連結是下面tesseract的Node.js wrapper

範例程式如下

> options可不給，預設為english，語言縮寫可參考[https://github.com/tesseract-ocr/tesseract/blob/a75ab450a8cc9a2b69cf05f5c4f7a39bc44cbacc/contrib/traineddata.txt](https://github.com/tesseract-ocr/tesseract/blob/a75ab450a8cc9a2b69cf05f5c4f7a39bc44cbacc/contrib/traineddata.txt)

```text
var tesseract = require('node-tesseract');


var options = {
    l: 'chi_tra',
    psm: 6,
    binary: '/usr/local/bin/tesseract'
};

// Recognize text of any language in any format
tesseract.process(__dirname + '/022.png', options,function(err, text) {
    if(err) {
        console.error(err);
    } else {
        console.log(text);
    }
});
```

如說要設路徑可參考

```text
export TESSDATA_PREFIX=/usr/local/Cellar/tesseract/3.05.00/share/tessdata/
```

> 注意:他辨識中文需花比較長時間，且識別度不高

完整版相關gist

[https://gist.github.com/EasonWang01/db8eae7202b6e7dde790c2ff7e8fb810](https://gist.github.com/EasonWang01/db8eae7202b6e7dde790c2ff7e8fb810)

## \#從browser發出request

文章 [http://terrenceryan.com/blog/index.php/working-with-cloud-vision-api-from-javascript/](http://terrenceryan.com/blog/index.php/working-with-cloud-vision-api-from-javascript/)

完整範例 [https://github.com/GoogleCloudPlatform/web-docs-samples/tree/master/vision/explore-api](https://github.com/GoogleCloudPlatform/web-docs-samples/tree/master/vision/explore-api)

官方文件 [https://cloud.google.com/vision/docs/detecting-text\#vision-text-detection-protocol](https://cloud.google.com/vision/docs/detecting-text#vision-text-detection-protocol)

> 注意，官方文件images應該為image

```text
payload =  {
              requests: [
                {
                  image: {
                    "content": base64Data.replace("data:image/jpeg;base64,", "")
                  },
                  features: [
                    {
                      type: "TEXT_DETECTION"
                    }
                  ]
                }
              ]
            }

  axios.post('https://vision.googleapis.com/v1/images:annotate?key=AIzaSyAp9SJv4H5ibESc4aFG28SBdP51jNqOQoA',
    JSON.stringify(payload),
    {
      headers: {
        "Content-Type": "application/json",
        "Content-Length": base64Data.replace("data:image/jpeg;base64,", "").length
      }
    }
  )
  .then(response => {
    console.log(util.inspect(response.data, {depth: null}));
  })
  .catch(err => {
     console.log(util.inspect(err, {depth: null}));
  })
```

