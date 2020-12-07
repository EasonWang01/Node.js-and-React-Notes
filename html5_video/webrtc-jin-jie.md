# WebRTC 進階

## 串流中關閉與開啟視訊

```javascript
  let localVideoEnabled = false;
  
  function handleDisconnect() {
    localVideoEnabled = !localVideoEnabled;
    localStream.getTracks().forEach((track) => track.enabled = localVideoEnabled);
  }
  
  // 如果用 track.stop() 會永久關閉
```

