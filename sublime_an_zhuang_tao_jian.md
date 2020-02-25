# Sublime 安裝套件

1.先安裝package manger

``按ctrl+```  根據這個網址填入 [https://packagecontrol.io/installation\#st2](https://packagecontrol.io/installation#st2)

2.按`ctrl + shift+ p`後點選install package

## 安裝eslint 的sublime套件

1.輸入linter 後點選SublimeLinter [http://sublimelinter.readthedocs.org/en/latest/installation.html](http://sublimelinter.readthedocs.org/en/latest/installation.html)

2.之後即可安裝在SublimeLinter中的plugin

`輸入eslint 之後再下面找到sublimeLinter-contrib-eslint` 3.之後再去設定目錄內的eslintrc檔案即可

[https://github.com/roadhump/SublimeLinter-eslint](https://github.com/roadhump/SublimeLinter-eslint)

## 安裝Babel jsx的syntax

一樣在package control 輸入babel安裝

再安裝`npm install babel-eslint -g`

之後在view--syntax--Babel--javscript\(Babel\)即可

## 安裝Emmet

安裝後於Package-setting=&gt;Emmet=&gt;Setting-User 輸入設定檔案

範例:

```text
 {

"snippets":{

        "html":{
            "snippets":{

                "css":"<link rel='stylesheet' type='text/css' href='' />",
                "js":"<script>   </script>",
                "e":"<%   %>",
                "ajax":"var xhttp = new XMLHttpRequest();\nxhttp.open('POST', '/ajax');\nxhttp.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');\nxhttp.onreadystatechange = function() {\nif (xhttp.readyState == 4 && xhttp.status == 200) {console.log(xhttp.responseText);}}\nxhttp.send();"


        },

    }

  }

}
```

如果要用多行

```text
\　：區隔"
\n ：斷行
\t ：向後空一格
```

可參考 [http://phoenote.blogspot.tw/2015/10/emmet-sublime-plugin.html](http://phoenote.blogspot.tw/2015/10/emmet-sublime-plugin.html)

## 安裝LiveStyle

\(不用儲存檔案即可及時看css效果\)

1.於sublime安裝LiveStyle

2.於Chrome的dev tool安裝LiveStyle

3.新增一個css檔案於html中\(不是用於inline style\)

4.開啟chrome dev tool並點選LiveStyle查看是否載入

5.記得，開著chrome dev tool 才可使用

