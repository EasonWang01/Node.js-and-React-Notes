# HTML5 Video

1.有關串流

參考此  
[https://github.com/EasonWang01/Node.js-stream-video/tree/master/Desktop/Node.js-stream-video-master](https://github.com/EasonWang01/Node.js-stream-video/tree/master/Desktop/Node.js-stream-video-master)

但其原理為使用canvas擷取影像，並使用websocket傳遞canvas資料，所以只有影像，這種做法client端用來顯示的 dataURL會持續改變，所以雖然是串流，但畫面會有閃爍問題

2.從HTML5錄製影片並下載

教學

[https://developers.google.com/web/updates/2016/01/mediarecorder](https://developers.google.com/web/updates/2016/01/mediarecorder)

Source code

[https://github.com/webrtc/samples/blob/gh-pages/src/content/getusermedia/record/js/main.js](https://github.com/webrtc/samples/blob/gh-pages/src/content/getusermedia/record/js/main.js)

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

`var superBuffer = new Blob(blob1, {type: 'video/webm'});`

最後轉為可用在url的型態

```
window.URL.createObjectURL(superBuffer)
```

把他放到video的src即可

3.後來想到可以使用將影片擷取10秒一格並分開連續傳送給client達到串流的效果

# WebRTC串流

![](/assets/螢幕快照 2018-07-20 下午3.14.32.png)

# 名詞:

[https://blog.mozilla.com.tw/posts/3261/webrtc-相關縮寫名詞簡介](https://blog.mozilla.com.tw/posts/3261/webrtc-相關縮寫名詞簡介)

# 範例:

[https://shanetully.com/2014/09/a-dead-simple-webrtc-example/](https://shanetully.com/2014/09/a-dead-simple-webrtc-example/)

[https://github.com/shanet/WebRTC-Example](https://github.com/shanet/WebRTC-Example)

以上兩個為很好且簡單的範例，上面是文章下面是程式碼。

Client 過程

```
1. 初始化連線: new RTCPeerConnection，並加入本地影像 peerConnection.addStream(localStream);

2. 設定SDP: peerConnection.setLocalDescription

3. 在本地蒐集到ice後傳送給另一個peer: onicecandidate 並且傳送 serverConnection.send
(ICE candidate可能接收到多個)

4. 另一個peer接到ice後把ice加入： peerConnection.addIceCandidate

5. 接收到視訊：peerConnection.ontrack = gotRemoteStream

6. 顯示遠端視訊： remoteVideo.srcObject = event.streams[0]
```

Server

