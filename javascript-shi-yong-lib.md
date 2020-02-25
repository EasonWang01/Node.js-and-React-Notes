# Javascript 實用Lib

## 1.Axios <a id="axios"></a>

1.簡單的restful 請求工具

ex:

```text
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

```text
1.axios會自動帶入cookie 
2.response要用data取出 
3.用在server端請求要帶上域名`axios.post('http://....')`
4.react server side render 時componentWIllMount等會先在server跑一次的也要加上域名`axios.post('http://....')`
5.如果跨域要加上`{withCredentials: true})//因為是跨域，所以要設定才能從res.cookie設定cookie`
```

## 2.監控

[https://github.com/getsentry/sentry](https://github.com/getsentry/sentry)

## 3.螢幕截圖 錄影

[https://github.com/mgechev/jscapture/blob/master/src/index.js](https://github.com/mgechev/jscapture/blob/master/src/index.js)

[https://github.com/spite/ccapture.js/](https://github.com/spite/ccapture.js/)

[https://github.com/Huddle/Resemble.js](https://github.com/Huddle/Resemble.js) \(圖片比較\)

[https://github.com/Huddle/PhantomCSS](https://github.com/Huddle/PhantomCSS) \(css render比較測試\)

## 4.表單檢驗

[https://github.com/mailcheck/mailcheck](https://github.com/mailcheck/mailcheck) \(mail提示\)

## 5.測試

[https://github.com/visionmedia/supertest](https://github.com/visionmedia/supertest) \(http測試\)

