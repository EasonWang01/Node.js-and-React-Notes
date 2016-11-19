# imgur API

先去註冊https://api.imgur.com/oauth2/addclient

如果不用oauth2可直接使用client ID發request


#browser
參考以下範例
```
import { User, Post } from './DB.js';

exports.api = (app) => {

app.post('/getArticle',function(req,res){
	Post.find({})
  	.then(data => {
	  	 res.end(JSON.stringify(data))
		})
})

app.get('/getUser',function(req,res){
	User.find({account:req.session.user},{_id:0,account:1,email:1,name:1})
		.then(data => {
			//req.cookie.user = req.body.account;
			res.end(JSON.stringify(data[0]));
		})
		.catch(err => console.log(err));
})

app.post('*',function(req,res,next){
	if(req.connection.remoteAddress !== '127.0.0.1'){
		res.end('not local')
	}
	next();
})

app.post('/login',function(req,res){
	User.find({account:req.body.account})
		.then(data => {
			if(data[0] === undefined) {
				res.end('帳號或密碼錯誤');
			}
			if (data[0].password === req.body.password) {
			  res.cookie('ifUser',true, { maxAge: 1000 * 60 * 60 * 24 * 30, httpOnly: false });
				req.session.user = req.body.account;//將會在cookie中存入token之後token回到server取值
			  res.end('success login');
			}else{
				res.end('帳號或密碼錯誤');
			}
		})
})

app.post('/logout',function(req,res){
	res.cookie('ifUser',true, { expires: new Date() });
	req.session.user = null
	res.end();
})

app.post('/signup',function(req,res){
	User.find({account:req.body.account})
  	.then(data => {
	  	if(data[0] !== undefined){
		  	res.end('此帳號已被註冊');
		  }
	  })
		.then(() => {
			let user = new User({
				account: req.body.account,
				password: req.body.password,
			  email: req.body.email,
			  name: req.body.nickName,
			  RegistedDate: new Date()
			});
			user.save()
			.catch(err => console.log(err));
			res.end('成功註冊');
		})
   	.catch(err => console.log(err));
})

app.post('/postArticle',function(req,res) {
	if(typeof req.session.user === 'string') {
		let post = new Post({
			posterAccount: req.body.account,
			posterName: req.body.name,
			title: req.body.title,
		  content: req.body.content,
		  PostDate: new Date()
		});
		post.save()
		.then(() => {
			res.end('發表文章成功');
		})
		.catch(err => {
			res.end('發表文章錯誤');
		});
	}
})




};

```
#Node.js
https://github.com/kaimallea/node-imgur/blob/master/lib/imgur.js