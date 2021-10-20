# Cloud storage 串接 Cloud CDN

## Cloud storage

Cloud storage 為類似 S3 的服務，串接 Cloud CDN 後可做到檔案快取。

上傳檔案後設定權限為 public 就會產生可下載的 URL

![](<../.gitbook/assets/截圖 2020-11-17 下午5.32.40.png>)



## Cloud CDN

{% embed url="https://cloud.google.com/cdn/docs/resources?hl=zh-tw" %}

需要先建立 後端 bucket 與 load balancing

![](<../.gitbook/assets/截圖 2020-11-17 下午5.03.37.png>)

然後

![](<../.gitbook/assets/截圖 2020-11-17 下午5.03.44.png>)

之後會給你一個 IP

![](<../.gitbook/assets/截圖 2020-11-17 下午5.05.26.png>)

原本 cloud storage&#x20;

```
https://storage.googleapis.com/<bucket name>/<object name>
```

改為以下即可

```
<cloud cdn ip>/<object name>
```

> 如果直接設定 http load balancing 然後有打勾 cdn 之後也會出現在 cloud cdn 看到新增東西出來，但可能會連不上去，建議還是在cloud cdn 建立

## 權限設定

cloud storage 可以設定 domain 權限，或是也可以產生 sign URL 來讓特定權限的帳戶存取[https://cloud.google.com/cdn/docs/using-signed-urls](https://cloud.google.com/cdn/docs/using-signed-urls)

## 相關文章

{% embed url="https://cloud.google.com/cdn/docs/setting-up-cdn-with-bucket" %}

[https://medium.com/techintoo/serving-static-files-using-google-cloud-cdn-storage-bucket-db1287cb5e40](https://medium.com/techintoo/serving-static-files-using-google-cloud-cdn-storage-bucket-db1287cb5e40)
