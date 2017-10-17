# \#跨平台測試

[https://saucelabs.com/beta/dashboard/manual](https://saucelabs.com/beta/dashboard/manual)

[https://developer.mozilla.org/zh-TW/docs/Learn/Tools\_and\_testing/Cross\_browser\_testing/Automated\_testing](https://developer.mozilla.org/zh-TW/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing)

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

http://webdriver.io/

# \#nightmare

[https://github.com/segmentio/nightmare](https://github.com/segmentio/nightmare)

# \#phantomjs

[http://phantomjs.org/quick-start.html](http://phantomjs.org/quick-start.html)

# \#casper.js

[http://casperjs.org/](http://casperjs.org/)

