# class

基本用法：

```javascript
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

## Static

```javascript
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

## class 均為嚴格模式執行 \(no autoboxing\)

```javascript
class human {
  constructor(height, weight){
    this.height = height;
    this.weight = weight;
  }
  c(){
    return this
  }
}

const a = new human(178, 67);
cc = a.c;
console.log(cc()) // undefined

cc = a.c();
console.log(cc) // human { height: 178, weight: 67 }
```

## 繼承父 class

> 如果class內 function名稱相同會繼承，但也可以直接在子類別覆蓋

```javascript
function Animal (name) {
  this.name = name;  
}

Animal.prototype.speak = function () {
  console.log(this.name + ' makes a noise.');
}

class Dog extends Animal {
  speak() {
    console.log(this.name + ' barks.');
  }
}

var d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```

## class 換成 function

```javascript
class Animal {
  constructor(name){
    this.name = name
  }
}

// 等同於

function Animal (name) {
  this.name = name;  
}
```

## 不同class 傳遞參數

假設其上層的 js

```javascript
  const infoBox = new InfoBox(data);
  const worldMap = new Map(data, updateYear);
  
  // 這時當 worldMap 內執行了 this.updateYear 時會執行父層的 updateYear，
  // 這時可存取 infoBox 並將直傳入
  
  const updateYear = (year) => {
    // year 假設來自 worldMap
    // infoBox 內有個 updateTextDescription function
    infoBox.updateTextDescription({ activeYear: year });
  };
```

