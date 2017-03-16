#影像辨識

以下為圖片上傳網站範例

https://cloud.google.com/vision/


須先點選以下建立憑證，服務帳戶金鑰

https://console.developers.google.com/apis/credentials

以下為範例程式碼


`./01.json`為金鑰檔案
```

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



#Open Source可用模組


https://github.com/desmondmorris/node-tesseract
https://github.com/tesseract-ocr/tesseract


上面的連結是下面tesseract的Node.js wrapper


範例程式如下

>options可不給，預設為english，語言縮寫可參考https://github.com/tesseract-ocr/tesseract/blob/a75ab450a8cc9a2b69cf05f5c4f7a39bc44cbacc/contrib/traineddata.txt

```
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

```
export TESSDATA_PREFIX=/usr/local/Cellar/tesseract/3.05.00/share/tessdata/
```
