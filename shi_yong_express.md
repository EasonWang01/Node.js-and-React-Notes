# 使用express

1.創建一個目錄，再進到該目錄
```
mkdir expressExample

cd expressExample
```
2.初始化
```
npm init
```
3.
```
npm install express --save
```
先創建一個index.js
```
var express = require('express');
var app = express();

app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/

app.listen(8080);
```
##但下面這是什麼?
```
app.use(express.static(__dirname + '/public'));
```
##試著把你剛才創的index.js複製一個到public資料夾，之後在網址打上http://localhost:8080/index.js


可以設定多個靜態目錄
```
app.use(express.static('public'));
app.use(express.static('img'));
app.use(express.static('pdf'));
```

4.我們在index.js 內加入下面，再執行看看
```
app.get('/', function (req, res) {
  res.send('Hello world!');
});
app.get('/hi', function (req, res) {
  res.send('Hello world!');
});
```
##每次修改完都要重新啟動server覺得很麻煩

所以我們要安裝一個套件:forever

```
npm install forever --save
```
開始監控
```
forever --watch index.js
```
結束監控
```
開啟另一個terminal 輸入 forever stopall
```
如果使用forever發現無法下指令關掉，可直接從os的工作管理員結束掉node.exe的程序即可
----------------------
5.接著我們加入更多路由功能
```
app.get('/', function (req, res) {
  res.send('Hello wogd!');
});
app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });
```
但發現這樣會很雜亂，所以我們新建一個目錄叫routes

裡面放入一個檔案index.js
```
module.exports = function (app) {
app.get('/', function (req, res) {
  res.send('Hello wogd!');
});
app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};
```
而 原本的檔案改為
```
var express = require('express');
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/

app.listen(8080);
```
這時我們試著把
```
var router = require('./routes/index.js')(app);
改為
var router = require('./routes')(app);

```
發現還是可以，原因是require如果指定為資料夾，他會預設去找下面的index檔案

express 是一個架構在http上的框架

##app.locals設定全局變數

除了用script設定變數外，我們還可用app.locals或res.locals設定變數

app.locals可給所有render後的views使用

而res.locals只有當次request生效
```
app.locals({
  title: 'Extended Express Example'
});
```

```
<%= title %>
```

#middleware

接收http請求，並對其進行加工，記得調用next，表示繼續往下加工
```
function IamMiddleware(req, res, next) {
  next();
}
```
有哪些種類?
```
應用程式層次的中介軟體
路由器層次的中介軟體
錯誤處理中介軟體
內建中介軟體
外部中介軟體
```
應用程式層次的中介軟體，將下面code貼在我們的index.js看看
```
app.use(function (req, res, next) {
  console.log('現在時間:', Date.now());
  next();
});
```
限定指定路徑使用
```
app.get('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});
```
```
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method);
  console.log(req.params.id);
  next();
});
```
#注意

(不要使用如下，因為有/hi,/as都會先跑過下面這行)

```

app.use('/', function (req, res) {
//相當於app.use(全部)
}
```


我們可以指定條件再middleware，先在原本middleware後加入一個function，

指定如果url的params = 條件的話跳過其下逗點後的function直接進行next middleware

呼叫 next('route')，來略過其餘的路由回呼。您可以使用這項機制，將控制權傳遞給後續的路由。
```
var express = require('express');
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/
app.get('/user/:id', function (req, res, next) {
  // 如果id參數為0跳到下面的middleware
  if (req.params.id == 0) next('route');
   
  else next(); 
}, function (req, res, next) {
  
  console.log("I am this")
  next(); 
});

// handler for the /user/:id path, which renders a special page
app.get('/user/:d', function (req, res, next) {
  
  console.log("I am next")
  next();
});
app.listen(8080);
```

##next() 跟next("router")的差別?

```
app.get('/forum/:fid', middleware1, middleware2, function(){
  // ...
})
middleware1() 可以使用 next() 去執行 middleware2, 或使用 next("route") 跳過後面，直接傳給下一個app實例
```
發現如果url為
```
http://localhost:8080/user/hi
```
將console出 
```
I am this 

I am next
```
如果url為
```
http://localhost:8080/user/0
```
將console出 
```
I am next
```
#整理路徑router

###第一個方法
1.index.js

```

var fruit = require('./fruit');
app.use('/fruit', fruit); ///路徑為/fruit時使用express.Router
```
2.設定router檔案
```
var express = require('express');
var fruit = express.Router();

fruit.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});

fruit.get('/banana', function(req, res) {
  res.send('Birds home page');
});

fruit.get('/apple', function(req, res) {
  res.send('About birds');
});

module.exports = fruit;
```
之後可以使用，以下連結瀏覽
```
http://localhost:8080/fruit/banana
```

##第二個方法
再index.js內加入
```

app.route('/fruit')
  .get(function(req, res) {
    res.send('Get a random fruit');
  })
  .post(function(req, res) {
    res.send('Add a fruit');
  })
  .put(function(req, res) {
    res.send('Update the fruit');
  });
```
##第三個方法
輸出router檔案
```
module.exports = function (app) {

app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Helloo worldd');
 
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};


```
index.js
```
var app = express();
require('./routes/index.js')(app);//引用router檔案，傳入express實例為參數
```
Express 支援下列的 HTTP路由方法：get、 post、put、head、delete、options、 trace、copy、lock、mkcol、move、purge、propfind、proppatch、unlock、report、mkactivity、checkout、merge、m-search、notify、subscribe、unsubscribe、patch、search， connect。


可使用，app.all來接受所有方法

