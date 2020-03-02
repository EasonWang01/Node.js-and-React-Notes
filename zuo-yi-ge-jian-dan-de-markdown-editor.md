# 做一個簡單的markdown editor

## 做一個簡單的markdown editor

放入package.json後使用npm install

package.json

```text
 {
      "name": "RealtimeMarkdownViewer",
      "description": "Realtime Markdown Viewer",
      "main": "server.js",
      "version": "1.0.0",
      "repository": {
        "type": "git",
        "url": "git@github.com:sifxtreme/realtime-markdown.git"
      },
      "keywords": [
        "markdown",
        "realtime",
        "sharejs"
      ],
      "author": "Asif Ahmed",
      "dependencies": {
        "express": "^4.12.4",
        "ejs": "^2.3.1",
        "redis": "^0.10.3",
        "share": "0.6.3"
      },
      "engines": {
        "node": "0.10.x",
        "npm": "1.3.x"
      }
    }
```

server.js

```text
var express = require('express');
var app = express();

// set the view engine to ejs
app.set('view engine', 'ejs');

// public folder to store assets
app.use(express.static(__dirname + '/public'));

// routes for app
app.get('/', function(req, res) {
  res.render('pad');
});

// listen on port 8000 (for localhost) or the port defined for heroku
var port = process.env.PORT || 8000;
app.listen(port,function(){console.log("listen 8000")});
```

建造views資料夾，裡面存放模板

在views內創造pad.ejs

```text
<!DOCTYPE html>
<html>
<head>
    <title>Realtime Markdown Viewer</title>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css" rel="stylesheet">
    <link href="style.css" rel="stylesheet">
</head>

<body class="container-fluid">

    <section class="row">
        <textarea class="col-md-6 full-height" id="pad">Write your text here..</textarea>
        <div class="col-md-6 full-height" id="markdown"></div>
    </section>

    <script src="https://cdn.rawgit.com/showdownjs/showdown/1.0.2/dist/showdown.min.js"></script>
    <script src="script.js"></script>

</body>
</html>
```

建造public資料夾在根目錄，即為我們的static folder

裡面放入style.css

```text
html, body, section, .full-height {
    height: 100%;
} 

#pad{
    font-family: Menlo,Monaco,Consolas,"Courier New",monospace;

    border: none;
    overflow: auto;
    outline: none;
    resize: none;

    -webkit-box-shadow: none;
    -moz-box-shadow: none;
    box-shadow: none;
}

#markdown {
    overflow: auto;
    border-left: 1px solid black;
}
```

public裡面放入script.js

```text
window.onload = function() {
    var converter = new showdown.Converter();
    var pad = document.getElementById('pad');
    var markdownArea = document.getElementById('markdown');   

    var convertTextAreaToMarkdown = function(){
        var markdownText = pad.value;
        html = converter.makeHtml(markdownText);
        markdownArea.innerHTML = html;
    };

    pad.addEventListener('input', convertTextAreaToMarkdown);

    convertTextAreaToMarkdown();
};
```

此時輸入 node server 可看到兩個框框，在左邊那個框輸入markdown試試

## 讓他成為共筆的markdown

```text
npm install --save share@0.6.3
npm install --save redis
```

[https://github.com/showdownjs](https://github.com/showdownjs) [https://www.npmjs.com/package/showdown](https://www.npmjs.com/package/showdown)

