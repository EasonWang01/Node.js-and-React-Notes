# Class

基本用法：

```js
class human {
  constructor(height, weight){
    this.height = height;
    this.weight = weight;
  }
  get getHeight() {
    return this.height;
  }
}

const alice = new human(178, 67);
console.log(alice);
// 178
```

> constructor 用來將傳進new class 的參數賦予給class內的變數。
>
> 用this.height來將傳進來的height讓class內可以使用。

#### Static

```js
class human {
  constructor(height, weight){
    this.height = height;
    this.weight = weight;
  }
  static fly() {
    console.log(this)
    console.log(new this(12,12));
  }
}

human.fly();
```

> static 方法的this會是還沒有new之前的class，所以也沒辦法存取到this.height
>
> console.log\(this\) 會是 \[Function: human\]



