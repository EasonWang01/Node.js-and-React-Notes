# \#跨平台測試

[https://saucelabs.com/beta/dashboard/manual](https://saucelabs.com/beta/dashboard/manual)

[https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Automated\_testing](https://developer.mozilla.org/zh-TW/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing)

[https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Your\_own\_automation\_environment](https://developer.mozilla.org/zh-TW/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment)

# 

# \#[GoogleChrome](https://github.com/GoogleChrome)/[**puppeteer**](https://github.com/GoogleChrome/puppeteer)

安裝:[https://github.com/GoogleChrome/puppeteer](https://github.com/GoogleChrome/puppeteer)

文件:[https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md\#pageclickselector-options](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#pageclickselector-options)

安裝時他會安裝最新版本的Chromium 所以安裝會比較久

> 記得加上headless: false 才會打開瀏覽器

```
const browser = await puppeteer.launch({headless: false});
```

Example:

```js
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

# 

# \#WebDriver.io

[http://webdriver.io/](http://webdriver.io/)

# \#selenium-webdriver

[https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver](https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver)

# \#nightmare

\(使用electron當作介面\)

[https://github.com/segmentio/nightmare](https://github.com/segmentio/nightmare)



> 異步需要參考如下:

https://github.com/rosshinkley/nightmare-examples/blob/master/docs/common-pitfalls/async-operations-loops.md

```js
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

# \#phantomjs

[http://phantomjs.org/quick-start.html](http://phantomjs.org/quick-start.html)

# \#casper.js

[http://casperjs.org/](http://casperjs.org/)

