# \#cluster with Docker

https://hub.docker.com/\_/cassandra/

#### 1.同台電腦不同Docker連線

1.

```
docker run --name some-cassandra -d cassandra:latest
```

2.

連線到同台電腦的其他docker

```
docker run --name some-cassandra2 -d -e CASSANDRA_SEEDS="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' some-cassandra)" cassandra:latest
```

或是

```
docker run --name some-cassandra2 -d --link some-cassandra:cassandra cassandra:latest
```



#### 不同台電腦Docker連線

假設兩台電腦ip位址分別為以下

`10.42.42.42`

`10.43.43.43`





