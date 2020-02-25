# HTML5 audio

可參考:[http://www.html5rocks.com/en/tutorials/getusermedia/intro/](http://www.html5rocks.com/en/tutorials/getusermedia/intro/)

audio的src可用base64當連結

而blob只能在瀏覽器當下的session有效，因為它存在瀏覽器的記憶體中

要將blob當audio的src時要轉為blob類型的src

使用`audio.src = window.URL.createObjectURL(s);`

其中s為blob物件

轉完後會類似`blob:http://localhost:8000/7ae545fe-cfac-413c-9c34-3fea70d842eb`

但這個路徑存在瀏覽器記憶體，如果要存到database要先轉為base64

## base64 to blob

參考此  
[http://stackoverflow.com/questions/16245767/creating-a-blob-from-a-base64-string-in-javascript](http://stackoverflow.com/questions/16245767/creating-a-blob-from-a-base64-string-in-javascript)

## blob to base64

參考此

[http://stackoverflow.com/questions/18650168/convert-blob-to-base64](http://stackoverflow.com/questions/18650168/convert-blob-to-base64)

## 另外 atob btob

btob為encode  把值encode為base64

而atob為decode 把 base64轉為原始檔案的二進位格式

#### 但是試很很多base64跟decode的字串都太長

所以最後還是轉為檔案存在server的硬碟內

使用formdata的方式ajax給server，一般post的方式只能傳字串，所以contentType才會用formdata，但要注意的是:

formdata不同於一般`"application/x-www-form-urlencoded")`的POST方法

formdata的POST不可指定contentType，如果指定了會收不到

Formdata Ajax範例如下

```
var oMyForm = new FormData();

oMyForm.append("file",s);

var oReq = new XMLHttpRequest();

oReq.open("POST", "/app");

oReq.send(oMyForm);
```

以下為完整的錄音然後存到express server的範例\(使用multer\)

PS:需先新建一個`uploads`資料夾於目錄，注意`var storage`的設定程式碼要放在post的路由前面

server.js

```
var express = require('express');
var bodyParser = require('body-parser');
var crypto = require('crypto');

var app = express();
var multer = require('multer');
app.set('view engine','ejs');

app.use(express.static(__dirname + '/views'));
app.use(bodyParser.urlencoded({extended: true}));


var storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, './uploads/')
  },
  filename: function (req, file, cb) {
    crypto.pseudoRandomBytes(16, function (err, raw) {
      cb(null, raw.toString('hex') + Date.now() + '.' + 'wav');
    });
  }
});




app.get('/',function(req,res){
    res.render('audio');
})
app.post('/app1',function(req,res){
    console.log(req.body);
})

app.post('/app', multer({storage: storage}).single('file'), function(req,res){

    console.log(req.file); //form files

});

app.listen('8000',function(){
    console.log('listen 8000')
})
```

audio.ejs

```
<html>
  <body>
    <audio controls autoplay></audio>
    <script type="text/javascript" src="recorder.js"> </script>

    <input onclick="startRecording()" type="button" value="start recording" />
    <input onclick="stopRecording()" type="button" value="stop recording and play" />

    <script>
      var onFail = function(e) {
        console.log('Rejected!', e);
      };

      var onSuccess = function(s) {
        var context = new AudioContext();
        var mediaStreamSource = context.createMediaStreamSource(s);
        recorder = new Recorder(mediaStreamSource);
        recorder.record();

        // audio loopback
        // mediaStreamSource.connect(context.destination);
      }

      window.URL = window.URL || window.webkitURL;
      navigator.getUserMedia  = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia;

      var recorder;
      var audio = document.querySelector('audio');

      function startRecording() {
        if (navigator.getUserMedia) {
          navigator.getUserMedia({audio: true}, onSuccess, onFail);
        } else {
          console.log('navigator.getUserMedia not present');
        }
      }

        function stopRecording() {
        recorder.stop();
        recorder.exportWAV(function(s) {


var oMyForm = new FormData();

oMyForm.append("file",s);

var oReq = new XMLHttpRequest();

oReq.open("POST", "/app");

oReq.send(oMyForm);


          audio.src = window.URL.createObjectURL(s);

              }) 

        };
    </script>

  </body>
</html>
```



