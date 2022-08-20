# AWS S3

1. 申請IAM，取得access key 和 secret key
2. 下載 aws-sdk

官方文件：

[https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html)

## S3 公開存取

S3 有 host 靜態網站功能，但必須開啟公開讀取權限。

`許可 -> 儲存貯體政策`，增加如下設置

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::<更改為你的 Bucket-Name>/*"
            ]
        }
    ]
}
```

取消勾選封鎖存取

![](<.gitbook/assets/截圖 2022-08-20 下午1.56.01.png>)

[https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html#bucket-policy-static-site](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html#bucket-policy-static-site)

## 上傳到 S3 範例

> 以下程式開啟後會先當一個範例上傳網站，然後可以在此上傳檔案，之後會傳到S3，使用的上傳檔案模組為busboy

```javascript
const http = require('http'),
    path = require('path'),
    os = require('os'),
    fs = require('fs');

var Busboy = require('busboy');
const AWS = require('aws-sdk');

// 填入已經創建的S3 bucket名稱
const BUCKET_NAME = 'marketplace-model';

// 從IAM獲得
const IAM_USER_KEY = 'key';
const IAM_USER_SECRET = 'secret';

function uploadToS3(file, filename) {
 let s3bucket = new AWS.S3({
   accessKeyId: IAM_USER_KEY,
   secretAccessKey: IAM_USER_SECRET,
   Bucket: BUCKET_NAME,
 });
 s3bucket.createBucket(function () {
   var params = {
    Bucket: BUCKET_NAME,
    Key: filename,
    Body: file,
    ACL: "public-read"
   };
   s3bucket.upload(params, function (err, data) {
    if (err) {
     console.log('error in callback');
     console.log(err);
    }
    console.log('success');
    console.log(data);
   });
 });
}


http.createServer(function(req, res) {
    if (req.method === 'POST') {
    var busboy = new Busboy({ headers: req.headers });
    busboy.on('field', function(fieldname, val, fieldnameTruncated, valTruncated, encoding, mimetype) {
      folder_name = val
    });
    busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
      // busboy讀完的是stream，需要轉為buffer然後上傳到S3
      var bufs = [];
      file.on('data', function(data) {
        bufs.push(data);
      });
      file.on('end', function() {
        var buf = Buffer.concat(bufs);
        uploadToS3(buf, filename);
      });
    });
    busboy.on('finish', function() {
      res.writeHead(200, { 'Connection': 'close' });
      res.end("That's all folks!");
    });
    return req.pipe(busboy);
  } else if (req.method === 'GET') {
    res.writeHead(200, { Connection: 'close' });
    res.end('<html><head></head><body>\
               <form method="POST" enctype="multipart/form-data">\
                <input type="text" name="textfield"><br />\
                <input type="file" name="filefield"><br />\
                <input type="submit">\
              </form>\
            </body></html>');
  }
}).listen(8000, function() {
  console.log('Listening for requests');
});
```

> 在createBucket的params的參數key前面加上路徑/test/test.png 則會自動在S3新增test資料夾

> 如果是一些中文檔案名稱記得用 encodeURI 再 putObject 然後下載時也是

## 開啟 S3 CORS

到下圖中設定

![](<.gitbook/assets/截圖 2022-04-16 下午9.56.07.png>)

加上類似如下

```
<AllowedMethod>HEAD</AllowedMethod>
```

完整版為：

```
<!-- Sample policy -->
<CORSConfiguration>
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>HEAD</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>Authorization</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```
