# 自動化部屬

安裝 GitLab

```text
mkdir GitLab && cd GitLab
curl https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml -O
docker-compose up
```

> 如果出現 ERROR: manifest for sameersbn/gitlab:13.0.7 not found:
>
>  把 docker-compose.yml 的gitlab image 版本改為 
>
> > sameersbn/gitlab:latest

安裝好輸入 docker ps 確認

![](.gitbook/assets/ying-mu-kuai-zhao-20200731-shang-wu-10.03.27.png)

進入 `localhost:10080`

![](.gitbook/assets/ying-mu-kuai-zhao-20200731-shang-wu-10.03.31.png)

預設會先請你輸入密碼，使用者名稱為 root

> 之後也可輸入 email 創建 user，不過不會收到 email

如果想要忘記密碼等傳送email功能需要設定 SMTP server

[https://docs.gitlab.com/omnibus/settings/smtp.html](https://docs.gitlab.com/omnibus/settings/smtp.html)

