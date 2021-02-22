# WebRTC 進階

## 串流中關閉與開啟視訊

```javascript
  let localVideoEnabled = true;
  let localAudioEnabled = true;
  
  function handleVideoDisconnect() {
    localVideoEnabled = !localVideoEnabled;
    localStream.getTracks()[1].enabled = localVideoEnabled;
  }
  
  function handleAudioDisconnect() {
    localAudioEnabled = !localAudioEnabled;
    localStream.getTracks()[0].enabled = localAudioEnabled;
  }
  
  // 如果用 track.stop() 會永久關閉
```

> 可選擇關音訊或視訊。

![](../../.gitbook/assets/jie-tu-20201207-xia-wu-5.03.06.png)

## 控制斷線重連

首先斷線時發送 websocket 訊息給對方，並且關閉 peerConnection，然後清空 video srcObject。

```javascript
  function handleWebRTCDisconnect() {
    serverConnection.send(JSON.stringify({ event: "peer disconnect", uuid }));
    peerConnection.close();
    localStream = "";
    localVideo.current.srcObject = null; // 這邊用 React ref 所以是 .current
    remoteVideo.current.srcObject = null;
    init();
  }
```

對方接收到訊息後

```javascript
  function gotMessageFromServer(message) {
    var signal = JSON.parse(message.data);

    if (signal.event === "peer disconnect") {
      console.log("peer disconnect");
      peerConnection.close();
      remoteVideo.current.srcObject = null;
      init();
      return;
    }
    ....
   } 
```

`init` function 如下，即為初始化連線與獲取 media 等設備

```javascript
const init = () => {
    peerConnection = new RTCPeerConnection(peerConnectionConfig);
    const constraints = {
      video: true,
      audio: true,
    };
    serverConnection.onmessage = gotMessageFromServer;
    if (navigator.mediaDevices.getUserMedia) {
      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          localStream = stream;
          localVideo.current.srcObject = stream;
          peerConnection.ontrack = (event) => {
            console.log("got remote stream");
            if (!remoteVideo.current.srcObject) {
              remoteVideo.current.srcObject = event.streams[0];
              !isCaller && peerConnection.addStream(localStream);
            }
          };
        })
        .catch(errorHandler);
    } else {
      alert("Your browser does not support getUserMedia API");
    }
  };
```

## Data channel 傳送訊息

1. 建立 channel

```javascript
  let sendChannel;
  let receiveChannel;
  
  const handleConnect = (_isCaller) => {
    if (_isCaller) {
      ....
      sendChannel = peerConnection.createDataChannel("sendDataChannel");

      sendChannel.onopen = (e) => {
        console.log('send channel open')
      }
      
      sendChannel.onmessage = onChannelMessage;
      peerConnection.addStream(localStream);
      peerConnection.createOffer().then(createdDescription).catch(errorHandler);
      ....
    }
  }    
  const onChannelMessage = (e) => {
    console.log(`on channel message: ${e.data}`)
  }  
```

> 只用建立一個 channel 即可，撥打方建立，然後 createOffer 後會把 channel 送給遠端

2. 之後監聽接收 channel

```javascript
    peerConnection.ondatachannel = (e) => {
      receiveChannel = e.channel;
      receiveChannel.onmessage = onChannelMessage;
    }
```

3. 發送訊息到 channel

```javascript
  const handleSendMessage = (value) => {
    if(receiveChannel) {
      receiveChannel.send(value);
    } else {
      sendChannel.send(value);
    }
  }
```

可參考：[https://webrtc.github.io/samples/src/content/datachannel/basic/js/main.js](https://webrtc.github.io/samples/src/content/datachannel/basic/js/main.js)

## ShareScreen

### 獲取畫面並重新設定 addStream 然後傳給對方

重新發起 createOffer

```javascript
async function handleShareScreen() {
    try {
      captureStream = await navigator.mediaDevices.getDisplayMedia({
        video: true,
        audio: true,
      });
      captureStream.getVideoTracks()[0].onended = () => { // Click on browser UI stop sharing button
        console.info("ScreenShare has ended");
        // Switch to camera stream
        localVideo.current.srcObject = localStream;
        peerConnection.removeStream(captureStream);
        peerConnection.addStream(localStream);
        peerConnection.createOffer().then(sendDescription).catch(errorHandler);
      };
    } catch (err) {
      console.error("Error: " + err);
    }
    // Switch to shareScreen stream
    localVideo.current.srcObject = captureStream;
    peerConnection.removeStream(localStream);
    peerConnection.addStream(captureStream);
    peerConnection.createOffer().then(sendDescription).catch(errorHandler);
  }
  
  function sendDescription(description) {
    console.log("got description");
    console.log(description);
    peerConnection
      .setLocalDescription(description)
      .then(function () {
        serverConnection.send(
          JSON.stringify({ sdp: peerConnection.localDescription, uuid })
        );
      })
      .catch(errorHandler);
  }
```

