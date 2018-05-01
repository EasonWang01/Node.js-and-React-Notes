# VS code 編輯器

#### 1.記得先到喜好設定的工作區設定tab size不然程式碼排版會改變

把想要改的地方從左側貼到右側在輸入數字即可

```
{
  "editor.tabSize": 2
}
```

#### 2.快速關閉開啟資料夾結構bar

```
cmd+b
```

#### 3.預設為使用cmd+z+shift（較特別）為回復上一步

使用以下改為

keybindings.json

```
// 將您的按鍵組合放入此檔案中以覆寫預設值
[
    { "key": "cmd+y",           "command": "redo",
                                     "when": "editorTextFocus && !editorReadonly" },
]
```

#### 4.開啟終端機

    control+`

#### 5.從terminal開啟vscode

先到vscode中點選`Shift+command+P`

之後輸入`shell command`安裝

之後cd到專案資料夾即可輸入`code .`開啟

#### 6.code snippet

[https://www.facebook.com/pjchender/photos/a.1330200713732028.1073741836.768320183253420/1330201030398663/?type=3&theater](https://www.facebook.com/pjchender/photos/a.1330200713732028.1073741836.768320183253420/1330201030398663/?type=3&theater)

#### 7.Format code

[https://stackoverflow.com/questions/29973357/how-do-you-format-code-in-visual-studio-code-vscode](https://stackoverflow.com/questions/29973357/how-do-you-format-code-in-visual-studio-code-vscode)

如果indent空格不對可以參考這篇設定[https://stackoverflow.com/questions/36251820/visual-studio-code-format-is-not-using-indent-settings](https://stackoverflow.com/questions/36251820/visual-studio-code-format-is-not-using-indent-settings)

> 如果mac的shift+option+ F 沒反應，可以把所有plugin更新並重新啟動即可

8.選不同行的相同字

```
選字完後按下   command + d

//windows  ctrl + d
```

#### 8. Code snippets

> 可自行定義code 快捷鍵

https://code.visualstudio.com/docs/editor/userdefinedsnippets

> 範例: https://github.com/xabikos/vscode-react/blob/master/snippets/snippets.json



