---
description: 可用來操作 Android 手機的 terminal 工具
---

# adb

#### 選擇設備

```text
adb -s 53F0219321001531 <指令>
```

#### 刪除設備上 APP

```text
adb uninstall <套件名稱 com.>
```

#### 列出目前連接之設備

```text
adb devices
```

#### 指定 react native連接設備

```text
npx react-native run-android --variant=release --deviceId=<設備 ID>
```

#### 如何開啟 adb

需要先開啟 USB 偵錯 debugging

[https://www.syncios.com/android/how-to-debug-huawei-p30-and-p30-pro.html](https://www.syncios.com/android/how-to-debug-huawei-p30-and-p30-pro.html)

