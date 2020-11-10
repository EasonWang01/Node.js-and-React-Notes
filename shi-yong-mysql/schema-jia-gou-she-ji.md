# Schema 架構設計

## Messaging APP

> User: 存入 conversion Id List（參與的對話列表）
>
> Messages: 存入每個對話的細節以及 conversion Id
>
> Conversions: Key 為 conversion Id，存入該對話最後的傳輸人與時間
>
> 使用：對話列表為查詢 User conversion Id List 對應出的 Conversions Table 內容

> ，點進去則查詢 conversion Id 對應的所有 messages

{% embed url="https://stackoverflow.com/questions/6033062/messaging-system-database-schema" %}

{% embed url="https://stackoverflow.com/questions/6420264/creating-a-threaded-private-messaging-system-like-facebook-and-gmail" %}

[https://stackoverflow.com/questions/6541302/thread-messaging-system-database-schema-design](https://stackoverflow.com/questions/6541302/thread-messaging-system-database-schema-design)

