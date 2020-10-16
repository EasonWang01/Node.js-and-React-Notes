# Cloud Run

類似於 AWS lambda 的 serverless 架構。

{% embed url="https://console.cloud.google.com/run?hl=zh-tw" %}

官方教學：[https://cloud.google.com/run/docs/quickstarts/build-and-deploy\#node.js\_1](https://cloud.google.com/run/docs/quickstarts/build-and-deploy#node.js_1)

## 注意事項：

1.dockerfile 不用寫 expose port，因為寫在介面上定義即可

![](../.gitbook/assets/ying-mu-kuai-zhao-20201016-shang-wu-10.35.43.png)

2.如果是抓取 github project 記得要設定 cloud build 的專案目錄

點選以下選取專案，然後點選藍字進入 cloud build 設定

![](../.gitbook/assets/ying-mu-kuai-zhao-20201016-shang-wu-10.37.36.png)

3.記得設定 dockerfile 在專案的目錄

![](../.gitbook/assets/ying-mu-kuai-zhao-20201016-shang-wu-10.36.49.png)

