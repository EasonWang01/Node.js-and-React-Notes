---
description: 可用來存儲時序 time-series 資料
---

# Influx DB

下載：

{% embed url="https://portal.influxdata.com/downloads" %}

啟動：

```
sudo service influxdb start

sudo service influxdb status
```

之後到 8086 port 可看到 UI 介面

![](<.gitbook/assets/截圖 2021-10-29 下午2.05.57.png>)

### 範例

目前 2.0 版本官方範例均為 Flux 語言，不是 SQL

```javascript
const {InfluxDB} = require('@influxdata/influxdb-client')

// You can generate a Token from the "Tokens Tab" in the UI
const token = ''
const org = ''
const url = ''
const bucket = 'test'

const client = new InfluxDB({url, token})
const queryApi = client.getQueryApi(org)
const {Point} = require('@influxdata/influxdb-client')
const writeApi = client.getWriteApi(org, bucket)
writeApi.useDefaultTags({host: 'host1'})

const point = new Point('mem')
  .floatField('used_percent', 23.43234543)
writeApi.writePoint(point)
writeApi
    .close()
    .then(() => {
        console.log('FINISHED')
    })
    .catch(e => {
        console.error(e)
        console.log('\\nFinished ERROR')
    })


const query = `from(bucket: "${bucket}") |> range(start: -1h)`
queryApi.queryRows(query, {
  next(row, tableMeta) {
    const o = tableMeta.toObject(row)
    console.log(o)
  },
  error(error) {
    console.error(error)
    console.log('\\nFinished ERROR')
  },
  complete() {
    console.log('\\nFinished SUCCESS')
  },
})
```

### Flux 語言

[https://docs.influxdata.com/influxdb/v2.0/query-data/get-started/query-influxdb/](https://docs.influxdata.com/influxdb/v2.0/query-data/get-started/query-influxdb/)
