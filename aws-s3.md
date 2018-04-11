1. 申請IAM，取得access key 和 secret key
2. 下載aws-sdk

# 上傳到 S3 範例

> 以下程式開啟後會先當一個範例上傳網站，然後可以在此上傳檔案，之後會傳到S3，使用的上傳檔案模組為busboy

```js
var http = require('http'),
    path = require('path'),
    os = require('os'),
    fs = require('fs');

var Busboy = require('busboy');
const AWS = require('aws-sdk');

// 填入已經創建的S3 bucket名稱
const BUCKET_NAME = 'marketplace-model';

// 從IAM獲得
const IAM_USER_KEY = 'AKIAIV4LYK7OX6MZGGTQ';
const IAM_USER_SECRET = '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';

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



