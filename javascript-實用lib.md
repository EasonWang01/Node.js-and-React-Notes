# 1.Axios {#axios}


1.簡單的restful 請求工具

ex:
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

注意：
```
1.axios會自動帶入cookie 
2.response要用data取出 
3.用在server端請求要帶上域名`axios.post('http://....')`
4.react server side render 時componentWIllMount等會先在server跑一次的也要加上域名`axios.post('http://....')`
 
 ```