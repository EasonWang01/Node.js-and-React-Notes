# 有關Fetch

以前用xmlHttprequest但寫法太多，fetch為比較簡潔的寫法，並且有then可使用

注意事項:

1.使用json\(\)轉換或是其他型態轉換https://developer.mozilla.org/en-US/docs/Web/API/Body

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

# 注意:

如果是要發送cookie記得要加上 withCredentials: true` `

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

