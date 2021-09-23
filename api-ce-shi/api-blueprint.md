# API Blueprint

## \# API Blueprint

> 點選Editor網頁上方人的按鈕即可發送Email給團隊成員編輯。
>
> 免費方案可以給五個成員共同編輯，十個成員瀏覽。

## 也是一種寫API 文件的工具

[https://github.com/apiaryio/api-blueprint](https://github.com/apiaryio/api-blueprint)

[https://app.apiary.io](https://app.apiary.io)

[https://help.apiary.io/](https://help.apiary.io/)

畫面稍微看起來比swagger漂亮一點

#### 寫文件教學\(Specification\)

[https://github.com/apiaryio/api-blueprint/blob/master/API Blueprint Specification.md\#def-headers-section](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-headers-section)

### \#Example

```yaml
FORMAT: 1A
HOST: https://api.xblockchain.co/

# xblockchain

xblockchain API is a industrial blockchain service, real-time, high speed, no miner, based on Distributed Ledger technology.It allows anyone issue and manage digital assets like stocks and bonds, commodities, currencies, land titles, music or software licensing, gift cards and loyalty points.

## Upload Data [/api/v1/upload]

### Chain upload [POST]

This the method that provide nodes to upload `Payload Name and value`from the main chain and using AES-256 and GZIB.

which will return a `file hash`

+ Request 

    + Headers 

            content-type: application/x-www-form-urlencoded
    + Body

            data="{"name": "Adam", "height": "178cm"}"&password=123452

+ Response 200 (application/x-www-form-urlencoded)

        Added file:<file hash>

## Download Data [/api/v1/download]

### Chain Download [POST]

This the method that provide nodes to download `Payload Name and value`from the main chain providing the file hash returned by the upload API then AES-256 decrypt and unzip.

which will return a `original content of the data`

+ Request 

    + Headers 

            content-type: application/x-www-form-urlencoded
    + Body

            hash=QmPJe1PJgKi6jS12Z96v8NZBPjSyBdGvqBqtkPgjCuhrEx&password=12345

+ Response 200 (application/x-www-form-urlencoded)

        <Decrypted File content>


## Chain status  [/api/v1/chainstatus]

### Chain status query [GET]

+ Response 200 (application/json)

        [
            {
                "status": "ok",
                "timestamp": "2017-08-05T08:40:51.620Z",
                "nodes": ["21.112.2.33","23,56,77,122"]
            }
        ]

## Object info [/api/v1/object]

### Object info query [POST]

+ Request (application/json)

        {
            "question": "Favourite programming language?",
            "choices": [
                "Swift",
                "Python",
                "Objective-C",
                "Ruby"
            ]
        }

+ Response 201 (application/json)

    + Headers

            Location: /questions/2

    + Body

            {
                "question": "Favourite programming language?",
                "published_at": "2015-08-05T08:40:51.620Z",
                "choices": [
                    {
                        "choice": "Swift",
                        "votes": 0
                    }, {
                        "choice": "Python",
                        "votes": 0
                    }, {
                        "choice": "Objective-C",
                        "votes": 0
                    }, {
                        "choice": "Ruby",
                        "votes": 0
                    }
                ]
            }
```

## 範例:

[https://pandurangpatil.docs.apiary.io/\#reference/user](https://pandurangpatil.docs.apiary.io/#reference/user)

> 點左上角即可下載範例檔案

