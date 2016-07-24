# HTML5 audio

可參考:http://www.html5rocks.com/en/tutorials/getusermedia/intro/

audio的src可用base64當連結

而blob只能在瀏覽器當下的session有效，因為它存在瀏覽器的記憶體中

所以要將blob要當audio的src要轉為blob類型的src

使用` audio.src = window.URL.createObjectURL(s);`

其中s為blob物件

轉完後會類似`blob:http://localhost:8000/7ae545fe-cfac-413c-9c34-3fea70d842eb`

但這個路徑存在瀏覽器記憶體，如果要存到database要先轉為base64






