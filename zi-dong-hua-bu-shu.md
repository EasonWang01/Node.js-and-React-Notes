# GitLab 與 Drone

安裝 GitLab

```
mkdir GitLab && cd GitLab
curl https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml -O
docker-compose up
```

> 如果出現 ERROR: manifest for sameersbn/gitlab:13.0.7 not found:
>
> &#x20;把 docker-compose.yml 的gitlab image 版本改為&#x20;
>
> > sameersbn/gitlab:latest

安裝好輸入 docker ps 確認

![](<.gitbook/assets/螢幕快照 2020-07-31 上午10.03.27.png>)

進入 `localhost:10080`

![](<.gitbook/assets/螢幕快照 2020-07-31 上午10.03.31.png>)

預設會先請你輸入密碼，使用者名稱為 root

> 之後也可輸入 email 創建 user，不過不會收到 email

如果想要忘記密碼等傳送email功能需要設定 SMTP server

{% embed url="https://docs.gitlab.com/omnibus/settings/smtp.html" %}

## Gitlab clone 專案 with access token

通常在部署主機上需要 clone 專案，但使用個人的 ssh key 權限過大，所以可以創建專案的 access token，之後如下 clone 專案。

```
git clone https://oauth2:ACCESS_TOKEN@domain.com/.../package.git
```

## 安裝 Drone CI

> 這邊如果還沒綁定 domain，建議先把 localhost 的 GitLab 與等下會用的 drone 的 port 都用 ngrok 產生 domain ，這樣才能正常使用

{% embed url="https://docs.drone.io/server/provider/gitlab/" %}

1.先去新增 OAuth Application : [http://localhost:10080/profile/applications](http://localhost:10080/profile/applications)

> callback url 記得在最後面加上 /login

2.然後產生 share secret `openssl rand -hex 16`

3.新增 docker-compose.yml

> 這邊如果 GitLab 或 Drone 用 localhost ，之後再 gitLab 連結 drone 時會產生錯誤
>
> Login Failed. Post "[http://localhost:10080/oauth/token](http://localhost:10080/oauth/token)": dial tcp 127.0.0.1:10080: connect: connection refused\`

```
version: '2.3'

services:
  drone-server:
    image: drone/drone:1
    ports:
      - 10081:80
    volumes:
      - ./:/data
    restart: always
    environment:
      - DRONE_SERVER_HOST=66934108131d.ngrok.io
      - DRONE_SERVER_PROTO=https
      - DRONE_RPC_SECRET=131f7e1a919158e42dfdfc74d5552ec3
      - DRONE_AGENTS_ENABLED=true
      # Gitlab Config
      - DRONE_GITLAB_SERVER=http://1ee9b44b230d.ngrok.io
      - DRONE_GITLAB_CLIENT_ID=7f71d69fe5dab5d770a7cf1a56c876e3b6a366ba0b6036c4e320382714e5c84e
      - DRONE_GITLAB_CLIENT_SECRET=9b7e8f676137598590efbbf4081e0bedb949c6df6b2ca29be40b3bfb56bb419a
      - DRONE_LOGS_PRETTY=true
      - DRONE_LOGS_COLOR=true

  drone-runner:
    image: drone/drone-runner-docker:1
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DRONE_RPC_HOST=66934108131d.ngrok.io
      - DRONE_RPC_PROTO=http
      - DRONE_RPC_SECRET=131f7e1a919158e42dfdfc74d5552ec3
```

之後連入 drone url 會自動導到 gitLab 連結 drone 畫面，點選 authorize 即會導入到 drone 頁面

![](<.gitbook/assets/螢幕快照 2020-07-31 上午11.18.58.png>)

4\. 剛才看到 docker-compose 下方的 runner 為用來執行 pipeline的
