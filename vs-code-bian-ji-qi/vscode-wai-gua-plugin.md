# VSCode 外掛 Plugin

#### 1.Preview on Web Server yuichinukiyama

方便在VSCode 上查看網頁。

&#x20;開啟 html 後點選 ctrl+shift+v

![](<../.gitbook/assets/截圖 2020-10-23 上午9.58.24.png>)

#### 2. GitLens

查看相關 GIt 記錄，並且會直接顯示在行旁邊。

#### 3. Hex Editor

方便查看 binary 的檔案

#### 4. Path Intellisense

輸入路徑時會自動建議

#### 5. Vscode Google Translate

選取後快速翻譯，點選 plugin setting 設定要翻譯哪個語言

選擇字後按 shift+option+t

#### 6. Remote ssh

![](<../.gitbook/assets/截圖 2022-10-18 上午10.16.24.png>)

編輯 .ssh/config 檔案後會自動出現在列表中

![](../.gitbook/assets/fdsfsdfsdf.png)

#### 7. 使用瀏覽器 ssh 進機器並且為 VSCode 介面

Github: [https://github.com/coder/code-server](https://github.com/coder/code-server)

Docker hub: [https://hub.docker.com/r/codercom/code-server](https://hub.docker.com/r/codercom/code-server)

docker-compose 範例：

```yaml
version: "2.1"
services:
  code-server:
    image: codercom/code-server:latest
    container_name: code-server
    user: root
    environment:
      - PASSWORD=password #optional
      - HASHED_PASSWORD= #optional
      - SUDO_PASSWORD=password #optional
      - SUDO_PASSWORD_HASH= #optional
    volumes:
      - ~/.config:/config
      - ~/project:/home/user/project
    ports:
      - 8080:8080
    restart: unless-stopped
```

其他 environment config 可參考：

{% embed url="https://github.com/coder/deploy-code-server/tree/main/deploy-container#environment-variables" %}

#### 8. VsCode MongoDB playground

類似 mongoDB compass, 並且可以在上面快速的更新資料，與使用 playground 測試 query

{% embed url="https://code.visualstudio.com/docs/azure/mongodb" %}

#### 9. VsCode 進入 Docker&#x20;

![](<../.gitbook/assets/截圖 2022-11-03 下午6.10.49.png>)

選擇 Containers 之後下面選一個 container，點選 Attach to container ，即可進入該 docker ，快速方便的改 container 內檔案

[https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers)
