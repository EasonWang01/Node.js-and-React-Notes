# Instagram API

## Instagram API

改版後期改用sandbox模式，意思為，所有app要先經過審核才可使用public\_content部分，一開始你只可讀取到你自己的content，或是你可以把別人邀請入你的sandbox，之後其同意後你也可以API獲取其資訊，不然都需經過審核才可使用API

1.先到如下網站，註冊帳號

[https://www.instagram.com/developer](https://www.instagram.com/developer)

* 到Authentication可看獲取Access token的方法
* 到EndPoints可看使用API的方法

2.其分為兩種存取API的方式

```text
使用三階段(建議此種作法)
client-ID-->code-->access-token

使用兩階段(如沒有server，安全性較低)
client-ID-->access-token


參考如下
https://www.instagram.com/developer/authentication/
```

### 範例:

```text
https://www.instagram.com/oauth/authorize?client_id=3b6d608eb019431ca90bde60c9178e21&redirect_uri=http://localhost:3000/users/auth/instagram/callback&response_type=token&scope=basic+public_content+follower_list+comments+relationships+likes
```

之後會跳轉到你填寫的redirect URL 並在網址最後面附上`Access-token`

## 爬蟲參數

因為現在API開放少，所以決定直接使用爬蟲方法來使用API

### 查詢指定tag的所有文章

GET

```text
https://www.instagram.com/explore/tags/輸入欲查詢的TAG/?__a=1
```

### 查詢使用者基本資訊與前幾篇文章

GET

```text
https://www.instagram.com/使用者帳號/?__a=1
```

> 可以查到使用者數字ID、名字、追蹤人數、等等。

### 查詢使用者文章與動態

```text
https://www.instagram.com/graphql/query/?query_hash=...&variables=...
```

#### Query請求格式

instagram的XHR固定格式如下

```text
https://www.instagram.com/graphql/query/?query_hash=....&variables=...
```

* query\_hash 為發送請求的種類 \( 例如請求加載使用者後續圖片的hash均為 `472f257a40c653c64c666ce877d59d2b`\)
* variables 為一個json經過urlEncode過的字串

#### Querystring參數

有兩個參數，query\_hash與variables

**第一種：** 

當query\_hash為`7e1e0c68bbe459cf48cbd5533ddee9d`時 \(加載使用者推薦好友相關的資訊\)

variables參數：

```javascript
{
 "user_id":"275237117", 
 "include_chaining":true, 
 "include_reel":true,
 "include_suggested_users":false, 
 "include_logged_out_extras":false
}
```

e.g.

```text
https://www.instagram.com/graphql/query/?query_hash=7e1e0c68bbe459cf48cbd5533ddee9d4&variables=%7B%22user_id%22%3A%22275237117%22%2C%22include_chaining%22%3Atrue%2C%22include_reel%22%3Atrue%2C%22include_suggested_users%22%3Afalse%2C%22include_logged_out_extras%22%3Afalse%7D
```

**第二種：** 

當query\_hash為 `472f257a40c653c64c666ce877d59d2b`時 \(加載使用者文章\)

variables參數：

```javascript
{
  "id":"275237117",
  "first":12,
  "after":"AQBEU_pfdtAHWuxSKwtTEIYRnN8LIHtBASC8bAaQGgpD9r3ZaaVu0qMQzh_qArARwpdM2jt0tprfp35rtcX268DNOFUTBEH7yme7oC8R6mRAug"
}
```

> 其中first參數代表取初幾張圖，after也為end\_cursor，意思為結束的位置，所以越大越好，這樣我們才能一次取出足夠的圖片
>
> after參數建議使用如下
>
> ```text
> AQBEU_pfdtAHWuxSKwtTEIYRnN8LIHtBASC8bAaQGgpD9r3ZaaVu0qMQzh_qArARwpdM2jt0tprfp35rtcX268DNOFUTBEH7yme7oC8R6mRAug
> ```
>
> end\_cursor可以從上次的query請求中的Response獲得。

e.g.

```text
https://www.instagram.com/graphql/query/?query_hash=472f257a40c653c64c666ce877d59d2b&variables=%7B%22id%22%3A%22275237117%22%2C%22first%22%3A12%2C%22after%22%3A%22AQDgv0_xlXhuHI_YQW8deViqPYXPj7dim6ODe_tAbM6XLhqwbe-Xp4JPEHpLAJ5XGusu-nKdFoCYCVFcF7OkjSscKISMfCYIsEVs8zx9h2rWaQ%22%7D
```

**第三種：**

當query\_hash為 `bf41e22b1c4ba4c9f31b844ebb7d9056` 時 \(加載使用者動態影片\)

```javascript
query_hash: bf41e22b1c4ba4c9f31b844ebb7d9056
variables: {"reel_ids":["275237117"],"precomposed_overlay":false}
```

> reel\_ids即為使用者ID

e.g.

```text
https://www.instagram.com/graphql/query/?query_hash=bf41e22b1c4ba4c9f31b844ebb7d9056&variables=%7B%22reel_ids%22%3A%5B%22275237117%22%5D%2C%22precomposed_overlay%22%3Afalse%7D
```

### 取得使用者發佈過的文章圖片

所以現在我們來試著取得使用者的所有文章，首先我們要先知道要查詢的使用者ID

所以我們先使用如下查詢ID

```javascript
const https = require('https');


function https_request(username, querystring) {
  let chunk = '';
  return new Promise((resolve, reject) => {
    const options = {
      hostname: 'www.instagram.com',
      port: 443,
      path: `/${username}/${querystring}`,
      method: 'GET'
    };
    const req = https.request(options, (res) => {
      res.on('data', (d) => {
          chunk += d;
      });
      res.on('end', () => {
        resolve(chunk)
      })
    });
    req.on('error', (e) => {
      console.error(e);
    });
    req.end();
  })  
}  

https_request('liona_luona', '?__a=1').then(data => {
  console.log(JSON.parse(data).graphql.user.id)
})
```

之後我們有了ID

我們可以將下面改為如下查找使用者發表過的文章數量

```javascript
https_request('liona_luona', '?__a=1').then(data => {
  console.log(JSON.parse(data).graphql.user.edge_owner_to_timeline_media.count)
})
```

有了ID與文章數量後我們就可以來拼出參數

我們先拼出querystring

```javascript
{
  "id":"275237117",
  "first": 1000, //或是上剛才查出使用者的文章數量，因為我們要一次查全部
  "after":"AQBEU_pfdtAHWuxSKwtTEIYRnN8LIHtBASC8bAaQGgpD9r3ZaaVu0qMQzh_qArARwpdM2jt0tprfp35rtcX268DNOFUTBEH7yme7oC8R6mRAug"
}
```

發送請求

```javascript
const https = require('https');


function https_request(path, querystring) {
  let chunk = '';
  return new Promise((resolve, reject) => {
    const options = {
      hostname: 'www.instagram.com',
      port: 443,
      path: `/${path}/${querystring}`,
      method: 'GET'
    };
    const req = https.request(options, (res) => {
      res.on('data', (d) => {
          chunk += d;
      });
      res.on('end', () => {
        resolve(chunk)
      })
    });
    req.on('error', (e) => {
      console.error(e);
    });
    req.end();
  })  
}  

https_request('yicheng71248', '?__a=1').then(data => {
  userID = JSON.parse(data).graphql.user.id
  userArticleCount = JSON.parse(data).graphql.user.edge_owner_to_timeline_media.count
}).then(() => {
  let urlencodeP = encodeURIComponent(
    `{"id": ${userID},
     "first": ${userArticleCount},
     "after":"AQBo_T54D3Isvkn39aEAn5WO1VvQXmLmZzReXHtfgylI-l4IrcVMMRs0Kqz1Q2tu5Jrkcw1ScAfAUddkbVuBiDTXhkHI5jz58I1xj3kxVuzlDQ"
    }`)
    console.log(urlencodeP)
  let querystring = `?query_hash=472f257a40c653c64c666ce877d59d2b&variables=${urlencodeP}`
  https_request('graphql/query', querystring).then(data => {
    console.log(data)
  })
})
```

注意：如果first參數超過一千以上會產生timeout情況，返回FB錯誤頁面。

接著我們用Async Loop的方式讀取使用者所有圖片

```javascript
const https = require('https');

let articles = [];
let current_count = 0;
let iterateCount = 1000;
let username = "liona_luona";

function https_request(path, querystring) {
  let chunk = '';
  return new Promise((resolve, reject) => {
    const options = {
      hostname: 'www.instagram.com',
      port: 443,
      path: `/${path}/${querystring}`,
      method: 'GET'
    };
    const req = https.request(options, (res) => {
      res.on('data', (d) => {
        chunk += d;
      });
      res.on('end', () => {
        resolve(chunk)
      })
    });
    req.on('error', (e) => {
      console.error(e);
    });
    req.end();
  })
}

https_request(username, '?__a=1').then(data => {
  userID = JSON.parse(data).graphql.user.id
  userArticleCount = JSON.parse(data).graphql.user.edge_owner_to_timeline_media.count
  current_endCursor = "AQBo_T54D3Isvkn39aEAn5WO1VvQXmLmZzReXHtfgylI-l4IrcVMMRs0Kqz1Q2tu5Jrkcw1ScAfAUddkbVuBiDTXhkHI5jz58I1xj3kxVuzlDQ"
}).then(() => {

  (async function loop() {
    for (let i = 0; i < userArticleCount; i += iterateCount) {
      await new Promise(resolve => {
        let urlencodeP = encodeURIComponent(
          `{"id": ${userID},
            "first": ${iterateCount},
            "after": "${current_endCursor}"
          }`);
        let querystring = `?query_hash=472f257a40c653c64c666ce877d59d2b&variables=${urlencodeP}`
        https_request('graphql/query', querystring).then(data => {
          if(JSON.parse(data).status === 'fail') {
            console.log(data);
            return
          }

          let _articles = JSON.parse(data).data.user.edge_owner_to_timeline_media.edges;
          _articles.forEach(article => {
            articles.push(article.node.display_url);
          })
          current_endCursor = JSON.parse(data).data.user.edge_owner_to_timeline_media.page_info.end_cursor;
          resolve();
          if (i + iterateCount > userArticleCount) {
            // 讀取全部後
            console.log(articles)
          }
        })
      });
    }
  })();
}).catch(err => {
  console.log('No user')
})
```

## 注意事項：

1.2017/10/1號之後只能取得Basic的資訊，其他 API 都不開放了。

[https://www.instagram.com/developer/changelog/](https://www.instagram.com/developer/changelog/)

2.爬蟲執行過多次後會出現以下錯誤：

```text
{"message": "rate limited", "status": "fail"}
```

> 解決方法為把first調大，並減少iterate的request次數

3.2019年之後\`?\_\_a=1\` request要加上cookie才可

## 從 userid 獲得資料

```javascript
var https = require("https");
const fs = require("fs");
const accounts = ['8'];
const promises = [];
// https://i.instagram.com/api/v1/users/{user_id}/info/
const doRequest = (account) => {
  const result = new Promise((resolve, reject) => {
    let chunk = "";
    var options = {
      host: "i.instagram.com",
      port: 443,
      path: `/api/v1/users/${account}/info/`,
      method: "GET",
      headers: {
        cookie: `ig_did=CEDC32AA-9FF4-4A94-B2C3-5833EE96BE87; mid=X5kwfQAEAAFmGSa00xLy_QF0z8dv; ig_nrcb=1; fbm_124024574287414=base_domain=.instagram.com; fbsr_124024574287414=alcLEmDcis8uHix2t39oJKFeAnWICvOYIkoI8LuybD0.eyJ1c2VyX2lkIjoiMTAwMDE0NzI0Mjc2MDc1IiwiY29kZSI6IkFRQ1ZXc0pqbzhZZ3FhbmVPUmU2bThDVzdNelFOSXU4c3NVV1JleW5uZnJhbFBQS2MzUU9mQ0lPZmZ6OWFqQ2NQdWVmUHA0MzRlVWJSYU1Yc3N2NlJfY1daaUkwUzZYdjdoOVhmUnlfTG1MdVZBVUxWODJuR1d0V2VWTDRrMVVNaWFNcmJXck8xT0dlT0NKa1ZDakMxaU9FSXNFODYyWkhjdWZhQ0w4a0NQTUdRVHAwWl92MS1SbzA2YVZCSkc2ZWM0U3NwLS1CeWFPc1A1Z3RxZktfV1k5c3BvLTFDbm9LbVd1VVlLN2FnaVdEMzllTDI5UWh6dTZDa1pxaXlzT3ItcTB0VUNCSllEZzJpdzRFUFYzY09ZekI5eHI2YUhtSHhUbW1WSTZUeEpPc3NUQXBwRGx4ZlROV1h2eVh4QjZEUE1zemlxb0d1ZmNBUFRuejU3SEtLbEhJIiwib2F1dGhfdG9rZW4iOiJFQUFCd3pMaXhuallCQUoyTzdHeFpDczRTckptR09oRXo1Z2lXbkhNN3cybDV6RW9ucTJJdFdtcHpWbEFaQjA4clhnMUpvelR5ZExRb2JCWVJHOGRuNENsUmd2emd5NGt3WEJsT3pHQVVaQUkxb2ltb0gzUGV4MDB4Q1ZUQndjeVdjRXRhbUYwS0gzMDlRa2t4RXpNcUFRWkF5QmRqNjdCMWQ2VTJpWXdGSFljUVlaQlpBSGU2b0siLCJhbGdvcml0aG0iOiJITUFDLVNIQTI1NiIsImlzc3VlZF9hdCI6MTYxMjQ4OTc3MH0; csrftoken=XtzdIhOjSR0ZTlGJS1ZeTTzxWkemduLQ; ds_user_id=45331647121; sessionid=45331647121:1wHNFGMdrml5No:8; rur=FTW`,
        'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_3 like Mac OS X) AppleWebKit/603.3.8 (KHTML, like Gecko) Mobile/14G60 Instagram 12.0.0.16.90 (iPhone9,4; iOS 10_3_3; en_US; en-US; scale=2.61; gamut=wide; 1080x1920)'
      },
    };

    var req = https.request(options, function (res) {
      //console.log("STATUS: " + res.statusCode);
      //console.log('HEADERS: ' + JSON.stringify(res.headers));
      res.setEncoding("utf8");
      res.on("data", function (data) {
        chunk += data;
      });
      res.on("end", function () {
        console.log(chunk)
        const data = JSON.parse(chunk);
        console.log(data)
        resolve();
      });
    });

    req.on("error", function (e) {
      console.log("problem with request: " + e.message);
      reject();
    });

    // write data to request body
    req.write("data\n");
    req.write("data\n");
    req.end();
  });
  promises.push(result);
};

accounts.forEach((account) => {
  console.log(account);
  try {
    doRequest(account);
  } catch (err) {
    console.log(err);
  }
});

Promise.all(promises).then(() => {
  console.log('finish')
})

```

