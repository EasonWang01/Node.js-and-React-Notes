# Git hook

## Pre commit hook

每次 commit 時會自動執行。例如我們想要每次 git commit 在 package.json version 自動加 0.0.1

可以如下新增 `setup-pre-commit.sh`：

> 記得先 `brew install jq`

```bash
#!/bin/sh

# setup-pre-commit.sh

# Path to the pre-commit hook
HOOK_PATH=".git/hooks/pre-commit"

# Pre-commit hook script
HOOK_SCRIPT="#!/bin/sh\n\n# Increment version in package.json using jq\nVERSION=\$(jq -r '.version' package.json)\nNEW_VERSION=\$(echo \$VERSION | awk -F. -v OFS=. '{\$NF = \$NF + 1 ; print}')\n\n# Update the version in package.json\njq \".version = \\\"\$NEW_VERSION\\\"\" package.json > temp.json && mv temp.json package.json\n\n# Add the package.json to the commit\ngit add package.json\n\n# Exit successfully\nexit 0"

# Create the pre-commit hook
echo "$HOOK_SCRIPT" > "$HOOK_PATH"

# Make the pre-commit hook executable
chmod +x "$HOOK_PATH"

echo "Pre-commit hook set up successfully."
```

之後執行 sh ./`setup-pre-commit.sh`

會自動把以上 HOOK\_SCRIPT 腳本 加入到 ./git/hooks/pre-commit 檔案，每次 commit 前就會執行  pre-commit 檔案內容。

## Git Hooks

[https://git-scm.com/book/zh-tw/v1/Git-客製化-Git-Hooks](https://git-scm.com/book/zh-tw/v1/Git-%E5%AE%A2%E8%A3%BD%E5%8C%96-Git-Hooks)

在git的專案的目錄下有一個隱藏資料夾為`.git`

進入後可看到如下![](https://github.com/easonwang01/web\_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/sdfsdf.png)

進入到hooks資料夾可看到一些包含

```
pre-commit 
post-commit
```

我們先把pre-commit.sample檔案名稱改為pre-commit

然後在裡面加上

> 使用第一行的#!決定要用哪一種方式解譯，如果更改檔案副檔名則無效

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

## Link hook file

> 但之後改完因為.git無法加入commit，我們必須使用link file的方式讓別人執行，然後加入到git hook

新增如下檔案結構

![](https://github.com/easonwang01/web\_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/asx.png)

link.js

```javascript
var fs = require("fs");
var path = require("path");

["applypatch-msg", "commit-msg", "post-commit", "post-receive", "post-update", "pre-applypatch", "pre-commit",
 "prepare-commit-msg", "pre-rebase", "update"].forEach(function (hook) {
    var hookInSourceControl = path.resolve(__dirname, hook);

    if (fs.existsSync(hookInSourceControl)) {
        var hookInHiddenDirectory = path.resolve(__dirname, "../..", ".git", "hooks", hook);

        if (fs.existsSync(hookInHiddenDirectory)) {
            fs.unlinkSync(hookInHiddenDirectory);
        }

        fs.linkSync(hookInSourceControl, hookInHiddenDirectory);
    }
});
```

pre-commit

```javascript
#!/usr/bin/env node

const { exec } = require('child_process');


// 如果修改的檔案為/dist 會要求修改/js 並使用babel compile
exec('git diff --cached --name-only', (error, stdout, stderr) => {
  if (error) {
    console.error(`exec error: ${error}`);
    return;
  }

  if(stdout.indexOf('/dist') !== -1) { // 修改了dist資料夾內的檔案
      console.log(`您修改了包含/dist檔案內的資料，所以被禁止commit!!`)
      console.log(stdout);
      process.exit(1);
  }
});
```

之後執行該link.js檔案即可讓兩隻檔案連結起來，互相複製內容，改動其中一隻另外一個檔案也會被改變。

其他可參考:

[https://github.com/zwhu/blog/issues/31](https://github.com/zwhu/blog/issues/31)

[https://gist.github.com/domenic/2238951](https://gist.github.com/domenic/2238951)

第三方模組:

[https://github.com/typicode/husky](https://github.com/typicode/husky)
