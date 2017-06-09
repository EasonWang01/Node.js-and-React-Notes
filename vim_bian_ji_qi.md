# Vim 編輯器

## 1

有時出現以下，而無法WQ離開時

`NO WRITE SINCE...`

可加上`!`如下

`:Q!`

## 2

如果檔案編輯中突然離開

它會自動產生`.檔名.swp`檔案

把他刪除即可

## 3

啟用滑鼠

```
:set mouse=a
```

接著就可以使用滑鼠移動游標或是選取文字區塊

## 4

復原  
`u`

貼上  
`shift+ins(鍵盤右上)`

或直接按右鍵

## 5

在某一行按`v`

之後利用方向鍵即可開始選行，進行複製刪除等動作

## 6

按下小寫`y`可以複製起選擇的文字，按下d可以刪除掉選取的文字。  
        在想要貼上文字的地方，按下`p`就可以貼上剛才複製好的文字。

7

```
到行尾 ： shift+$
到行首 ： 0
```

8.安裝vim package manger  
 有幾種可以選擇，這裡使用NeoBundle

```
 $ curl https://raw.githubusercontent.com/Shougo/neobundle.vim/master/bin/install.sh > ./install.sh
 $ sh ./install.sh
```

> 注意事項:
>
> 如果使用windows cmd去用vim可能會造成跑版，但putty沒問題





