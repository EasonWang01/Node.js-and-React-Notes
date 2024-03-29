# js trick

## js trick

## Sort Array of number 時包含 NaN

因為 NaN, Null 沒辦法被 sort&#x20;

```javascript
[1, NaN, 3, 5].sort((a, b) => b - a)
// [1, NaN, 5, 3]
```

所以可以用如下方式

```javascript
function handleSort(eleA, eleB) {
  if(isNaN(eleB)) {
    eleB[1] = order === sortOrder.Ascend ? Number.MAX_SAFE_INTEGER : 0;
  }
  return order === sortOrder.Descend ? eleB - eleA : eleA - eleB
}

[....].sort(handleSort)
```

## 1.替換input type＝"file"標籤為客製化按鈕

因為label規定不可放button在內，所以我們使用click()的方法，如下

```
<div>
<button onClick={() => this.fileBtn()} style={style.picBtn} />
<input style={style.fileInput} id="file-upload" ref="fileInput" type='file' />
</div>
```

```
   fileBtn() {
    findDOMNode(this.refs.fileInput).click();
  }
```

style

```
   picBtn: {
    background: 'url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDgiIGhlaWdodD0iNDgiIHZpZXdCb3g9IjAgMCA0OCA0OCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48dGl0bGU+aWNfcGljXzhmOGY4ZjwvdGl0bGU+PGcgZmlsbD0iIzhGOEY4RiIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNMTUuOTg4IDEyYTQgNCAwIDEgMCAwIDggNCA0IDAgMCAwIDAtOCIvPjxwYXRoIGQ9Ik00MC4wNjggMjQuNzA0bC02LjM3My01LjkyOC05LjM1IDkuNTc1LTUuNDUyLTUuNTg2TDggMzMuMzQyVjE0LjAwNUE2LjAxMiA2LjAxMiAwIDAgMSAxNC4wMDUgOGgyMC4wNThhNi4wMTIgNi4wMTIgMCAwIDEgNi4wMDUgNi4wMDV2MTAuNjk5ek0zNC4wNjMgNkgxNC4wMDVBOC4wMDkgOC4wMDkgMCAwIDAgNiAxNC4wMDV2MjAuMDU4YTguMDA5IDguMDA5IDAgMCAwIDguMDA1IDguMDA1aDIwLjA1OGE4LjAwOSA4LjAwOSAwIDAgMCA4LjAwNS04LjAwNVYxNC4wMDVBOC4wMDkgOC4wMDkgMCAwIDAgMzQuMDYzIDZ6Ii8+PC9nPjwvc3ZnPg==) no-repeat',
    backgroundSize: 'cover',
    backgroundPosition: '50%',
    height: '22px',
    width: '22px',
    border: 'none',
    cursor: 'pointer',
    display: 'block',
    outline: 'none'
  },
    fileInput: {
    display: 'none'
  }
```

## 2.轉為boolean

`!!`

## 3.除與二的次方

`>>>`

## 4.複製到剪貼版

```javascript
<input id="pageHideInput"/> 
//新增一個隱藏的Input，但記得如果使用display:none會無法使用select() 所以建議把它用absolute然後width:0px; height: 0px; top: -1200px
<div id="textToCopy"></div> 

var v = document.getElementById('textToCopy').innerHTML;
document.getElementById('pageHideInput').setAttribute('value', v);
document.getElementById('pageHideInput').select();
document.execCommand('copy');
document.getElementById('pageHideInput').blur(); //避免手機彈起鍵盤
```

