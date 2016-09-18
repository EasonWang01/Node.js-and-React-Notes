# HTML5 Video

1.有關串流

參考此
https://github.com/EasonWang01/Node.js-stream-video/tree/master/Desktop/Node.js-stream-video-master

但其原理為使用canvas擷取影像，並使用websocket傳遞canvas資料，所以只有影像，這種做法client端用來顯示的 dataURL會持續改變，所以雖然是串流，但畫面會有閃爍問題

2.從HTML5錄製影片並下載

教學

https://developers.google.com/web/updates/2016/01/mediarecorder

Source code

https://github.com/webrtc/samples/blob/gh-pages/src/content/getusermedia/record/js/main.js

其原理為使用 `navigator.mediaDevices.getUserMedia`存取網頁攝影機後使用`new MediaRecorder`錄製

而MediaRecorder接到資料後要存入blob
```
  recorder.ondataavailable = e => {
    console.log(e);
    blob1.push(e.data);
  }
  //這裡記得要推入的是e.data
```
之後再把blob轉格式

```var superBuffer = new Blob(blob1, {type: 'video/webm'});```

最後轉為可用在url的型態

```
window.URL.createObjectURL(superBuffer)
```
把他放到video的src即可


3.後來想到可以使用將影片擷取10秒一格並分開連續傳送給client達到串流的效果