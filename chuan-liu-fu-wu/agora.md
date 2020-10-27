---
description: 一個提供第三方串流服務的供應商
---

# Agora

### 文件：

[https://docs.agora.io/](https://docs.agora.io/)

### 收費方式：

[https://docs.agora.io/cn/Video/billing\_rtc?platform=All%20Platforms](https://docs.agora.io/cn/Video/billing_rtc?platform=All%20Platforms)

### Web API: 

[https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html)

### 範例：

[https://github.com/AgoraIO/Basic-Video-Call](https://github.com/AgoraIO/Basic-Video-Call)

建立帳戶後去取得相關開發金鑰：

![](../.gitbook/assets/jie-tu-20201027-xia-wu-3.04.43.png)

簡單一對一視訊範例：

```javascript
import React, { useState, useEffect } from "react";
import StreamPlayer from "agora-stream-player";
import agora from "agora-rtc-sdk";
import enhanceAgoraRTC from "agoran-awe";

const AgoraRTC = enhanceAgoraRTC(agora); // 提供原本函數使用 async 功能

const streamID = null; // 不提供 streamId 他會隨機產生，如果提供相同 streamId會視為同一個client
function App() {
  const [localStream, setLocalStream] = useState("");
  const [remoteStreams, setRemoteStreams] = useState("");
  useEffect(() => {
    const init = async () => {
      const stream = AgoraRTC.createStream({
        streamID,
        video: true,
        audio: true,
      });
      await stream.init();
      // 記得要先創建 stream 再創 client 不然會有不顯示其他 stream 問題
      const client = AgoraRTC.createClient({ mode: "rtc", codec: "h264" });
      await client.init("輸入 appID");
      await client.join(
        "輸入token",
        "輸入 channel",
        streamID
      );

      // Stream 加入後 必須要訂閱
      client.on("stream-added", function (evt) {        
        var remoteStream = evt.stream;
        var id = remoteStream.getId();
        client.subscribe(remoteStream, function (err) {
          console.log("stream subscribe failed", err);
        });
        console.log("stream-added remote-uid: ", id);
      });
      client.on("stream-subscribed", function (evt) {
        var remoteStream = evt.stream;
        var id = remoteStream.getId();
        setRemoteStreams(remoteStream);
        console.log("stream-subscribed remote-uid: ", id);
      });

      await client.publish(stream);
      setLocalStream(stream);
    };
    init();
  }, []);
  return (
    <>
      {localStream && (
        <StreamPlayer
          style={{ width: "50px", height: "50px" }}
          key={1023}
          video={true}
          audio={true}
          stream={localStream}
          fit="contain"
        />
      )}
      {remoteStreams && (
        <StreamPlayer
          style={{ width: "50px", height: "50px" }}
          key={1024}
          video={true}
          audio={true}
          stream={remoteStreams}
          fit="contain"
        />
      )}
    </>
  );
}

export default App;
```

