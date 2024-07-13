---
description: Amazon Athena
---

# AWS Athena

可用來分析 CloudFront 的 requesst log

1.需要先建立一個 s3 然後把 cloudfront log 串接，如下設置 cloudfront Standard logging

<figure><img src="../.gitbook/assets/截圖 2024-07-13 下午8.18.25.png" alt=""><figcaption></figcaption></figure>

2.在 Athena 連接相同 s3&#x20;

<figure><img src="../.gitbook/assets/截圖 2024-07-13 下午8.18.54.png" alt=""><figcaption></figcaption></figure>

3.創建 Table

```
CREATE EXTERNAL TABLE IF NOT EXISTS cloudfront_standard_logs (
  `date` DATE,
  time STRING,
  x_edge_location STRING,
  sc_bytes BIGINT,
  c_ip STRING,
  cs_method STRING,
  cs_host STRING,
  cs_uri_stem STRING,
  sc_status INT,
  cs_referrer STRING,
  cs_user_agent STRING,
  cs_uri_query STRING,
  cs_cookie STRING,
  x_edge_result_type STRING,
  x_edge_request_id STRING,
  x_host_header STRING,
  cs_protocol STRING,
  cs_bytes BIGINT,
  time_taken FLOAT,
  x_forwarded_for STRING,
  ssl_protocol STRING,
  ssl_cipher STRING,
  x_edge_response_result_type STRING,
  cs_protocol_version STRING,
  fle_status STRING,
  fle_encrypted_fields INT,
  c_port INT,
  time_to_first_byte FLOAT,
  x_edge_detailed_result_type STRING,
  sc_content_type STRING,
  sc_content_len BIGINT,
  sc_range_start BIGINT,
  sc_range_end BIGINT
)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY '\t'
LOCATION 's3://BUCKET-名稱/'
TBLPROPERTIES ( 'skip.header.line.count'='2' )
```

4.即可 Query 出資料

<figure><img src="../.gitbook/assets/截圖 2024-07-13 下午8.16.52.png" alt=""><figcaption></figcaption></figure>

{% embed url="https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html#access-logs-analyzing" %}

[https://docs.aws.amazon.com/athena/latest/ug/cloudfront-logs.html](https://docs.aws.amazon.com/athena/latest/ug/cloudfront-logs.html)
