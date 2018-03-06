# js trick

# 1.替換input type＝"file"標籤為客製化按鈕

因為label規定不可放button在內，所以我們使用click\(\)的方法，如下

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

# 2.轉為boolean

`!!`

# 3.除與二的次方

`>>>`

# 4.複製到剪貼版

```js
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



