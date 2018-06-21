# 進入psql後

[https://www.tutorialspoint.com/postgresql/postgresql\_create\_database.htm](https://www.tutorialspoint.com/postgresql/postgresql_create_database.htm)

## 建立DB

```
create database test;
```

也可輸入

```
CREATE DATABASE dbname;
```

> 如果沒反應，離開psql重進即可。

## 顯示目前所有DB列表。

```
\l
```

## 連線DB

```
\c <DB名稱>
```

## 建立Table

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

## 列出所有Table

```
\d
```



