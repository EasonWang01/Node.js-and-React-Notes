# 基本指令

使用 docker-compose.yml

> 一樣要建立 volumn

```text
# Use postgres/example user/password credentials
version: '3.1'

services:

  db:
    image: postgres
    restart: always
    volumes:
      - "./dbdata:/var/lib/postgresql/data"
    ports:
      - 5432:5432
    environment:
      POSTGRES_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

## 進入psql後

[https://www.tutorialspoint.com/postgresql/postgresql\_create\_database.htm](https://www.tutorialspoint.com/postgresql/postgresql_create_database.htm)

### 建立DB

```text
create database test;
```

也可輸入

```text
CREATE DATABASE dbname;
```

> 如果沒反應，離開psql重進即可。

### 顯示目前所有DB列表。

```text
\l
```

### 連線DB

```text
\c <DB名稱>
```

### 建立Table

```text
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```

與

```text
CREATE TABLE DEPARTMENT(
   ID INT PRIMARY KEY      NOT NULL,
   DEPT           CHAR(50) NOT NULL,
   EMP_ID         INT      NOT NULL
);
```

> 可用ID SERIAL 來創建AUTO INCREMENT的Table，之後會建立一個特別相對應的表。

### 列出所有Table

```text
\d
```

### 檢視詳細Table schema

```text
\d <Table名稱>
```

