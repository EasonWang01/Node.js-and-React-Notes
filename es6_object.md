# ES6 Object


##1.Object.assign

###(功能:淺層融合object)

```
a={"v":12}
Object.assign(a,{"v":13})
Object.assign({},a,{"v":14})
```
上面兩個object.assign差別在哪?

答:第一個會新建一個物件且順便把a變數裡面的"v"的值覆蓋
   第二個會新建一個物件，但不會改變定義好的參數。
   
但，新建物件Object.assign後面的參數如有相同值都會覆蓋前面的值。


##預設參數屬性

```
function todoApp(state = initialState, action) {}
//有了 = 符號
//如果沒有傳入state參數(第一個參數)，則會字動找initialState變數指定給function 內的state變數
```

#Object 解構賦值

1.
```
要保證key值匹配。

let obj = {x: 1, y: 2}; let {x, y} = obj; // x = 1, y = 2

//產生兩個新變數x,y

```
2.當函式的參數為物件，名稱與值相同時
```
const sendLoginRequest = ({username, password}) => dispatch(loginRequest({username, password}))


//類似於{username:username,password:password}

```
可參考

https://read01.com/oE8Q.html


http://www.html5cn.org/article-9274-1.html
