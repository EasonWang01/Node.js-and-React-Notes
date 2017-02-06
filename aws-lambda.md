1.前往 AWS Lambda

2.選左上的`Create a Lambda function`，之後選擇左上的 `Blank Function`，再來先點選Next

3.填寫function名稱，runtime選擇Node.js

將code部分改為

```
exports.handler = function(event, context) {
  context.succeed("你好!");
};
```

4.拉到下面Role選單選擇`Create a custom role`然後會跳出一個視窗點選`Allow`，這樣Role即是`lambda_basic_execution`

5.點選Next並建立，之後可點擊`Test`

