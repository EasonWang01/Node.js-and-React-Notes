# Git Hooks

[https://git-scm.com/book/zh-tw/v1/Git-客製化-Git-Hooks](https://git-scm.com/book/zh-tw/v1/Git-客製化-Git-Hooks)

在git的專案的目錄下有一個隱藏資料夾為`.git`

進入後可看到如下![](/assets/sdfsdf.png)

進入到hooks資料夾可看到一些包含

```
pre-commit 
post-commit
```

我們先把pre-commit.sample檔案名稱改為pre-commit

然後在裡面加上

> 使用第一行的\#!決定要用哪一種方式解譯，如果更改檔案副檔名則無效

```
#!/usr/bin/env node

console.log(6678);

// process.exit(1); 加上後會取消commit
```

之後commit時就會執行

如果要寫shell script可以改為

```
#!/bin/sh

echo 123;

# exit 1;  加上後會取消commit
```

其他可參考:

[https://github.com/zwhu/blog/issues/31](https://github.com/zwhu/blog/issues/31)

[https://gist.github.com/domenic/2238951](https://gist.github.com/domenic/2238951)

第三方模組:

[https://github.com/typicode/husky](https://github.com/typicode/husky)

