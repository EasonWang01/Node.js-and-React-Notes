# 有關日期Date



## 有關日期

```text
Date.now()
1480399826585
```

```text
new Date()
Tue Nov 29 2016 14:10:18 GMT+0800 (CST)
```

將date字串轉為timestamp

```text
new Date().getTime()
1480399892808
```

將timestamp轉為new Date\(\)

```text
new Date(timestamp);
```

轉為ISO

```text
new Date().toISOString();

2016-11-29T06:06:04.276Z
```

現在時間減十天 `ex:將"2017-04-11" 轉為 "2017-04-01"`

```text
(new Date(Date.now() - 60*60*1000*24*10)).toISOString().substring(0,10)
```

## \#注意事項

```text
1.new Date(1496716816519) 在browser會比node環境多8小時
因為new Date會自動判斷當下時區UTC去做timestamp加減
```

