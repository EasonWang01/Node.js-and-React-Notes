# Docker指令

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
[https://hub.docker.com/](https://hub.docker.com)

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