但ios有的會有一些問題，所以建議可用[https://clipboardjs.com/](https://clipboardjs.com/)

範例:

[https://codepen.io/anon/pen/EbJBjP](https://codepen.io/anon/pen/EbJBjP)

```
記得clipboard.js的new Clipboard參數要填入id或是class，例如: new Clipboard('#' + ele.id);
```

## 倒數計時

```
<div id="timer"></div>
```

```javascript
/**
 * 設定畫面倒數計時
 * 
 * @ element 綁定畫面元素ID
 * @ time 倒數分鐘數
 * @ callback {Function} 倒數結束後的執行Function
 */
function setUITimer(element, time, callback) {
  var countDownDate = new Date().getTime() + 1000 * 60 * time;
  if(window.scan_wc_timer) {
    clearInterval(window.scan_wc_timer);
    document.getElementById('timer').innerHTML = "05:00"
  }
  window.scan_wc_timer = setInterval(function () {
    // Get todays date and time
    var now = new Date().getTime();
    // Find the distance between now an the count down date
    var distance = countDownDate - now;
    // Time calculations for days, hours, minutes and seconds
    var days = Math.floor(distance / (1000 * 60 * 60 * 24));
    var hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
    var seconds = Math.floor((distance % (1000 * 60)) / 1000);
    seconds.toString().length > 1 ? seconds = seconds : seconds = '0' + seconds

    document.getElementById(element).innerHTML = "0" + minutes + ":" + seconds;


    if (distance < 0) {
      clearInterval(window.scan_wc_timer);
      document.getElementById('timer').innerHTML = "05:00"
      callback();
    }
  }, 300);
}
```

版本2

```javascript
  function timer(intDiff) {

      var intDiff = parseInt(intDiff);
      var day = 0,
        hour = 0,
        minute = 0,
        second = 0;


      window.setInterval(function () {

        if (intDiff > 0) {
          day = Math.floor(intDiff / 86400);
          hour = Math.floor(intDiff / 3600) - (day * 24);
          minute = Math.floor(intDiff / 60) - (day * 24 * 60) - (hour * 60);
          second = Math.floor(intDiff) - (day * 24 * 60 * 60) - (hour * 60 * 60) - (minute * 60);
        }
        console.log(hour)
        if (hour <= 9) hour = "0" + hour;
        if (minute <= 9) minute = "0" + minute;
        if (second <= 9) second = "0" + second;
        console.log(" " + day + ":" + hour + ":" + minute + ":" + second + "");
        intDiff--;
      }, 1000);
      // alert("The language is: " + userLang);
    }
    timer(200000)
```

如果想設定倒數結束的開始時間：

```javascript
// 結束日期為2018, 5, 22 要用5-1的原因是JS date的一月是0
let elapsed = Math.floor(Date.now() / 1000) - Math.floor(new Date(2018, 5 - 1, 20).getTime() / 1000);
timer(60 * 60 * 24 - elapsed);
```

## 角子老虎機捲動效果

[https://codepen.io/EasonWang01/pen/VxNzXz](https://codepen.io/EasonWang01/pen/VxNzXz)

主要是創造兩階層的0-9個數字，然後在0接到9時位置跳回開頭。

```javascript
  componentDidMount() {
    var c = "";
    for (let i = 0; i <= 8; i++) {
      c = c + this.htmlstrFun(`c_0${i}`)
    }
    document.getElementById('money').insertAdjacentHTML('beforeend', c);
    this.runPrize("564787263")
  }

  runPrize(target) {
    setTimeout(() => {
      for (let i = 0; i <= 8; i++) {
        this.runNum(1 / (i + 1) * 20, i, target)
      }
    }, 1000)
  }

  runNum(fe, index, target) {
    // fe代表運轉的速度，index為數字的位置，target為目標數字。
    let gap = document.querySelector('.bonus_money li').clientHeight;
    let position = 0;
    var v = setInterval(() => {
      if (position === -(gap * 10) - (gap * target[index]) && index == runIndex) {
        runIndex -= 1;
        clearInterval(v);
        return
      }
      if (position === -(gap * 19) + Math.floor(gap / 2)) {
        position = -(gap * 9) + Math.floor(gap / 2)
      } else {
        position -= 1;
      }
      document.querySelector(`#c_0${index}`).style.top = `${position}px`;
    }, fe)
  }