和app.use類似，但app.use必須放在你要用到的東西前面
```
app.all('/', function (req, res, next) {
  console.log('all method');
  next(); 
});
```
區別
```
app.use:

1.inject middlware to your front controller configuring for instance: header, cookies, sessions, etc.

2.must be written before app[http_method] otherwise there will be not executed.

3.only one callback

4.比app.all先執行
-----------------------------------------
app.all:

for configuring routes' controllers,"all" means it applies on all http methods.

several callback
```
#錯誤處理middleware
錯誤處理中介軟體函數的定義方式，與其他中介軟體函數相同，差別在於錯誤處理函數的參數是四個，而非三個，而錯誤處理通常在其他 app.use() 和路由呼叫之後，最後才定義錯誤處理中介軟體。



#外部middleware

較常見為
```
cookie-parser

範例:https://www.youtube.com/watch?v=qOSTEnod0Qw

bodyParser

範例:https://www.youtube.com/watch?v=C3G3N4LMJeE
```
```
var express = require('express')
var bodyParser = require('body-parser')
 
var app = express()
 
// create application/json parser 
var jsonParser = bodyParser.json()
 
// create application/x-www-form-urlencoded parser 
var urlencodedParser = bodyParser.urlencoded({ extended: false })
 
// POST /login gets urlencoded bodies 
app.post('/login', urlencodedParser, function (req, res) {
  if (!req.body) return res.sendStatus(400)
  res.send('welcome, ' + req.body.username)
})
 
// POST /api/users gets JSON bodies 
app.post('/api/users', jsonParser, function (req, res) {
  if (!req.body) return res.sendStatus(400)
  // create user in req.body 
})
```
但如果post的編碼類型是multipart/form-data呢?(ex:上傳檔案)
```
使用multer middleware
```
simple middle寫法
```
var app = express();

var middleware = {

    render: function (view) {
        return function (req, res, next) {
            res.render(view);
        }
    },

    globalLocals: function (req, res, next) {
        res.locals({ 
            siteTitle: "My Website's Title",
            pageTitle: "The Root Splash Page",
            author: "Cory Gross",
            description: "My app's description",
        });
        next();
    },

    index: function (req, res, next) {
        res.locals({
            indexSpecificData: someData
        });
        next();
    }

};


app.use(middleware.globalLocals);
app.get('/', middleware.index, middleware.render('home'));
app.get('/products', middleware.products, middleware.render('products'));

```



更多可參考http://expressjs.com/zh-tw/resources/middleware.html

# express 的set 方法
```
app.set("views", __dirname + "/views");

app.set("view engine", "handlebars");
```
最常看見上面這兩種寫法，但他其實只是為你的前面的參數的值指定為第二個參數
```
app.set('Fruit', 'I am banana');
console.log(app.settings.Fruit);///需使用app.settings去讀取
console.log(app.get('Fruit'));或app.get

```
###1.設定cookie


https://github.com/expressjs/session/issues/403#issuecomment-269422586
```
cookieParser = require('cookie-parser')

app.use(cookieParser());

app.get("/",function(req,res){
res.cookie('cookieName',12, { maxAge: 900000, httpOnly: true });
res.render('land');

});
```
讀取
```
console.log(req.cookies);//記得加S
```

移除

```
res.cookie('ifUser',true, { expires: new Date() });
```

###1.設定檔案目錄
如果render html後想在該html裡面讀取某個js檔案，必須用
```
app.use(express.static(__dirname + '/public/'));
```
否則你的server找不到該檔案


###1.使用session


session有兩種，一種是brower的，關閉及消失．一種是會把cookie拿來server驗證的session

以下介紹第二種

```
const session = require('express-session');
const MongoStore = require('connect-mongo')(session);
app.use(session({
	saveUninitialized: false, // don't create session until something stored
  resave: false, //don't save session if unmodified
  secret: 'yicheng',
  key: 'auth_token',//cookie name
  cookie: {maxAge: 1000 * 60 * 60 * 24 * 1},//1 days
  store: new MongoStore({
		url:'mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass'
  })
}));
```

####注意：如果講此選項設為true則重新整理網頁在你還沒設session會自動附加上去，所以建議為false
```
saveUninitialized:false
```

上面為基本設置

新增，會附加cookie到browser上
```
req.session.user = req.body.account;//將會在cookie中存入token之後token回到server取值
```

移除

```
req.session.user = null;  //移除server端資料
req.session.cookie.expires = new Date(Date.now()); ///移除browser端cookie

```

#1.使用JWT Token 

https://github.com/auth0/node-jsonwebtoken



我們要先安裝

```
 npm install jsonwebtoken
```

之後

```
let token = jwt.sign({
  exp: Math.floor(Date.now() / 1000) + (60 * 60 * 24),
  data: {
  user: jwtpayload
  }
}, jwtSecret);
res.cookie('t', token, { maxAge: 1000 * 60 * 60 * 24 * 1, httpOnly: true });
```
上面是將token存在cookie的做法，也可使用別的方式傳回client

認證:

```
 app.post('/',function(req,res) {
 	jwt.verify(req.cookies.t, jwtSecret, (err, decoded) => {
 		if(decoded){
 			// TODO 認證成功後要做的事
 		} else {
 			console.log('no token');
		}
 	}
 }
```

>也可寫為function然後用middleware方式

```
const authToken = (req,res,next) => {
	const token = req.cookies.t;
	if (token) {
		jwt.verify(req.cookies.t, jwtSecret, (err, decoded) => {
			if(decoded){
				next();
			} else {
				res.end('token not correct');
			}
	  });
	} else {
		res.end('no Token');
	}
}
```

```
app.get('/userArticles/:user',authToken,(req,res) => {
	Post.find({posterAccount: req.params.user})
	.then(data => {
  	 res.end(JSON.stringify(data))
	})
})

```