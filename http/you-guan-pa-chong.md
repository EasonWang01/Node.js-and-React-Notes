# 有關爬蟲

簡單範例：

> 爬完後存到檔案

```javascript
const http = require("http");
const fs = require("fs");
const iconv = require("iconv-lite");
const querystring = require("querystring");
const cheerio = require("cheerio");
const filepath = "./data.csv";

let reqPageNum = 0;
let totalPageCount = 18;

function sendRequest(pageNum) {
  const requestBody = {
    PG2: `&#160;${pageNum}&#160;`,
    PgNo: pageNum,
    s: 0,
  };
  const postData = querystring.stringify(requestBody);

  const options = {
    hostname: "lotto.bestshop.com.tw",
    path: "/649/where.asp",
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
      "Content-Length": Buffer.byteLength(postData),
    },
  };

  // 因為是要存 buffer 必須用 array
  const result = [];
  const req = http.request(options, (res) => {
    res.on("data", (chunk) => {
      result.push(chunk);
    });
    res.on("end", () => {
      const resp = iconv.decode(Buffer.concat(result), "big5");
      parseDom(resp);
    });
  });
  req.on("error", (e) => {
    console.error(`problem with request: ${e.message}`);
  });
  req.write(postData);
  req.end();
}

function parseDom(dom) {
  let count = 0;
  $ = cheerio.load(dom);
  $(".TDLine1").each((i, ele) => {
    if (count === 8 || count === 0) {
      fs.appendFileSync(filepath, "\r\n");
      count = 0;
    }
    const content = $(ele).text();
    fs.appendFileSync(filepath, `${count !== 0 ? "," : ""}${content}`);
    count += 1;
  });
  if (reqPageNum < totalPageCount) {
    reqPageNum += 1;
    sendRequest(reqPageNum);
  }
}

// Create empty file
fs.writeFile(filepath, "", { flag: "wx" }, function (err) {
  if (err) throw err;
});
sendRequest(reqPageNum);
```