```

## 移除畫面上的事件綁定

有時如果畫面更新但元素沒更新時，造成重複綁定事件，可用以下方法更新元素，移除舊事件。

```javascript
var old_element = document.getElementById("btn");
var new_element = old_element.cloneNode(true);
old_element.parentNode.replaceChild(new_element, old_element);
```

例如

```javascript
element.addEventListener('click', function () {
  scanPayDepositApplication(element)
  var old_element = element;
  var new_element = old_element.cloneNode(true);
  old_element.parentNode.replaceChild(new_element, old_element);    
})
```

[https://stackoverflow.com/questions/9251837/how-to-remove-all-listeners-in-an-element](https://stackoverflow.com/questions/9251837/how-to-remove-all-listeners-in-an-element)

## 取得所有可能的字串組合

假設四個字母組成字串，要印出所有可能組合

> 4 \*\* 4 = 256 種組合

```javascript
c = []
ca = ["A", "B", "C", "D"];
for(let i = 0; i < 4; i ++){
  c[0] = ca[i]
  for(let i = 0; i < 4; i ++){
    c[1] = ca[i]
    for(let i = 0; i < 4; i ++){
      c[2] = ca[i]
       for(let i = 0; i < 4; i ++){
          c[3] = ca[i]
          console.log(c)
        }
    }
  }
}
```

## 解析QueryString

先使用location.search取得QueryString

```javascript
function parseQuerystring(s) {
  var urlParams = {};
  var match,
    pl = /\+/g, // Regex for replacing addition symbol with a space
    search = /([^&=]+)=?([^&]*)/g,
    decode = function(s) {
      return decodeURIComponent(s.replace(pl, " "));
    },
    query = s.substring(1);
  urlParams = {};
  while ((match = search.exec(query)))
    urlParams[decode(match[1])] = decode(match[2]);
  return urlParams;
}
```

使用

```
parseQuerystring(location.search)
```

> 或是可使用官方ＡＰＩ
>
> ```javascript
> var urlParams = new URLSearchParams(window.location.search);
>
> console.log(urlParams.has('post')); // true
> ```

### Promise cancel

拋出error當timeout，中斷所有其他promise

```javascript
function timeoutPromise(delay) {
    return new Promise(function(resolve, reject) {
      setTimeout(function() {
        throw new Error()
      }, delay);
    });
  }

var winnerPromise = new Promise(function (resolve) {
    setTimeout(function () {
        resolve('this is winner');
    }, 4000);
});
var loserPromise = new Promise(function (resolve) {
    setTimeout(function () {
        resolve('this is loser');
    }, 5000);
});
Promise.race([winnerPromise, loserPromise, timeoutPromise(1000)]).then(function (value) {
    console.log(value); 
}, function(err) {
  console.log(err)
});
```

## Closure

```javascript
a = ['s', 'ss','sss']; obj = {}

function setValue(obj, a) {
  if (a.length > 0) {
    var n = a.shift()
    if (!(n in obj)) obj[n] = {}
    obj = obj[n]
    setValue(obj, a)
  }
}

setValue(obj, a)
console.log(obj)
```

上面將等同於 (上面較易理解) 來源：[https://stackoverflow.com/a/20240290](https://stackoverflow.com/a/20240290)

```javascript
a = ['s', 'ss','sss']; obj = {}

function setValue(obj, a) {
  var o = obj
  while (a.length - 0) {
    var n = a.shift()
    if (!(n in o)) o[n] = {}
    o = o[n] // 等同於閉包，將 o 的 context 改為o[n]
  }
}

setValue(obj, a)
console.log(obj)
```

## 隨機產生人物資料

```javascript
const generateRandomUser = () => {
  return new Promise(async (resolve, reject) => {
    try {
      const resp = await axios.get('https://randomuser.me/api/');
      resolve(resp.data);
    } catch (err) {
      reject(err);
    }
  });
};
```

## 隨機產生圖片

[https://picsum.photos/200/300](https://picsum.photos/200/300)
