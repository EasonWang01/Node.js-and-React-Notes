# create-react-app



## Create-react-app

> 為一個快速建立React 模板的工具

### 更改預設PORT

1.新增 `.env`

```text
PORT=8002
```

2.之後啟動後會自動執行在8002 PORT

3.如果要讓Node.js 檔案也可以讀.env檔案可以使用以下模組

```text
yarn add dotenv
```

然後

```text
require('dotenv').load();
console.log(process.env)
```

## 設定多個.env

創建一個目錄結構如下

![](/assets/d32.png)

將env.js改為如下

```javascript
'use strict';

const fs = require('fs');
const path = require('path');
const paths = require('./paths');

// Make sure that including paths.js after env.js will read .env variables.
delete require.cache[require.resolve('./paths')];

const NODE_ENV = process.env.NODE_ENV;
if (!NODE_ENV) {
  throw new Error(
    'The NODE_ENV environment variable is required but was not specified.'
  );
}

// https://github.com/bkeepers/dotenv#what-other-env-files-can-i-use
var dotenvFiles = [
  path.resolve(process.cwd(), `./env/${process.env.stage}/.env`),
  `${paths.dotenv}.${NODE_ENV}.local`,
  `${paths.dotenv}.${NODE_ENV}`,
  // Don't include `.env.local` for `test` environment
  // since normally you expect tests to produce the same
  // results for everyone
  NODE_ENV !== 'test' && `${paths.dotenv}.local`,
  paths.dotenv,
].filter(Boolean);

// Load environment variables from .env* files. Suppress warnings using silent
// if this file is missing. dotenv will never modify any environment variables
// that have already been set.  Variable expansion is supported in .env files.
// https://github.com/motdotla/dotenv
// https://github.com/motdotla/dotenv-expand
dotenvFiles.forEach(dotenvFile => {
  if (fs.existsSync(dotenvFile)) {
    require('dotenv-expand')(
      require('dotenv').config({
        path: dotenvFile,
      })
    );
  }
});

// We support resolving modules according to `NODE_PATH`.
// This lets you use absolute paths in imports inside large monorepos:
// https://github.com/facebookincubator/create-react-app/issues/253.
// It works similar to `NODE_PATH` in Node itself:
// https://nodejs.org/api/modules.html#modules_loading_from_the_global_folders
// Note that unlike in Node, only *relative* paths from `NODE_PATH` are honored.
// Otherwise, we risk importing Node.js core modules into an app instead of Webpack shims.
// https://github.com/facebookincubator/create-react-app/issues/1023#issuecomment-265344421
// We also resolve them to make sure all tools using them work consistently.
const appDirectory = fs.realpathSync(process.cwd());
process.env.NODE_PATH = (process.env.NODE_PATH || '')
  .split(path.delimiter)
  .filter(folder => folder && !path.isAbsolute(folder))
  .map(folder => path.resolve(appDirectory, folder))
  .join(path.delimiter);

// Grab NODE_ENV and REACT_APP_* environment variables and prepare them to be
// injected into the application via DefinePlugin in Webpack configuration.
const REACT_APP = /^REACT_APP_/i;

function getClientEnvironment(publicUrl) {
  const raw = Object.keys(process.env)
    .filter(key => REACT_APP.test(key))
    .reduce(
      (env, key) => {
        env[key] = process.env[key];
        return env;
      },
      Object.assign({
        NODE_ENV: process.env.NODE_ENV || 'development',
        PUBLIC_URL: publicUrl
      } ,process.env)
    );
  // Stringify all values so we can feed into Webpack DefinePlugin
  const stringified = {
    'process.env': Object.keys(raw).reduce((env, key) => {
      env[key] = JSON.stringify(raw[key]);
      return env;
    }, {}),
  };

  return { raw, stringified };
}

module.exports = getClientEnvironment;
```

其中以下部分可以下後端的process.env傳到前端。

```javascript
Object.assign({
        NODE_ENV: process.env.NODE_ENV || 'development',
        PUBLIC_URL: publicUrl
      } ,process.env)
```

package.json中的 script 如下

```javascript
"scripts": {
    "dev": "cross-env stage=dev concurrently --kill-others \"yarn start\" \"yarn runProxy\"",
    "sit": "cross-env stage=sit concurrently --kill-others \"yarn start\" \"yarn runProxy\""
  },
```

## Adding typescript

```text
yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

