---
description: 用來處理影片或串流
---

# FFmpeg

{% embed url="https://www.ffmpeg.org/" %}

下載: [https://www.ffmpeg.org/download.html](https://www.ffmpeg.org/download.html)

## 剪輯影片

> 有其影片如果只剪輯前三秒會有誤差

```text
 ffmpeg -ss 00:01:00 -i input.mp4 -to 00:02:00 -c copy output.mp4
```

{% embed url="https://stackoverflow.com/questions/18444194/cutting-the-videos-based-on-start-and-end-time-using-ffmpeg" %}

## 合併影片

先新增一個 vidlist.txt 裡面要放合併的影片列表

```text
file 'output0.mkv'
file 'output.mkv'
```

```text
ffmpeg -f concat -i vidlist.txt -c copy out.mp4
```

[https://stackoverflow.com/a/49373401/4622645](https://stackoverflow.com/a/49373401/4622645)

