# EJS

類似於Handlebars的模板引擎

好處為語法即為javascript使用上較為直覺

Client Side ejs (始祖)
http://www.embeddedjs.com/

第一版server side ejs
https://github.com/tj/ejs

第二版server side ejs
https://github.com/mde/ejs

https://www.npmjs.com/package/ejs

##使用

安裝好Express後用

```

app.set('view engine', 'ejs');
```
引入即可，之後創建views資料夾，資料夾內檔案後檔名取.ejs

##!!如出現找不到ejs module
1.原因為如果是使用-g的express必須將ejs也安裝在-g

2.或是將express和ejs都安裝在local

```
<% %>同一行有HTML tag 則加等號 <%= %>

```

##使用client端EJS
https://github.com/mde/ejs/issues/158#issuecomment-215120752

(此與tj的ejs不同)
http://www.embeddedjs.com/

所以你如果要用在client不可直接使用npm安裝的ejs，要另外從他們官網下載(上面網址)

之後使用時因為要render所以要找views目錄，但又要找ejs.js，
所以折衷的方法為，將ejs.js放入views資料夾，然後
```
app.use(express.static('views'));
```

或是使用兩個靜態目錄
```
app.use(express.static('scripts'));
app.use(express.static('views'));
```
最後

```
<script src="ejs.js"></script>

 var fragment = new EJS({url:'userprofile.ejs'}).render();//需使用舊版ejs非tj
 
 亦可塞入data
 
   var data = { title: "Try EJS!" };
   var fragment = new EJS({url: 'template.ejs'}).render(data);

```
#SPA example
```
  <li><a onclick="articleFragment()
">顯示文章</a></li>

function articleFragment () {document.getElementById('post').innerHTML =  new EJS({url:'article.ejs'}).render()};

```

####(但是，發現在article.ejs 內寫script沒反應)

原因是因為，在div內的script無法作用，我們要加上
```
function articleFragment() {document.getElementById('post').innerHTML =  new EJS({url:'article.ejs'}).render()

//執行div內的所有script標籤
MyDiv = document.getElementById('post')
var arr = MyDiv.getElementsByTagName('script')
for (var n = 0; n < arr.length; n++)
    eval(arr[n].innerHTML)



};

```
####將server render附帶的data於client端用js儲存
```
原先res.render(abc.ejs,{data})

這個data只使用<% data %＞讀出，無法用一般js取操
作


```

所以我們可以在client端把它存成一般的variable

```
<script>

b =  <%= JSON.stringify(res)%> ;
</script>
```



###使用TJ的EJS用在CLIENT端
可參考他的範例
https://github.com/tj/ejs/issues/39
但目前EJS要寫在HTML的SCRIPT標籤內，無法使用EJS外部路徑




#用法
##1.Include
在ejs2.0可使用include加上傳入data
```
  <%- include('post', {content: 52}) %>
```
1.0不可傳入data，但1.0的include寫法在2.0仍適用
```
<% include post %>
```
include模板時傳入資料第二個方法:

使用`app.locals`設定
```
app.locals({
  title: 'Extended Express Example'
});
```
view.ejs
```
   <h1><a href="/"><%= title %></a></h1>
```

##2.模板內的js只讀的到用<%%>包起來的js

```
//在script標籤內寫var a = true 無效，必須用<%%>包起來

<% var a = true %>
  <% if(a){ %>
    <%- include('post') %>
  <% }; %>
```

#附註:mde的ejs如server端跟client都用mde的，同時使用會有些問題
ex:
```
  <script type="text/javascript">
    
 var people = ['geddy', 'neil', 'alex'],
      aa = ejs.render('<%= people %>', {people: people});

  </script>
```
server一開始會說people undefined，因為server端的ejs會去找res.render有沒有people，而不是等到執行client後才去從ejs.render裡面找，但假如我們也在res.render裡面加上people參數的話，會發現可正常執行，但是出現aa最後產生的值沒有引入people

所以建議的做法是在client端使用
http://www.embeddedjs.com/
的