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

![](../.gitbook/assets/jie-tu-20201207-xia-wu-5.03.06.png)

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

