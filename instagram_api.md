# Instagram API

改版後期改用sandbox模式，意思為，所有app要先經過審核才可使用public\_content部分，一開始你只可讀取到你自己的content，或是你可以把別人邀請入你的sandbox，之後其同意後你也可以API獲取其資訊，不然都需經過審核才可使用API

1.先到如下網站，註冊帳號

[https://www.instagram.com/developer](https://www.instagram.com/developer)

* 到Authentication可看獲取Access token的方法

* 到EndPoints可看使用API的方法

2.其分為兩種存取API的方式

```
使用三階段(建議此種作法)
client-ID-->code-->access-token

使用兩階段(如沒有server，安全性較低)
client-ID-->access-token


參考如下
https://www.instagram.com/developer/authentication/
```

## 範例:

```
https://www.instagram.com/oauth/authorize?client_id=3b6d608eb019431ca90bde60c9178e21&redirect_uri=http://localhost:3000/users/auth/instagram/callback&response_type=token&scope=basic+public_content+follower_list+comments+relationships+likes
```

之後會跳轉到你填寫的redirect URL 並在網址最後面附上`Access-token`

# 爬蟲參數

因為現在API開放少，所以決定直接使用爬蟲方法來使用API

## 查詢指定tag的所有文章

GET

```
https://www.instagram.com/explore/tags/輸入欲查詢的TAG/?__a=1
```

## 查詢使用者基本資訊與前幾篇文章

GET

```
https://www.instagram.com/使用者帳號/?__a=1
```

> 可以查到使用者數字ID、名字、追蹤人數、等等。

## 取得使用者發布過的文章

instagram的XHR固定格式如下

```
https://www.instagram.com/graphql/query/?query_hash=....&variables=...
```

* query\_hash 為發送請求的種類 \( 例如請求加載使用者後續圖片的hash均為 `472f257a40c653c64c666ce877d59d2b`\)

* variables 為一個json經過urlEncode過的字串

#### varibles格式有兩種

第一種： 當query\_hash為`7e1e0c68bbe459cf48cbd5533ddee9d`時 \(加載使用者相關的資訊\)

```json
{
 "user_id":"275237117", 
 "include_chaining":true, 
 "include_reel":true,
 "include_suggested_users":false, 
 "include_logged_out_extras":false
}
```

第二種：  當query\_hash為 `472f257a40c653c64c666ce877d59d2b`時 \(加載使用者文章\)

```json
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
> ```
> AQBEU_pfdtAHWuxSKwtTEIYRnN8LIHtBASC8bAaQGgpD9r3ZaaVu0qMQzh_qArARwpdM2jt0tprfp35rtcX268DNOFUTBEH7yme7oC8R6mRAug
> ```

所以現在我們來試著取得使用者的所有文章，首先我們要先知道要查詢的使用者ID

所以我們先使用如下查詢ID

```js
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
https_request('yicheng71248', '?__a=1').then(data => {
  console.log(JSON.parse(data).graphql.user.id)
})
```

之後我們有了ID就可以來查找使用者以前發送過的所有文章



# 注意事項：

2017/10/1號之後只能取得Basic的資訊，其他 API 都不開放了。

[https://www.instagram.com/developer/changelog/](https://www.instagram.com/developer/changelog/)

