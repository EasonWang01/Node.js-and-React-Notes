# 有關Fetch

以前用xmlHttprequest但寫法太多，fetch為比較簡潔的寫法，並且有then可使用

注意事項:

1.使用json()轉換

2.第二個then才拿得到資料，第一個then只是一個promise結果

3.cookie要手動在header加入(第二個參數)

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

https://www.reddit.com/r/learnprogramming/comments/3ydnmn/javascriptnodejswhatwgfetch_why_does_this_return/



#axios

https://github.com/mzabriskie/axios

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
       alert(err.response.data.message); 
     }
  })
```

https://github.com/mzabriskie/axios/issues/376