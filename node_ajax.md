# Node ajax


```
app.post("/login",function(req,res){
    console.log(req.body);//console出ajax.data的內容
    res.send(req.body);//輸出到client端的ajax.success中
	res.end();
	
});
```

```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
  <script src="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
</head>
<body>

<div class="container">
  <h2>登入</h2>
  <form id="form1" class="form-horizontal" role="form" method="post" >
    <div class="form-group">
      <label class="control-label col-sm-2" for="email">Account:</label>
      <div class="col-sm-10">
        <input type="account" class="form-control" id="account" placeholder="Enter Account"name="account">
      </div>
    </div>
    <div class="form-group">
      <label class="control-label col-sm-2" for="pwd">Password:</label>
      <div class="col-sm-10">          
        <input type="password" class="form-control" id="pwd" placeholder="Enter password"name="password">
      </div>
    </div>

    <div class="form-group">        
      <div class="col-sm-offset-2 col-sm-10">
        <button type="submit" id="login" class="btn btn-default">登入</button>
        <p>{{message}}</p>
      </div>
    </div>
  </form>
</div>
<script>
//取消表單行為

  $('#form1').on('submit', function(e){
    // validation code here
    return false;
    
  });


//ajax登入
$("#login").click(function(){
var account = $("#account").val();
  $.ajax({
    type: 'POST',
    url:'/login',
    data:{account:account},
    success:function(result){console.log(result)},
    dataType:'json'
    
   
  });
});
</script>
</body>
</html>
```
