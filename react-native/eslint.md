# ESlint

內建已經有一個 .eslint.js 建議可以改為以下

```javascript
module.exports = {
  "root": true,
  "extends": '@react-native-community',
  "parser": "babel-eslint",
  "overrides": [
    {
      "files": ["*.js"],
      "rules": {
        "react-native/no-inline-styles": "off"
      }
    }
  ],
};
```

