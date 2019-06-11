# React hook 教學

可以讓functional component也擁有 ref, state, 生命週期等方法。



## State

```js
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

## ComponentDidMount

```js
import React, { useEffect } from 'react';

function Example() {

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    console.log('Mounted')
  });

  return (
    <div>
      <p>You clicked {count} times</p>
        Click me
      </button>
    </div>
  );
}
```

## Ref

```js
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



