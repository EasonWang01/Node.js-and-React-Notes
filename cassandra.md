# \#安裝

[http://cassandra.apache.org/download/](http://cassandra.apache.org/download/)

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

# 使用

```
cqlsh

1.新增DB
CREATE KEYSPACE tutorialspoint
WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};

2.使用DB
 USE tutorialspoint;

3.新增Table
CREATE TABLE emp(
   emp_id int PRIMARY KEY,
   emp_name text,
   emp_city text,
   emp_sal varint,
   emp_phone varint
   );

4.確認Table
select * from emp;

5.新增資料

CREATE TABLE users(
user_id int PRIMARY KEY,
user_account text,
user_password text,
);

INSERT INTO users (user_id, user_account, user_password) VALUES(1,'test123', '12345');


```

# 預設cassandra只能讓Localhost連

要更改可參考[https://stackoverflow.com/questions/32258471/restricting-cassandra-to-localhost-only](https://stackoverflow.com/questions/32258471/restricting-cassandra-to-localhost-only)

