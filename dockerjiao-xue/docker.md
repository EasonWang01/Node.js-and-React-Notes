# Docker 指令

Ubuntu安裝部分可參考此

[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)

查看執行中的容器

```
sudo docker ps
```

終止某個container by ID

```
sudo docker stop 3e0485555356
```

使用docker hub可搜尋所有別人寫好的可用container\
[https://hub.docker.com/](https://hub.docker.com/)

## 1.列出所有執行中的container

```
docker ps
```

## 2.列出被使用過名稱的container

```
docker ps -a
```

## 3.停止特定container

輸入

```
docker status
```

會顯示類似如下

```
CONTAINER           CPU %               MEM USAGE / LIMIT       MEM %               NET I/O             BLOCK I/O           PIDS
01f51f8c9f7b        30.80%              326.6 MiB / 1.952 GiB   16.34%              3.6 MB / 522 kB     9.84 MB / 0 B       12
```

接著

```
docker stop 01f51f8c9f7b
```

## 4.停止所有container

```
docker stop $(docker ps -a -q)
```

## 5.移除所有使用名稱的container

在kill或stop container後要再把名稱移除才可再次重新使用

```
docker rm `docker ps -aq`
```

## 6.以名字顯示running中的container

```
docker stats $(docker ps --format={{.Names}})
```

## 7.進入Docker的Terminal

```
docker exec -it ede59484d5cd bash
```

### 8. Volume

可用來保存容器內的資料或是共通資料，named Volume 或 Host Volume。

> 記得使用host volume 時 路徑要用全名&#x20;
>
> 之後更改host 或 docker 上 volume資料夾內的檔案後另一端也會跟著改變

```
volumes: ["/Users/yicheng/server/database:/db"]
```

### 9. 將特定資料夾指定為 Volume

使用 local-persist plugin，之後可以直接使用此 Volume，避免創建新的 Volume，如果用 docker-compose 要搭配 external: true

\> [https://docs.docker.com/compose/compose-file/compose-file-v3/#external](https://docs.docker.com/compose/compose-file/compose-file-v3/#external)

{% embed url="https://github.com/MatchbookLab/local-persist" %}

{% embed url="https://github.com/MatchbookLab/local-persist" %}

### 10. 檢視已經存在的 Volume

```
docker volume inspect	"Display detailed information on one or more volumes"
docker volume ls	"List volumes"
```

### 11. 將 docker log 存成檔案

mongo 之類的 docker image 產生的 log 也可以直接從 docker log 看到

```
docker logs containername >& logs/myFile.log
```

### 12. host 與 container 複製

One specific file can be copied TO the container like:

```
docker cp foo.txt container_id:/foo.txt
```

One specific file can be copied FROM the container like:

```
docker cp container_id:/foo.txt foo.txt
```

{% embed url="https://stackoverflow.com/a/31971697/4622645" %}

### 13. docker 網路互通使用 network gateway

範例：

```yaml
version: '3'
services:
  mongo:
    image: mongo:6.0.2-focal
    restart: always
    volumes:
      - "./mongoData:/data/db"
    networks:
      web3-network:
        ipv4_address: 10.5.0.6
    ports:
      - 27017:27017
  web3:
    image: server1:1.0.0
    networks:
      web3-network:
        ipv4_address: 10.5.0.5
    ports:
      - 3130:3130
networks:
  web3-network:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```

### 14. docker 之間使用 localhost 互相網路連通 (新方式，不用設置 network gateway)

> **雖然可用 docker run --network=host ，但此做法 docker 連得到 host (本機電腦) localhost 其他 port， 但電腦連不進 docker)**

只要將代碼內的 `localhost` 改為 `host.docker.internal` 則可以連到其他電腦localhost，電腦也連得到 docker

{% embed url="https://stackoverflow.com/a/63112795/4622645" %}

> Linux 要記得加上 flag: `docker run -it --add-host=host.docker.internal:host-gateway ubuntu bash`

docker-compose 要加上如下:

```
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

[https://stackoverflow.com/a/67158212/4622645](https://stackoverflow.com/a/67158212/4622645)
