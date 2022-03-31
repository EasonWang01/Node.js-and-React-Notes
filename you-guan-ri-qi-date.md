# 有關日期Date

如果跟時間相關的，記得本地環境與雲端 server 獲取到的 Date.now 因為時區關係可能不同，部署後要去設置雲端 server 時區。

```
data                                       // 查看當前 server 時間
timedatectl list-timezones                 // 查看所有可設置的時區
sudo timedatectl set-timezone Asia/Taipei  // 設置時區
```

## 時間範圍區間檢測

```javascript

const phaseConfig = {
  "phase-A": {
      start: "2022-03-31 13:23:50",
      end: "2022-04-30 19:00:00",
  }
}

  const { start, end } = phaseConfig[phase];
  const isWithinTime = withinTime(start, end);
  if (!isWithinTime.success) {
    return isWithinTime;
  }


function withinTime(start, end) {
  const toTimestamp = (strDate) => {
    var datum = Date.parse(strDate);
    return datum / 1000;
  };
  const now = Date.now();
  if (start && Number(`${toTimestamp(start)}000`) > now) {
    return {
      success: false,
      date: [start, end],
      message: "Not available yet",
    };
  }
  if (end && Number(`${toTimestamp(end)}000`) < now) {
    return {
      success: false,
      date: [start, end],
      message: "Already expired",
    };
  }
  return {
    success: true,
    date: [start, end],
    message: "Within time",
  };
}
```

## 有關日期

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
```

## 注意事項

```
1.new Date(1496716816519) 在browser會比node環境多8小時
因為new Date會自動判斷當下時區UTC去做timestamp加減
```
