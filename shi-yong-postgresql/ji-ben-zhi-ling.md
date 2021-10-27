# 基本指令

## 進入psql後

[https://www.tutorialspoint.com/postgresql/postgresql\_create\_database.htm](https://www.tutorialspoint.com/postgresql/postgresql\_create\_database.htm)

### 建立DB

```
create database test;
```

也可輸入

```
CREATE DATABASE dbname;
```

> 如果沒反應，離開psql重進即可。

### 顯示目前所有DB列表。

```
\l
```

### 連線DB

```
\c <DB名稱>
```

### 建立Table

```
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```

與

```
CREATE TABLE DEPARTMENT(
   ID INT PRIMARY KEY      NOT NULL,
   DEPT           CHAR(50) NOT NULL,
   EMP_ID         INT      NOT NULL
);
```

> 可用ID SERIAL 來創建AUTO INCREMENT的Table，之後會建立一個特別相對應的表。

### 列出所有Table

```
\d
```

### 檢視詳細Table schema

```
\d <Table名稱>
```

### 更改密碼

如果使用 docker 要如下更改：

1. 進入容器 `sudo docker exec -it `\<container id> `/bin/sh`
2. 更改當前使用者 `su - postgres`
3. 更改密碼 `\password postgres`
4. 之後輸入兩次新密碼後即可，Adminer 也會跟著更改
