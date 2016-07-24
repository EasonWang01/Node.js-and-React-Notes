# HTML5 audio

可參考:http://www.html5rocks.com/en/tutorials/getusermedia/intro/

audio的src可用base64當連結

而blob只能在瀏覽器當下的session有效，因為它存在瀏覽器的記憶體中

所以要將blob要當audio的src要轉為blob類型的src

使用` audio.src = window.URL.createObjectURL(s);`

其中s為blob物件

轉完後會類似`blob:http://localhost:8000/7ae545fe-cfac-413c-9c34-3fea70d842eb`

但這個路徑存在瀏覽器記憶體，如果要存到database要先轉為base64

##base64 to blob

參考此
http://stackoverflow.com/questions/16245767/creating-a-blob-from-a-base64-string-in-javascript

##blob to base64

參考此

http://stackoverflow.com/questions/18650168/convert-blob-to-base64


##另外 atob btob

btob為encode  把值encode為base64

而atob為decode 把 值還原

####但是試很很多base64跟decode的字串都太長

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






