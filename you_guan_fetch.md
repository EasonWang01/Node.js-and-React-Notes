# 有關Fetch

https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch\_API/Using\_Fetch

以前用xmlHttprequest但寫法太多，fetch為比較簡潔的寫法，並且有then可使用

注意事項:

1.使用json\(\)轉換or其他型態轉換回傳資料e.g. text\(\), blob\(\) ...[https://developer.mozilla.org/en-US/docs/Web/API/Body](https://developer.mozilla.org/en-US/docs/Web/API/Body)

2.第二個then才拿得到資料，第一個then只是一個promise結果

3.cookie要手動在header加入\(第二個參數\)

```
     fetch('http://localhost:3001/getArticle',{
           method: 'GET',
       })
       .then((response) => {
           if (response.status >= 200 && response.status < 300) {
               return response.json();
           } else {
               var error = new Error(response.statusText)
               error.response = response;
               throw error;
           }
       })
       .then((data) => {
         console.log(data)
       })
       .catch(function(error) {
           console.log('request failed', error);
           return error.response.json();
       })
```

[https://www.reddit.com/r/learnprogramming/comments/3ydnmn/javascriptnodejswhatwgfetch\_why\_does\_this\_return/](https://www.reddit.com/r/learnprogramming/comments/3ydnmn/javascriptnodejswhatwgfetch_why_does_this_return/)

使用POST

```
fetch('http://localhost:3016/login', {
    method: 'POST',
    headers: {'Content-Type':'application/x-www-form-urlencoded'},
    body: 'foo=bar&blah=1'
});
```

```js
fetch('http://localhost:3000/info', {
    method: "POST",
    body: JSON.stringify({test: 123})
  })
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```

# 如果要傳JSON要先stringify

```
fetch('http://localhost:3016/we6/api/login', {
    method: 'POST',
    headers: {'Content-Type':'application/json'},
    body: JSON.stringify({a:12})
});
```

# 

# axios

[https://github.com/mzabriskie/axios](https://github.com/mzabriskie/axios)

也是一個可發request的套件

```
axios.post(API_HOST+'/api/Member/GetQAList',
     {
      LocalLang: "string",
      MessageNo: this.state.msgNo,
      Content: findDOMNode(this.refs.replyContent).value,
     }
    , {      
      withCredentials: true,
      headers: {
        accesstoken: localStorage.getItem('accessToken')
      } 
    })
    .then((response) => {
      sweetAlert(response.data.ResultMessage);
    })
    .catch(err => {
      console.log(err)
    })
```

如果要抓取error message要使用如下

```
  .catch(err => {
    console.log(err)
     if (err.response) {
       alert(err.response.data); 
     }
  })
```

[https://github.com/mzabriskie/axios/issues/376](https://github.com/mzabriskie/axios/issues/376)

#### \#Get 範例

```js
    axios.get('http://localhost:10001/test',{
        params: {
            ID: 12345
          },
        headers: {
          Authorization: 'token ' + localStorage.getItem('t')
        }
    })
    .then(function (response) {
      // console.log(response);
    })
    .catch(function (error) {
      console.log(error);
    });
```

#### \#POST x-www-form-urlencoded範例

```js
   import qs from 'querystring';

      axios.post('http://localhost:82/login',
      qs.stringify({
        values: 'test',
        lang: 'zh-CN',
        v: 1513153379508
      }))
        .then((response) => {
          console.log(response.data);
        })
        .catch(err => {
          console.log(err)
    })
```

# 注意:

如果是要發送cookie記得要加上 withCredentials: `true`

      axios({
        method,
        url: `${API_HOST}/${url}`,
        withCredentials: true,
        data: {
          method,
          data: payload,
          endpoint: url
        }
      })
      .then((response) => {
        console.log(response.data)
        callbackReduxAction(response.data);
      }).catch(err => {
        console.log(err);
      })

或是直接寫為default

```
axios.defaults.withCredentials = true;
```

然後server的Cross domain要設定

```
app.use('*', function (req, res, next) {
  res.header('Access-Control-Allow-Origin', 'http://localhost:3012');
  res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
  res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
  res.header('Access-Control-Allow-Credentials', 'true');
  next();
});
```

記得Origin不可用\*

不然會顯示如下訊息.

![](/assets/asca.png)

# \#瀏覽器跨域請求

因為瀏覽器發出的請求會被限制同網域  不像後端server或是chrome plugin可以對其他網域請求

所以我們可以用[https://crossorigin.me這類的proxy服務](https://crossorigin.me這類的proxy服務)

> 如果是要往自己的server發request可以參考JSONP或在server設定CORS
>
> 或是使用chrome相關plugin

```js
  fetch('https://crossorigin.me/http://google.com',{
           method: 'GET',
       })
       .then((response) => {
           if (response.status >= 200 && response.status < 300) {
               return response.text()
           } else {
               var error = new Error(response.statusText)
               error.response = response;
               throw error;
           }
       })
       .then((data) => {
         console.log(data)
       })
       .catch(function(error) {
           console.log('request failed', error);
           return error.response.json();
       })
```

# 下載檔案

記得加上

```js
responseType: "blob"
```

```js
axios.post('https://test/export/file', {},
      {
        responseType: "blob",
        headers: {
          Authorization: 'token ' + localStorage.getItem('t')
        }
      }).then(d => {
        console.log(d.data)
        var saveByteArray = (function () {
          var a = document.createElement("a");
          document.body.appendChild(a);
          a.style = "display: none";
          return function (data, name) {
            var blob = new Blob(data, { type: "application/xlsx" }),
              url = window.URL.createObjectURL(blob);
              console.log(url)
            a.href = url;
            a.download = name;
            a.click();
            window.URL.revokeObjectURL(url);
          };
        }());

        saveByteArray([d.data], 'example.xlsx');
      }).catch(err => { console.log(err) })
```



