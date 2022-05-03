# React hook

## React hook 教學

可以讓 functional component 也擁有 ref, state, 生命週期等方法。

### 1. useState

> &#x20;1.set state 會進行淺比較，如果兩次傳入的參數相同則不會進行 re-render
>
> 2\. re-render 時不會使 state 重置，包含子組建的 state 也會保持狀態

```javascript
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

> tip: 多個boolean狀態管理在同個object

```javascript
const [studyListOpen, setStudyListOpen] = React.useState({});
setStudyListOpen({ ...studyListOpen, [id]: !studyListOpen[id] || false });
```

> 更新 array 可以如下，如果用push的會沒作用

```javascript
setArray([...arr, {obj}])
```

### 2. useEffect

當第二個參數的 array 中有 state 改變時會觸發 useEffect 內的 function，為異步的

```javascript
import React, { useEffect } from 'react';

function Example() {

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    console.log('Mounted')
  }, /** 記得傳此參數，否則會重複render/);

  return (
    <div>
      <p>You clicked {count} times</p>
        Click me
      </button>
    </div>
  );
}
```

> 用 useEffect 有個雷要注意，記得傳第二個參數，否則會發現怎麼一直重複render
>
> [https://reactjs.org/docs/hooks-reference.html#conditionally-firing-an-effect](https://reactjs.org/docs/hooks-reference.html#conditionally-firing-an-effect)

### 3. UseLayoutEffect

為同步的，在 dom 更新後但在畫面渲染前觸發，會比 useEffect 早觸發。

[https://reactjs.org/docs/hooks-reference.html#uselayouteffect](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)

```
useLayoutEffect(()=> {
  console.log('I am about to render!');
},[]);
```

### 4. Ref

> ref 除了可用來存取元素外，另一個好處是可以用來存值，並且每次 re-render 後會保存，並且使用 ref.current 後改變值並不會造成 re-render

```javascript
import React, { useState, useEffect, useRef } from 'react';

const Map = () => {
  const mapEle = useRef(null);
  return (
    <div ref={mapEle}>
  </div>
  )
};

export default Map;
```

> 之後用`mapEle.current` 即可存取

### **5. Memo**

memo只能用在stateless component

功能類似 pure component

> 當父元素重新 render 時子元素預設都會重新渲染，所以可以使用 memo 來讓子元件監測到只有 prop 改變時才重新渲染（以前的做法是用 shouldComponentUpdate）一般為淺比較，

```javascript
import React, { memo } from 'react';

 const Test = memo(function Son(props){
    return (
        <div>{props.val}</div>
    )
})
export default Test;
```

可以傳入第二個參數，用來做深比較，第二個參數如果回傳為 true 則不會 re-render

![](<../.gitbook/assets/截圖 2021-10-04 上午11.34.43.png>)

### 6.forwardRef, useImperativeHandle

用來將子元件的方法傳給父元件

```javascript
const { forwardRef, useRef, useImperativeHandle } = React;

// We need to wrap component in `forwardRef` in order to gain
// access to the ref object that is assigned using the `ref` prop.
// This ref is passed as the second parameter to the function component.
const Child = forwardRef((props, ref) => {

  // The component instance will be extended
  // with whatever you return from the callback passed
  // as the second argument
  useImperativeHandle(ref, () => ({

    getAlert() {
      alert("getAlert from Child");
    }

  }));

  return <h1>Hi</h1>;
});

const Parent = () => {
  // In order to gain access to the child component instance,
  // you need to assign it to a `ref`, so we call `useRef()` to get one
  const childRef = useRef();

  return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.getAlert()}>Click</button>
    </div>
  );
};
```

### 7. useCallback

當今天傳入子元素的參數是一個 function，這時要避免子元素 re-render 在子元素寫了 memo 會失效，所以除了 memo 外，還要在父元素的 function 包一個 useCallback。

```javascript
  .....
 const doSet = useCallback(() => {
    seta({c: 'aas'})
  }, [])
  return (
    <div className="App">
      <Counter doSet={doSet}></Counter>
    </div>
  );
  ....
```

### 8. useMemo

useMemo 跟子元素比較沒關係，單純是在該元素內記憶比較複雜的運算，避免 re-render，但通常裡面都是放沒有 side effect 的東西，如果有 side effect 的計算請放到 useEffect。

useMemo 其實跟 useEffect 類似，如果第二個參數是空的 \[] 則只有第一次 render 時會執行。

或是第二個參數裡面的 state 改變時才會再次執行。

### 9. Interval 寫法

```javascript
  const [counter, setCounter] = useState(0);
  useEffect(() => {
    const id = setTimeout(() => {
      setCounter(counter + 1);
    }, 1000);
    return () => {
      clearTimeout(id);
    };
  }, [counter]);
```

## UseMemo, UseCallback, React.memo 區別

[https://github.com/facebook/react/issues/15156](https://github.com/facebook/react/issues/15156)

1. 父元件的狀態被改變了，但是傳給子元件 props 的沒有變，子元件仍然會被重新渲染，所以要用 React.memo
2. 但假設使用了 React.memo 如果傳入的 prop 是 function 或 object 還是會重新渲染，所以可以在該 prop 包成 useCallback 或 UseMemo
3. useMemo 跟子元素比較沒關係，單純是在該元素內記憶比較複雜的運算，避免 re-render，但通常裡面都是放沒有 side effect 的東西，如果有 side effect 的計算請放到 useEffect
4. useCallback 回傳 function、UseMemo 回傳 value

[https://medium.com/starbugs/react-%E9%97%9C%E6%96%BC-component-%E6%95%88%E8%83%BD%E5%84%AA%E5%8C%96%E7%9A%84%E9%82%A3%E4%BB%B6%E5%B0%8F%E4%BA%8B-68e6e5ecc4d6](https://medium.com/starbugs/react-%E9%97%9C%E6%96%BC-component-%E6%95%88%E8%83%BD%E5%84%AA%E5%8C%96%E7%9A%84%E9%82%A3%E4%BB%B6%E5%B0%8F%E4%BA%8B-68e6e5ecc4d6)

[https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/react-optimize-performance-using-memo-usecallback-usememo-a76b6b272df3](https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/react-optimize-performance-using-memo-usecallback-usememo-a76b6b272df3)
