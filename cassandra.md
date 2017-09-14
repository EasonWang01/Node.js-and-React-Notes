# \#安裝

http://cassandra.apache.org/download/

Linux\(Debian or Ubuntu\)

```
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list;
curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -;
sudo apt-get update;
sudo apt-get install cassandra;
// start
sudo service cassandra start
//check
cqlsh
```

# 

# 基礎操作

1.輸入cqlsh進入command line

> 如果用docker 輸入 docker ps 然後複製docker id 之後輸入 docker exec -it &lt;docker ID&gt; /bin/bash

2.基本結構

![](/assets/analogy.jpg)

