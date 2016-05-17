# repl


test1.js
```
var repl = require("repl");
var fs = require('fs');

var replServer = repl.start({
  prompt: "my-app > ",
});




replServer.context.foo =  function(){
    fs.mkdir('./helloDir',0777, function (err) {
    if (err) throw err;
  });
}
```

之後執行

```
node test1
```

再來輸入

```
foo()
```

即可發現創造了一個helloDir的資料夾