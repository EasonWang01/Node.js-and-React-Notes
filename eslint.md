# ESLint

## ESLint

[https://wohugb.gitbooks.io/javascript/content/tool/lintmd.html](https://wohugb.gitbooks.io/javascript/content/tool/lintmd.html)

### 用處:用來檢查code的語法

1.安裝

```text
npm install -g eslint
```

2.配置

```text
於根目錄新增   .eslintrc.json
```

```text
{
  "globals": {
    // Put things like jQuery, etc
    "jQuery": true,
    "$": true
  },
  "env": {
    // I write for browser
    "browser": true,
    // in CommonJS
    "node": true
  },
  // To give you an idea how to override rule options:
  "rules": {
    // Tons of rules you can use, for example...
    "quotes": [1, "double"]
  }
}
```

看到上面的rules的數字，對照下表，以上面為例，意思是對所有出現quotes\(引號\)的地方要使用double\(兩個引號\)，強制性為1\(只產生警告訊息\)

```text
0 - Disable the rule
1 - Warn about the rule
2 - Throw error about the rule
```

### 其他rules可參考官網

[http://eslint.org/docs/rules/](http://eslint.org/docs/rules/)

3.使用parser去解析ES6

```text
npm install babel-eslint
```

之後在.eslintrc.json加上

```text
{
  "parser": "babel-eslint",
  "rules": {
    ...
  }
}
```

## 如何執行?

```text
eslint 檔案名稱.js
```

### 其他規則

```text
{  
  "env": {  
    "browser": true,  
    "node": true,  
    "commonjs": true  
  },  
  "ecmaFeatures": {  
    // lambda表達式  
    "arrowFunctions": true,  
    // 解構賦值  
    "destructuring": true,  
    // class  
    "classes": true,  
    // http://es6.ruanyifeng.com/#docs/function#函數參數的默認值  
    "defaultParams": true,  
    // 塊級作用域，允許使用let const  
    "blockBindings": true,  
    // 允許使用模塊，模塊內默認嚴格模式  
    "modules": true,  
    // 允許字面量定義對像時，用表達式做屬性名  
    // http://es6.ruanyifeng.com/#docs/object#屬性名表達式  
    "objectLiteralComputedProperties": true,  
    // 允許對像字面量方法名簡寫  
    /*var o = { 
        method() { 
          return "Hello!"; 
        } 
     }; 

     等同於 

     var o = { 
       method: function() { 
         return "Hello!"; 
       } 
     }; 
    */  
    "objectLiteralShorthandMethods": true,  
    /* 
      對像字面量屬性名簡寫 
      var foo = 'bar'; 
      var baz = {foo}; 
      baz // {foo: "bar"} 

      // 等同於 
      var baz = {foo: foo}; 
    */  
    "objectLiteralShorthandProperties": true,  
    // http://es6.ruanyifeng.com/#docs/function#rest參數  
    "restParams": true,  
    // http://es6.ruanyifeng.com/#docs/function#擴展運算符  
    "spread": true,  
    // http://es6.ruanyifeng.com/#docs/iterator#for---of循環  
    "forOf": true,  
    // http://es6.ruanyifeng.com/#docs/generator  
    "generators": true,  
    // http://es6.ruanyifeng.com/#docs/string#模板字符串  
    "templateStrings": true,  
    "superInFunctions": true,  
    // http://es6.ruanyifeng.com/#docs/object#對像的擴展運算符  
    "experimentalObjectRestSpread": true  
  },  

  "rules": {  
    // 定義對像的set存取器屬性時，強制定義get  
    "accessor-pairs": 2,  
    // 指定數組的元素之間要以空格隔開(,後面)， never參數：[ 之前和 ] 之後不能帶空格，always參數：[ 之前和 ] 之後必須帶空格  
    "array-bracket-spacing": [2, "never"],  
    // 在塊級作用域外訪問塊內定義的變量是否報錯提示  
    "block-scoped-var": 0,  
    // if while function 後面的{必須與if在同一行，java風格。  
    "brace-style": [2, "1tbs", { "allowSingleLine": true }],  
    // 雙峰駝命名格式  
    "camelcase": 2,  
    // 數組和對像鍵值對最後一個逗號， never參數：不能帶末尾的逗號, always參數：必須帶末尾的逗號，  
    // always-multiline：多行模式必須帶逗號，單行模式不能帶逗號  
    "comma-dangle": [2, "never"],  
    // 控制逗號前後的空格  
    "comma-spacing": [2, { "before": false, "after": true }],  
    // 控制逗號在行尾出現還是在行首出現  
    // http://eslint.org/docs/rules/comma-style  
    "comma-style": [2, "last"],  
    // 圈復雜度  
    "complexity": [2,9],  
    // 以方括號取對像屬性時，[ 後面和 ] 前面是否需要空格, 可選參數 never, always  
    "computed-property-spacing": [2,"never"],  
    // 強制方法必須返回值，TypeScript強類型，不配置  
    "consistent-return": 0,  
    // 用於指統一在回調函數中指向this的變量名，箭頭函數中的this已經可以指向外層調用者，應該沒卵用了  
    // e.g [0,"that"] 指定只能 var that = this. that不能指向其他任何值，this也不能賦值給that以外的其他值  
    "consistent-this": 0,  
    // 強制在子類構造函數中用super()調用父類構造函數，TypeScrip的編譯器也會提示  
    "constructor-super": 0,  
    // if else while for do後面的代碼塊是否需要{ }包圍，參數：  
    //    multi  只有塊中有多行語句時才需要{ }包圍  
    //    multi-line  只有塊中有多行語句時才需要{ }包圍, 但是塊中的執行語句只有一行時，  
    //                   塊中的語句只能跟和if語句在同一行。if (foo) foo++; else doSomething();  
    //    multi-or-nest 只有塊中有多行語句時才需要{ }包圍, 如果塊中的執行語句只有一行，執行語句可以零另起一行也可以跟在if語句後面  
    //    [2, "multi", "consistent"] 保持前後語句的{ }一致  
    //    default: [2, "all"] 全都需要{ }包圍  
    "curly": [2, "all"],  
    // switch語句強制default分支，也可添加 // no default 注釋取消此次警告  
    "default-case": 2,  
    // 強制object.key 中 . 的位置，參數:  
    //      property，'.'號應與屬性在同一行  
    //      object, '.' 號應與對像名在同一行  
    "dot-location": [2, "property"],  
    // 強制使用.號取屬性  
    //    參數： allowKeywords：true 使用保留字做屬性名時，只能使用.方式取屬性  
    //                          false 使用保留字做屬性名時, 只能使用[]方式取屬性 e.g [2, {"allowKeywords": false}]  
    //           allowPattern:  當屬性名匹配提供的正則表達式時，允許使用[]方式取值,否則只能用.號取值 e.g [2, {"allowPattern": "^[a-z]+(_[a-z]+)+$"}]  
    "dot-notation": [2, {"allowKeywords": true}],  
    // 文件末尾強制換行  
    "eol-last": 2,  
    // 使用 === 替代 ==  
    "eqeqeq": [2, "allow-null"],  
    // 方法表達式是否需要命名  
    "func-names": 0,  
    // 方法定義風格，參數：  
    //    declaration: 強制使用方法聲明的方式，function f(){} e.g [2, "declaration"]  
    //    expression：強制使用方法表達式的方式，var f = function() {}  e.g [2, "expression"]  
    //    allowArrowFunctions: declaration風格中允許箭頭函數。 e.g [2, "declaration", { "allowArrowFunctions": true }]  
    "func-style": 0,  
    "generator-star-spacing": [2, { "before": true, "after": true }],  
    "guard-for-in": 0,  
    "handle-callback-err": [2, "^(err|error)$" ],  
    "indent": [2, 2, { "SwitchCase": 1 }],  
    "key-spacing": [2, { "beforeColon": false, "afterColon": true }],  
    "linebreak-style": 0,  
    "lines-around-comment": 0,  
    "max-nested-callbacks": 0,  
    "new-cap": [2, { "newIsCap": true, "capIsNew": false }],  
    "new-parens": 2,  
    "newline-after-var": 0,  
    "no-alert": 0,  
    "no-array-constructor": 2,  
    "no-caller": 2,  
    "no-catch-shadow": 0,  
    "no-cond-assign": 2,  
    "no-console": 0,  
    "no-constant-condition": 0,  
    "no-continue": 0,  
    "no-control-regex": 2,  
    "no-debugger": 2,  
    "no-delete-var": 2,  
    "no-div-regex": 0,  
    "no-dupe-args": 2,  
    "no-dupe-keys": 2,  
    "no-duplicate-case": 2,  
    "no-else-return": 0,  
    "no-empty": 0,  
    "no-empty-character-class": 2,  
    "no-empty-label": 2,  
    "no-eq-null": 0,  
    "no-eval": 2,  
    "no-ex-assign": 2,  
    "no-extend-native": 2,  
    "no-extra-bind": 2,  
    "no-extra-boolean-cast": 2,  
    "no-extra-parens": 0,  
    "no-extra-semi": 0,  
    "no-fallthrough": 2,  
    "no-floating-decimal": 2,  
    "no-func-assign": 2,  
    "no-implied-eval": 2,  
    "no-inline-comments": 0,  
    "no-inner-declarations": [2, "functions"],  
    "no-invalid-regexp": 2,  
    "no-irregular-whitespace": 2,  
    "no-iterator": 2,  
    "no-label-var": 2,  
    "no-labels": 2,  
    "no-lone-blocks": 2,  
    "no-lonely-if": 0,  
    "no-loop-func": 0,  
    "no-mixed-requires": 0,  
    "no-mixed-spaces-and-tabs": 2,  
    "no-multi-spaces": 2,  
    "no-multi-str": 2,  
    "no-multiple-empty-lines": [2, { "max": 1 }],  
    "no-native-reassign": 2,  
    "no-negated-in-lhs": 2,  
    "no-nested-ternary": 0,  
    "no-new": 2,  
    "no-new-func": 0,  
    "no-new-object": 2,  
    "no-new-require": 2,  
    "no-new-wrappers": 2,  
    "no-obj-calls": 2,  
    "no-octal": 2,  
    "no-octal-escape": 2,  
    "no-param-reassign": 0,  
    "no-path-concat": 0,  
    "no-process-env": 0,  
    "no-process-exit": 0,  
    "no-proto": 0,  
    "no-redeclare": 2,  
    "no-regex-spaces": 2,  
    "no-restricted-modules": 0,  
    "no-return-assign": 2,  
    "no-script-url": 0,  
    "no-self-compare": 2,  
    "no-sequences": 2,  
    "no-shadow": 0,  
    "no-shadow-restricted-names": 2,  
    "no-spaced-func": 2,  
    "no-sparse-arrays": 2,  
    "no-sync": 0,  
    "no-ternary": 0,  
    "no-this-before-super": 2,  
    "no-throw-literal": 2,  
    "no-trailing-spaces": 2,  
    "no-undef": 2,  
    "no-undef-init": 2,  
    "no-undefined": 0,  
    "no-underscore-dangle": 0,  
    "no-unexpected-multiline": 2,  
    "no-unneeded-ternary": 2,  
    "no-unreachable": 2,  
    "no-unused-expressions": 0,  
    "no-unused-vars": [2, { "vars": "all", "args": "none" }],  
    "no-use-before-define": 0,  
    "no-var": 0,  
    "no-void": 0,  
    "no-warning-comments": 0,  
    "no-with": 2,  
    "object-curly-spacing": 0,  
    "object-shorthand": 0,  
    "one-var": [2, { "initialized": "never" }],  
    "operator-assignment": 0,  
    "operator-linebreak": [2, "after", { "overrides": { "?": "before", ":": "before" } }],  
    "padded-blocks": 0,  
    "prefer-const": 0,  
    "quote-props": 0,  
    "quotes": [2, "single", "avoid-escape"],  
    "radix": 2,  
    "semi": [2, "never"],  
    "semi-spacing": 0,  
    "sort-vars": 0,  
    "space-after-keywords": [2, "always"],  
    "space-before-blocks": [2, "always"],  
    "space-before-function-paren": [2, "always"],  
    "space-in-parens": [2, "never"],  
    "space-infix-ops": 2,  
    "space-return-throw-case": 2,  
    "space-unary-ops": [2, { "words": true, "nonwords": false }],  
    "spaced-comment": [2, "always", { "markers": ["global", "globals", "eslint", "eslint-disable", "*package", "!"] }],  
    "strict": 0,  
    "use-isnan": 2,  
    "valid-jsdoc": 0,  
    "valid-typeof": 2,  
    "vars-on-top": 0,  
    "wrap-iife": [2, "any"],  
    "wrap-regex": 0,  
    "yoda": [2, "never"]  
  }  
}
```

## 使用sublime+ESLint

進入sublime的package manage\(需先安裝\)

輸入

```text
SublimeLinter-eslint
```

參考至  
[http://jonathancreamer.com/setup-eslint-with-es6-in-sublime-text/](http://jonathancreamer.com/setup-eslint-with-es6-in-sublime-text/)

## Eslint React

如果在vscode React 沒有顯示 syntax error線，可安裝

```text
yarn add eslint-plugin-react
```

