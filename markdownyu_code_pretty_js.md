# Markdown與code pretty js


##1.Markdown
使用經驗:
```
一個星數較高的markdown.js
另一個是showdown.js

個人使用覺得showdown.js較好用，原因為，其解析和github的markdown相似

https://github.com/showdownjs/showdown
```


安裝:
```
 <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/showdown/1.4.2/showdown.min.js"></script>
```
範例:
```
var converter = new showdown.Converter();
document.getElementById('realArticle').innerHTML =converter.makeHtml(e);
```


##2.code pretty js


####用途:讓文章中的程式碼部份加上顏色

####下載:
```
 <script src="https://cdn.rawgit.com/google/code-prettify/master/loader/run_prettify.js"></script>
```
此為google的專案

https://github.com/google/code-prettify

####使用方法
```
<pre class="prettyprint"><code class="language-java">...</code></pre>
```
可換成其中language後可換成
```
    "bsh", "c", "cc", "cpp", "cs", "csh", "cyc", "cv", "htm", "html",
    "java", "js", "m", "mxml", "perl", "pl", "pm", "py", "rb", "sh",
    "xhtml", "xml", "xsl".
```


範例
```
<pre class="prettyprint"><code class="language-js">

document.getElementById('testSend').addEventListener('click',function(){

  loadhtml('test1.html',function(e){
   document.getElementById('test11').innerHTML = e;
   execScript('test11');
  })

</code></pre>
```


