# imgur API

先去註冊https://api.imgur.com/oauth2/addclient

如果不用oauth2可直接使用client ID發request


#browser
參考以下範例
```
<!DOCTYPE html>
<html>
  <head>

  </head>
<body>
  <input id="inp" type='file'>
  <p id="b64"></p>
  <img id="img" height="150">


  <script type="text/javascript">
    var base64;
      function readFile() {
        if (this.files && this.files[0]) {
          var FR= new FileReader();
          FR.onload = function(e) {
            document.getElementById("img").src = e.target.result;
            base64 = e.target.result.replace(/^data:image\/(png|jpg);base64,/, "");
            console.log(base64)
            document.getElementById("b64").innerHTML = e.target.result;


            var xhttp = new XMLHttpRequest();
            xhttp.open('POST','https://api.imgur.com/3/image',true)
            xhttp.setRequestHeader("Content-type", "application/json");
            xhttp.setRequestHeader("Authorization", "Client-ID b50a7351eee91f0");
            xhttp.send(JSON.stringify({'image': base64}));
            xhttp.onreadystatechange = function() {
              if (xhttp.readyState == 4 && xhttp.status == 200) {
                console.log(JSON.parse(xhttp.responseText));
              }
            };
          };
        FR.readAsDataURL( this.files[0] );
        }
      }
      document.getElementById("inp").addEventListener("change", readFile, false);
  </script>
</body>
</html>


```
#Node.js
https://github.com/kaimallea/node-imgur/blob/master/lib/imgur.js