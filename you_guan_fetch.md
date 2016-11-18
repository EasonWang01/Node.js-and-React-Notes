# 有關Fetch

注意事項:
1.使用json()轉換
2.第二個then才拿得到資料，第一個then只是一個promise結果

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