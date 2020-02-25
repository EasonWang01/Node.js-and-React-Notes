# JS 格式轉換

#1.String to DOM
使用DOM parser
```
var xmlString = "<div id='foo'><a href='#'>Link</a><span></span></div>"
  , parser = new DOMParser()
  , doc = parser.parseFromString(xmlString, "text/xml");
doc.firstChild // => <div id="foo">...
doc.firstChild.firstChild // => <a href="#">...
```
http://stackoverflow.com/questions/3103962/converting-html-string-into-dom-elements

#2. `'\/' === '/' in JavaScript`

你可以試著在瀏覽器console.log輸入

`"asdf<div><br><\/div><div>dff<\/div>"`

他會自動轉為

`"asdf<div><br></div><div>dff</div>"`

http://stackoverflow.com/questions/1580647/json-why-are-forward-slashes-escaped