# JS 格式轉換

String to Dom
使用Dom parser
```
var xmlString = "<div id='foo'><a href='#'>Link</a><span></span></div>"
  , parser = new DOMParser()
  , doc = parser.parseFromString(xmlString, "text/xml");
doc.firstChild // => <div id="foo">...
doc.firstChild.firstChild // => <a href="#">...
```