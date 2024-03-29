# create-react-app

## Create-react-app

> 為一個快速建立React 模板的工具

### 更改預設PORT

1.新增 `.env`

```
PORT=8002
```

2.之後啟動後會自動執行在8002 PORT

3.如果要讓Node.js 檔案也可以讀.env檔案可以使用以下模組

```
yarn add dotenv
```

然後

```
require('dotenv').load();
console.log(process.env)
```

## 設定多個.env

創建一個目錄結構如下

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

```
yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

之後把 App.js to App.tsx

然後重新啟動

### EXAMPLE:

```javascript
export interface Props {
  name: string;
}

const PrintName: React.FC<Props> = (props) => {
  return (
    <div>
      <p>{props.name}</p>
    </div>
  )
}

function App() {
  return (
    <div className="App">
      <PrintName name="test" />
    </div>
  );
}

export default App;
```

## 修改 Webpack config

不 eject 的情況可以使用：

{% embed url="https://github.com/timarney/react-app-rewired" %}

1.安裝 react-app-rewire

```
yarn add 
```

2\. 新增 config-overrides.js，類似如下

```javascript
const {alias} = require('react-app-rewire-alias')

module.exports = function override(config) {
  return alias({
    '~common': '../common',
  })(config)
}
```

3\. 修改 package.json

```
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    ....
   } 
```

## 在 CRA 使用 Crypto, Path, Stream 等 Node.js 模組

[https://github.com/facebook/create-react-app/issues/11756#issuecomment-1378461646](https://github.com/facebook/create-react-app/issues/11756#issuecomment-1378461646)

CRA 後續版本因為使用 webpack ModuleScopePlugin 插件所以無法引入 craco 這類型覆蓋 webpack config 設置 module resolve 的從引 src 外入 module，可以配置如下。

```
yarn add @craco/craco crypto-browserify path-browserify stream-browserify -D
```

craco.config.js 範例配置

```javascript
module.exports = {
  webpack: {
    configure: webpackConfig => {
      const scopePluginIndex = webpackConfig.resolve.plugins.findIndex(
        ({ constructor }) => constructor && constructor.name === 'ModuleScopePlugin'
      );

      webpackConfig.resolve.plugins.splice(scopePluginIndex, 1);
      webpackConfig['resolve'] = {
        fallback: {
          path: require.resolve("path-browserify"),
          crypto: require.resolve("crypto-browserify"),
          stream: require.resolve("stream-browserify"),
        },
      }
      return webpackConfig;
    },
  },
};
```

> 其他 polyfill 模組包含
>
> ```
>  npm install --save-dev crypto-browserify stream-browserify assert stream-http https-browserify os-browserify url buffer process
> ```
