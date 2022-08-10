# AWS Cloudfront、ELB

## 部署到AWS

```
記得開security group 來開port
```

[https://www.youtube.com/watch?v=WxhFq64FQzA](https://www.youtube.com/watch?v=WxhFq64FQzA)

### 上傳檔案

如果沒辦法用SFTP出現permission deny須先把該EC2上的資料夾使用`sudo chmod 777 資料夾名稱`來開啟權限即可

GUI部分，在`window`可用WINSCP或FileZillz在`Mac`可用CyberDuck

[http://stackoverflow.com/questions/20939562/scp-permission-denied-publickey-on-ec2-only-when-using-r-flag-on-directories](http://stackoverflow.com/questions/20939562/scp-permission-denied-publickey-on-ec2-only-when-using-r-flag-on-directories)

### 注意事項

1.如果上傳檔案時沒有權限，但你覺得一個一個資料夾開權限太麻煩，於是在usr之類的大資料夾整個開權限後會出現整個sudo出現錯誤

```
sudo: /usr/bin/sudo must be owned by uid 0 and have the setuid bit set
```

原因：：\
[http://stackoverflow.com/questions/16682297/getting-message-sudo-must-be-setuid-root-but-sudo-is-already-owned-by-root/19306929#19306929](http://stackoverflow.com/questions/16682297/getting-message-sudo-must-be-setuid-root-but-sudo-is-already-owned-by-root/19306929#19306929)\
目前還沒找到解法，網路上目前推薦重灌系統

2.Elastic IP分配後如果把該機器刪掉，之後自開一台把同個Elastic IP分配，可能會產生錯誤，所以建議開新機器並分配新的Elastic IP

3.如出現 以下可輸入`sudo chmod 400 ~/Downloads/Trading-Platform.pem`，並且ssh不要輸入sudo

> Load key "/Users/eason.wang/Downloads/Trading-Platform.pem": bad permissions\
> Permission denied (publickey).

## 其他AWS服務說明

> 可參考以下不錯文章

{% embed url="http://ch-tseng.blogspot.tw/2015/04/amazon-aws.html?m=1" %}

## AWS cloudfront 串接 S3

創建好 S3 後上傳檔案， bucket 先設為 private，且不用開啟靜態 host，之後設置 cloudfront 後使用OAI 讓他更改 bucket policy

![](<.gitbook/assets/截圖 2022-08-10 下午1.42.34.png>)



## AWS load balancer

要先創建 Target group 且連結到 EC2，並且確認 health check 正常：

![](<.gitbook/assets/截圖 2022-08-09 上午11.05.50.png>)

需要先設定好 target group (放入需要的 ec2 instance)，然後新增 load bal ancer，之後記得要去 LB 安全群組設置，不能用 default，不然會連線不到。

<img src=".gitbook/assets/截圖 2022-08-08 下午7.46.32.png" alt="" data-size="original">

![](<.gitbook/assets/截圖 2022-08-08 下午7.48.27.png>)

設置後用 DNS 名稱即可連線到 EC2 上的 web server。

## ELB 設定 HTTPS

記得去申請 acm 的 \*.domain 證書，可用在 api.domain 網域，然後串接上去 ELB listener&#x20;

![](<.gitbook/assets/截圖 2022-08-09 上午9.44.24.png>)

![](<.gitbook/assets/截圖 2022-08-09 上午9.43.47.png>)

最後到 Route53 新增 A 紀錄使用 ELB 即可

![](<.gitbook/assets/截圖 2022-08-09 上午9.44.03.png>)

## 串接 Cloudfront 與 ELB

這邊記得備用網域設置要使用的 domain，且使用的 https 證書與 ELB相同即可。

如果 ELB 可以直接設定為 A record 且連線正常，通常在 ELB 前面多串一個 cloudfront 然後把 A record 從 ELB 改為 cloudfront 也可以正常。

#### 設定內容

* Origin Domain Name: 選擇 ELB
* Origin Path: 空白
* Origin Protocol Policy: **HTTP Only （**CloudFront後面走 HTTP 即可）
* Viewer Protocol Policy: **Redirect HTTP to HTTPS**
* Allowed HTTP Methods: **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**
* Alternate Domain Names: 設定你的網站網址 (api.domain....)
* SSL Certificate: 使用 AWS Certification Manager 建立，將憑證建立在 (us-east-1) 才能使用。

> 設定後如果直接前往 cloudfront 給的網址會出現 502 錯誤，記得要先去 route53 設定好 A record 且指向 cloudfront，這樣前往要去的 domain 就可以了，但不要直接連到 cloudfront domain。

![](<.gitbook/assets/截圖 2022-08-09 上午10.35.10.png>)

<img src=".gitbook/assets/截圖 2022-08-09 上午10.36.48.png" alt="" data-size="original">

![](<.gitbook/assets/截圖 2022-08-09 上午10.36.43.png>)

且記得設置 "僅限 HTTP" 就好，讓流量從 cloudfront 後就從 HTTPS 轉為 HTTP，不然會有 server 502 ssl error。

![](<.gitbook/assets/截圖 2022-08-10 上午11.17.02.png>)

## AWS ECR

docker image 託管服務。

1.如果遇到如下問題：is not  authorized to perform: ecr-public:GetAuthorizationToken on resource: \* because no identity-based policy allows the ecr-public:GetAuthorizationToken action

[https://stackoverflow.com/questions/65727113/aws-ecr-user-is-not-authorized-to-perform-ecr-publicgetauthorizationtoken-on-r](https://stackoverflow.com/questions/65727113/aws-ecr-user-is-not-authorized-to-perform-ecr-publicgetauthorizationtoken-on-r)

```
Private registry: AmazonEC2ContainerRegistryFullAccess

Public registry: AmazonElasticContainerRegistryPublicFullAccess
```
