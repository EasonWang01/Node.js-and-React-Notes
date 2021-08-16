# REPL

讀取 Terminal 指令：

```javascript
const readline = require("readline");
const repl = readline.createInterface(process.stdin, process.stdout);
repl.on("line", (text) => {
  console.log(text);
});
```

