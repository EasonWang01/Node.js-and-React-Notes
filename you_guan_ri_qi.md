# 有關日期

```
Date.now()
1480399826585

```
```
new Date()
Tue Nov 29 2016 14:10:18 GMT+0800 (CST)
```
將date字串轉為timestamp
```
new Date().getTime()
1480399892808
```
將timestamp轉為new Date()
```
new Date(timestamp);
```
轉為ISO
```
new Date().toISOString();

2016-11-29T06:06:04.276Z
```

現在時間減十天 `ex:將"2017-04-11" 轉為 "2017-04-01"`
```
(new Date(Date.now() - 60*60*1000*24*10)).toISOString().substring(0,10)
"2017-04-01"
```