# VS code 編輯器

####1.記得先到喜好設定的工作區設定tab size不然程式碼排版會改變


把想要改的地方從左側貼到右側在輸入數字即可
```
{
  "editor.tabSize": 2
}
```

####2.快速關閉開啟資料夾結構bar
```
cmd+b
```

####3.預設為使用cmd+z+shift（較特別）為回復上一步
使用以下改為

keybindings.json
```
// 將您的按鍵組合放入此檔案中以覆寫預設值
[
    { "key": "cmd+y",           "command": "redo",
                                     "when": "editorTextFocus && !editorReadonly" },
]
```
####4.開啟終端機
```
control+`
```

####5.從terminal開啟vscode

先到vscode中點選`Shift+command+P`

之後輸入`shell command`安裝

之後cd到專案資料夾即可輸入`code .`開啟