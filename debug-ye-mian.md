# Debug 頁面

可參考 :

[https://stackoverflow.com/a/25818251/4622645](https://stackoverflow.com/a/25818251/4622645)

### 1.先在Source的右側選單選取要偵測的事件

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/df1.png)

### 2.這時如果有用套件的話 Debug Function trace會跑進很多不相關的套件內容，把他加入blackbox即可

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/df.png)

### 3.之後點擊按鈕後即會出現Debug選項

左邊是跳下一個script，右邊按鈕是跳入下一個Function

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/df3.png)

> 也可在程式加入`debugger;` 來指定執行到哪裡時要中斷。

### 4.也可在元素上點擊，幫元素加上斷點

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/df4.png)

## Debug深層Function

有時斷點執行時不會跳入深層Function的執行過程，我們只要在Function的開頭加上

```text
debugger;
```

即可

```text
function AssembleOrder() {
  debugger;
  if (!checkNonZeroNumber($("#multiples").val()))
  .....
}
```

