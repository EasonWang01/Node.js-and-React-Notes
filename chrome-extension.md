# 重新整理extension

> 點選extension的重新整理按鈕，然後到網頁上重新整理頁面即可

# extension點擊後跳出專用畫面

有分browser\_action 和 page\_action

[https://stackoverflow.com/questions/44712495/what-are-the-differences-between-page-action-and-browser-action](https://stackoverflow.com/questions/44712495/what-are-the-differences-between-page-action-and-browser-action)

第一種方法：

manifest.json

```
    "browser_action": {
        "default_icon": {
            "16": "images/get_started16.png",
            "32": "images/get_started32.png",
            "48": "images/get_started48.png",
            "128": "images/get_started128.png"
        },
        "default_title": "Title",
        "default_popup": "popup.html"
    },
```

第二種方法：

manifest.json

```
    "page_action": {
        "default_popup": "popup.html",
        "default_icon": {
            "16": "images/get_started16.png",
            "32": "images/get_started32.png",
            "48": "images/get_started48.png",
            "128": "images/get_started128.png"
        }
    },
```

並且

background.js

```js
    chrome.declarativeContent.onPageChanged.removeRules(undefined, function() {
      chrome.declarativeContent.onPageChanged.addRules([{
        conditions: [new chrome.declarativeContent.PageStateMatcher({
          pageUrl: {hostEquals: 'localhost'},
        })
        ],
            actions: [new chrome.declarativeContent.ShowPageAction()]
      }]);
    });
```

# Inject script 到任何網頁

Manifest.json

> 將要執行的程式寫在`./src/js/inject.js`

```json
  "content_scripts": [
    {
      "matches": ["https://*/*", "http://*/*"],
      "js": ["./src/js/inject.js"]
    }
  ]
```

inject.js

```js
document.addEventListener('click', function(e){
   console.log(123)
 }, false);

 // 以上的console 會直接出現在頁面devtool上
```

# 從網頁傳送訊息到extension

manifest.json

```js
    "permissions": ["activeTab"],
    "background": {
      "scripts": ["background.js"],
      "persistent": false
    },
```

background.js

```js
  chrome.extension.onMessage.addListener(function(myMessage, sender, sendResponse){
    //do something that only the extension has privileges here
    console.log(myMessage)
    chrome.extension.getBackgroundPage().console.log(myMessage);
    return true;
 });
```

> 點擊以下檢視background 的 console.log![](/assets/螢幕快照 2019-05-15 上午11.31.45.png)

# 點擊Extension後觸發js

```js
chrome.tabs.executeScript({
  code: `console.log(123)`
})
```

要加permission

```js
"permissions": [
  "activeTab"
]
```

# 將chrome extension 保持開啟

> 作法為開一個新視窗

popup.js

```js
document.getElementById('open').addEventListener('click', () => {
    chrome.windows.create({
        url:"popup.html",
        type:"panel",
        width:300,
        height:200
    });
})
```

# 

# 取得所有頁面上的xhr事件回應

```
此與chrome extension無關
```

> 可參考以下：
>
> [https://stackoverflow.com/a/5202999/4622645](https://stackoverflow.com/a/5202999/4622645)
>
> [https://www.jianshu.com/p/7337ac624b8e](https://www.jianshu.com/p/7337ac624b8e)

使用Ajax-hook

> [https://github.com/wendux/Ajax-hook](https://github.com/wendux/Ajax-hook)

```js
!function (t) { function r(i) { if (n[i]) return n[i].exports; var o = n[i] = { exports: {}, id: i, loaded: !1 }; return t[i].call(o.exports, o, o.exports, r), o.loaded = !0, o.exports } var n = {}; return r.m = t, r.c = n, r.p = "", r(0) }([function (t, r, n) { n(1)(window) }, function (t, r) { t.exports = function (t) { t.hookAjax = function (t) { function r(t) { return function () { return this.hasOwnProperty(t + "_") ? this[t + "_"] : this.xhr[t] } } function n(r) { return function (n) { var i = this.xhr, o = this; return 0 != r.indexOf("on") ? void (this[r + "_"] = n) : void (t[r] ? i[r] = function () { t[r](o) || n.apply(i, arguments) } : i[r] = n) } } function i(r) { return function () { var n = [].slice.call(arguments); if (!t[r] || !t[r].call(this, n, this.xhr)) return this.xhr[r].apply(this.xhr, n) } } return window._ahrealxhr = window._ahrealxhr || XMLHttpRequest, XMLHttpRequest = function () { this.xhr = new window._ahrealxhr; for (var t in this.xhr) { var o = ""; try { o = typeof this.xhr[t] } catch (t) { } "function" === o ? this[t] = i(t) : Object.defineProperty(this, t, { get: r(t), set: n(t) }) } }, window._ahrealxhr }, t.unHookAjax = function () { window._ahrealxhr && (XMLHttpRequest = window._ahrealxhr), window._ahrealxhr = void 0 }, t.default = t } }]);

hookAjax({
  //hook callbacks
  onreadystatechange: function (xhr) {
    console.log("onreadystatechange called: %O", xhr)
  },
  onload: function (xhr) {
    console.log("onload called: %O", xhr)
  },
  //hook function
  open: function (arg, xhr) {
    console.log("open called: method:%s,url:%s,async:%s", arg[0], arg[1], arg[2])
  },
  send: function (arg) {
    console.log(arg)
  },
  setRequestHeader: function (arg) {
    console.log(arg)
  }
})
```



