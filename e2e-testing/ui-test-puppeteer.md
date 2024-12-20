---
description: 測試工具
---

# Puppeteer 與其他 UI 測試工具

## 跨平台測試

[https://saucelabs.com/beta/dashboard/manual](https://saucelabs.com/beta/dashboard/manual)

[https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Automated\_testing](https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Automated\_testing)

[https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Your\_own\_automation\_environment](https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Your\_own\_automation\_environment)

## [GoogleChrome](https://github.com/GoogleChrome) / [**puppeteer**](https://github.com/GoogleChrome/puppeteer)

安裝:&#x20;

[https://github.com/GoogleChrome/puppeteer](https://github.com/GoogleChrome/puppeteer)

文件:[https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#pageclickselector-options](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#pageclickselector-options)

安裝時他會安裝最新版本的Chromium 所以安裝會比較久

> 記得加上headless: false 才會打開瀏覽器

```
const browser = await puppeteer.launch({headless: false});
```

Example:

```javascript
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch({ headless: false });
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.goto('https://google.com')
  const hg = await page.$eval('.gb_P', el => el.innerHTML);
  console.log(hg)
  await page.type('#lst-ib', 'Hello', {delay: 100});
  await page.click('#gsri_ok0')
  await page.screenshot({ path: 'example.png' });
})();
```

### 連線到已經開啟的chrome

因為預設程式結束時chrome也會關閉，所以要用connect的方法。

使用以下方式開啟chrome

```
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --no-first-run --no-default-browser-check --user-data-dir=$(mktemp -d -t 'chrome-remote_data_dir')
```

之後將以下擷取下來之後用如下方式

```javascript
const puppeteer = require('puppeteer');
const browserWSEndpoint = 'ws://127.0.0.1:9222/devtools/browser/15d59f55-f1c9-4c95-89c9-3f164988ba58';
(async () => {
    const browser = await puppeteer.connect({
        browserWSEndpoint,
    });
    const page = (await browser.pages())[0]; //存取第一個 tab
    let page = 'https://sakatu.com/';

    await page.goto(pageUrl, {
        waitUntil: 'networkidle0'
    });
})();
```

### 將 dom 傳送到 nodejs 端處理

> 在 evaluate 後 return 出來即可

```javascript

const puppeteer = require("puppeteer");
puppeteer.launch({ headless: false }).then(function (browser) {
  browser.newPage().then(function (page) {
    page.goto("http://www.cwl.gov.cn/kjxx/ssq/kjgg/").then(async function () {
      const pageData = await page.evaluate(async () => {
        function delay(time) {
          return new Promise(function (resolve) {
            setTimeout(resolve, time);
          });
        }
        document.querySelector(".zdy").click(); //2013001 // 2020120
        document.querySelectorAll("Strong")[1].click();
        document.querySelector(".inpQa").value = "2013001";
        document.querySelector(".inpQz").value = "2020120";
        document.querySelector(".anKs.aQzQ").click();
        await delay(500);
        return document.querySelector("body").innerHTML
      });
      console.log(pageData)
    });
  });
});
```

{% embed url="https://stackoverflow.com/questions/46202985/getting-dom-node-text-with-puppeteer-and-headless-chrome" %}

### 傳遞參數到 evaluate

> 因為 evaluate 預設執行環境是瀏覽器，變數抓不到 node 的

```javascript
let name = 'test';

await page.evaluate(({name}) => {

    console.log(name);
    console.log(age);
    console.log(location);

},{name});
```

[https://stackoverflow.com/a/47598159/4622645](https://stackoverflow.com/a/47598159/4622645)

傳遞 function

```javascript
await page.exposeFunction("myFunc", myFunc);
```

[https://stackoverflow.com/a/61336937/4622645](https://stackoverflow.com/a/61336937/4622645)

## Puppeteer-firefox

[https://github.com/GoogleChrome/puppeteer/tree/master/experimental/puppeteer-firefox](https://github.com/GoogleChrome/puppeteer/tree/master/experimental/puppeteer-firefox)goo

## WebDriver.io

[http://webdriver.io/](http://webdriver.io/)

## selenium-webdriver

[https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver](https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver)

## nightmare

(使用electron當作介面)

[https://github.com/segmentio/nightmare](https://github.com/segmentio/nightmare)

> 異步需要參考如下:

[https://github.com/rosshinkley/nightmare-examples/blob/master/docs/common-pitfalls/async-operations-loops.md](https://github.com/rosshinkley/nightmare-examples/blob/master/docs/common-pitfalls/async-operations-loops.md)

```javascript
var Nightmare = require('nightmare'),
  nightmare = Nightmare({
    show: true
  });

nightmare
  //load a url
  .goto('http://localhost:5081')
  //simulate typing into an element identified by a CSS selector
  //here, Nightmare is typing into the search bar
  .type('#username', 'daniel06')
  .type('#password', 'qwe123')
  .click('#loginBTN')
  .wait(1000)
  .click('#agree_btn')

  function runplay(nightmare, gametype){
    nightmare
    .wait(1000)
    .click(gametype)
    .wait(1500)
    .click('#auto_select')
    .wait(1000)
    .click('#bet_btn')
    .wait(1000)
    .click('#ok_btn')
    .wait(1500)
    .click('#other_btn')
  }

    runplay(nightmare, '#UUFFC');
    runplay(nightmare, '#UUSSC');
    runplay(nightmare, '#UU11X5');
    runplay(nightmare, '#TCP3P5');
    runplay(nightmare, '#JSK3');
  nightmare
  //end the Nightmare instance along with the Electron instance it wraps

  //run the queue of commands specified
  .run(function(error, result) {
    if (error) {
      console.error(error);
    } else {
      console.log(result);
    }
  });
```

mocha + nightmare範例

```javascript
import Nightmare from 'nightmare';
import {expect} from 'chai';

describe('test duckduckgo search results', () => {
  it('should find the nightmare github link first', (done) => {
    const nightmare = Nightmare()
    nightmare
      .goto('https://duckduckgo.com')
      .type('#search_form_input_homepage', 'github nightmare')
      .click('#search_button_homepage')
      .wait('#zero_click_wrapper .c-info__title a')
      .evaluate(() =>
        document.querySelector('#zero_click_wrapper .c-info__title a').href
      )
      .end()
      .then((link) => {
        expect(link).to.equal('https://github.com/segmentio/nightmare');
        done();
      })
  });
});
```

[https://segment.com/blog/ui-testing-with-nightmare/](https://segment.com/blog/ui-testing-with-nightmare/)

## phantomjs

[http://phantomjs.org/quick-start.html](http://phantomjs.org/quick-start.html)

## casper.js

[http://casperjs.org/](http://casperjs.org/)
